# Aurora

## Problema uint8 e estouro de RAM na inferencia

se o dtype nao for uint16 nem uint32, ele cai nisso:

![](Pasted%20image%2020260106155448.png)

6435672_eyesea_log.log

imagem do erro: 

S1A_IW_GRDH_1SDV_20260105T080938_20260105T081003_062629_07D9DE_9CFF.tif

```bash
Traceback (most recent call last):  
  File "/code/run.py", line 13, in <module>  
    oil_spotter.run()  
  File "/code/src/oil_spotter.py", line 93, in run  
    map_inference, y_contours, probs = self.patch_inference(img_path)  
  File "/code/src/oil_spotter.py", line 75, in patch_inference  
    y_contours, probs = model_classifier.predict(reconstructed_heatmp)  
  File "/code/src/predictors.py", line 477, in predict  
    contours, y_contours = self.get_inferences_and_classes(heatmap)  
  File "/code/src/predictors.py", line 447, in get_inferences_and_classes  
    y_contours = np.stack(y_contours, axis=0)  
  File "<__array_function__ internals>", line 180, in stack  
  File "/usr/local/lib/python3.8/dist-packages/numpy/core/shape_base.py", line 433, in stack  
    return _nx.concatenate(expanded_arrays, axis=axis, out=out)  
  File "<__array_function__ internals>", line 180, in concatenate  
numpy.core._exceptions.MemoryError: Unable to allocate 630. GiB for an array with shape (523, 11191, 14452) and data type float64  
double free or corruption (out)
```

isso ta acontecendo antes de chegar na parte que mexi, sobre os dtypes

imagem que deu erro no meu codigo de dtypes:

S1A_IW_GRDH_1SDV_20260105T080938_20260105T081003_062629_07D9DE_9CFF

## Outro erro loko da inferencia

**log de erro**: 6437555_eyesea_log.log

**imagem**: CSKS4_GEC_B_HR_02_VV_RD_FF_20260107205114_20260107205145.MBI

\*acho\* que é na hora do rasterio carregar a img.

pesquisando os erros pode ser do libtiff ou do GDAL.

```bash
Input file size is 10220, 10090  
0...ERROR 1: TIFFReadEncodedStrip:Read error at scanline 4294967295; got 6268 bytes, expected  
10220  
ERROR 1: TIFFReadEncodedStrip() failed.  
ERROR 1: CSKS4_GEC_B_HR_02_VV_RD_FF_20260107205114_20260107205145.MBI_light_notoptimized.tif, band 1: IReadBlock failed at X offset 0, Y offset 2282: TIFFReadEncodedStrip() failed.  
ERROR 1: _tiffSeekProc:Stale file handle  
ERROR 1: TIFFAppendToStrip:Maximum TIFF file size exceeded. Use BIGTIFF=YES creation option.  
ERROR 1: _tiffSeekProc:Stale file handle  
ERROR 1: TIFFAppendToStrip:Maximum TIFF file size exceeded. Use BIGTIFF=YES creation option.  
ERROR 1: _tiffSeekProc:Stale file handle
...
ERROR 1: TIFFAppendToStrip:Maximum TIFF file size exceeded. Use BIGTIFF=YES creation option.
More than 1000 errors or warnings have been reported. No more will be reported from now.
```

Depois disso ele diz que finalizou com sucesso, mas no final não gera nada.
## Evento duplicado watchdog

Quando copiamos uma nova imagem, solta, é disparado duas vezes, provavelmente assim que ela chega, com um tamanho inicial de 0kb (só um ponteiro?), e depois que ela termina de ser copiada, com seu tamanho final.

