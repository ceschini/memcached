embedding local de cada pixel

TaI-DPT faz isso (ver repo)
acessar a camada e dar um print, antes do global pooling.

qual efeito pra cada heatmap
pega embedding de texto e diz que isso eh o reproduzido na imagem

como funciona com o clip o gradCAM?

fish = pessoa?
normalizar independente do prompt
pra comparar dois prompts.
normalizacao absoluta e nao 

tentar mudar a rotina pra entrar mais de um prompt.

calcular heatmap pra mais de um prompt

antes do normalize pegar o x.max() fixo do maior dos x.

mudar rotina do normalize pra passar um valor

normalizar o gradCAM de cada prompt.

passar o valor de normalizacao como parametro

pegar o maior gradCAM dentre o subconjunto de prompts.

pegar o maior valor dentre todos gradcams


a cat = 0.50903946
a person = 0.10164739
a dog = 0.31106445

TODOS:
avaliar lista de sinonimos
comparar os mapas
colocar preto e branco a saida

fazer testes com essas ferramentas e organizar num documento.

https://colab.research.google.com/github/kevinzakka/clip_playground/blob/main/CLIP_GradCAM_Visualization.ipynb
