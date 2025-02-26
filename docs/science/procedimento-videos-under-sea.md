
# Procedimento para criar os vídeos em baixa resolução

Nosso objetivo aqui é transformar vídeos em alta resolução e qualidade, em vídeos de baixa resolução e baixa qualidade, com as mesmas restrições impostas pela simulação de transmissão hidro acústica.

## Passos para realizar a transformação

### 1. Renomear frames

Nossos dados de entrada eram frames no formato PNG sem uma nomenclatura padronizada, então para transformá-los em vídeos precisamos antes renomeá-los de acordo. O script python abaixo foi utilizado para isso.

```python
# rename-imgs.py

"""Rename images of a given folder to sequential zfilled numbers"""
import os
import sys


def rename_imgs(path):
    """Rename images of given path"""
    im_list = os.listdir(path)
    ext = im_list[0].split("/")[-1].split(".")[1]
    im_list = list(map(lambda x: x.split("/")[-1].split(".")[0], im_list))
    im_len = len(im_list)
    for i, im in enumerate(im_list, start=1):
        os.system('clear')
        print(f"Renaming {i} of {im_len}")
        curr_im = f"{path}/{im}.{ext}"
        new_im = f"{path}/im{str(i).zfill(4)}.{ext}"
        os.rename(curr_im, new_im)


if __name__ == "__main__":
    rename_imgs(sys.argv[1])
```

Para utilizá-lo basta chamar `python rename-imgs.py /caminho/das/imagens`. Com esse script, imagens foram de `ZdhsXc.png, Sdxgkm.png...` para `im0001.png, im0002.png`.

### 2. Juntar em vídeo

Agora que temos os frames em sequencia, podemos juntá-los em um vídeo, para isso utilizamos uma imagem singularity com FFMPEG, e agendamos um novo "job" para o slurm do cluster, usando o comando `sbatch make_vid.srm`. O script abaixo realiza a criação dos vídeos.

```bash
# make_vid.srm

#!/bin/sh
#SBATCH --nodes=1
#SBATCH --partition=gpu
#SBATCH --ntasks=1
#SBATCH --mem=7500
#SBATCH --gres=gpu:1
#SBATCH --oversubscribe
#SBATCH --ntasks-per-node=1
#SBATCH -J make_vid.srm

singularity run \
        -B /share_zeta/UnderTheSea/lucasmc/data/sr-train/originals:/originals \
        /share_zeta/UnderTheSea/singularities/ffmpeg.sif \
        -f image2 -framerate 1 \
        -i /originals/plem_e_plet/im%04d.png \
        -f rawvideo -pix_fmt yuv420p \
        -s 640x320 /originals/plem_e_plet.yuv
```

`-B` mapeia a pasta de origem para a pasta de destino no formato `origem:destino`. A pasta originals contem as demais pastas com os frames anteriormente renomeados. A próxima linha informa qual a imagem singularity que vamos utilizar, nesse caso a `ffmpeg.sif`. Os demais parâmetros são configurações do FFMPEG, para um vídeo de 1 frame por segundo, tendo como entrada (`-i`) a pasta `/originals/plem_e_plet`, onde os frames estão ordenados seguindo o padrão `im%04d`, isso é: `im0001`, `im0002`, etc. 

### 3. Transformar vídeo para baixa resolução/qualidade

Agora que temos o vídeo dos frames em alta qualidade e resolução, vamos usar o FFMPEG para convertê-lo em baixa resolução e qualidade, usando o **H.264** e as configurações adequadas.

```bash
# stream_vid.srm

#!/bin/sh
#SBATCH --nodes=1
#SBATCH --partition=gpu
#SBATCH --ntasks=1
#SBATCH --mem=7500
#SBATCH --gres=gpu:1
#SBATCH --oversubscribe
#SBATCH --ntasks-per-node=1
#SBATCH -J stream_vid.srm

singularity run \
        -B /share_zeta/UnderTheSea/lucasmc/data/sr-train/originals:/originals \
        /share_zeta/UnderTheSea/singularities/ffmpeg.sif \
        -pix_fmt yuv420p \
        -video_size 640x320 \
        -i /originals/anm.yuv \
        -preset ultrafast \
        -vcodec libx264 \
        -tune zerolatency \
        -r 1 \
        -maxrate 80k \
        -bufsize 320k \
        /originals/anm-stream.yuv

```

O processo é parecido com o script acima, porém aqui estamos dando como entrada para o FFMPEG o vídeo `/originals/anm.yuv`, e esperamos a saída em `/originals/anm-stream.yuv`. Precisamos passar o `video_size` pois o FFMPEG não tem como inferir a partir do formato `yuv`, especificamos o H.264 como a biblioteca `libx264`, no parâmetro `vcodec`, e restringimos a qualidade nas demais linhas.
