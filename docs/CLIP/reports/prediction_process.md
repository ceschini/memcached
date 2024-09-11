# Processo de Predição com CLIP

## 1. Gerando Embeddings

### Embeddings de Texto

Pega a lista de prompts

```python
txt_list = [
	"a photo of a white man",
	"a photo of a white woman",
	"a photo of a black man", 
	"a photo of a black woman",
	...
]
```

Concatena a lista dos prompts "tokenizados":

```python
text_inputs = torch.cat([tokenizer(c) for c in txt_list])
```

Codifica a lista usando o modelo:

```python
text_features = model.encode_text(text_inputs)
```

Normaliza a lista(?):

```python
text_features /= text_features.norm(dim=-1, keepdim=True)
```

### Embeddings de imagem

Abre a imagem com Pillow e pre-processa:

```python
img = Image.open(filepath)
img_input = model_preprocessing(img).unsqueeze(0)
```

Codifica a imagem usando o modelo:

```python
image_features = model.encode_image(img_input)
```

Normaliza a imagem (?):

```python
image_features /= image_features.norm(dim=-1, keepdim=True)
```

## 2. Pegando as similaridades

```python
img_similarity = 100 * image_features @ text_features.T
```

Relaciona cada prompt com o seu "score" de similaridade:

```python
for label, score in zip(txt_prompts, img_similarity[0]):
	sim_dict[label] = score.cpu().numpy().item()
```

Resultado:

```json
{
	"image.jpg": {
		"a photo of a white man": 20.84195327758789,
		"a photo of a white woman": 13.337924003601074,
		"a photo of a black man": 16.401159286499023,
		"a photo of a black woman": 9.255730628967285,
		"a photo of a latino hispanic man": 22.144479751586914,
		"a photo of a latino hispanic woman": 14.342647552490234,
		"a photo of an east asian man": 24.513031005859375,
		"a photo of an east asian woman": 16.19273567199707,
		"a photo of a southeast asian man": 26.074390411376953,
		"a photo of a southeast asian woman": 17.766008377075195,
		"a photo of an indian man": 17.086769104003906,
		"a photo of an indian woman": 8.459698677062988,
		"a photo of a middle eastern man": 18.602418899536133,
		"a photo of a middle eastern woman": 10.164729118347168
	}
}
```

## 3. Realizando Predições

Com base no dicionário de similaridades acima, realiza os procedimentos abaixo para cada uma das imagens.

### Average Sum

Primeiro cria um dicionário de pontuações vazio com cada raça:

```python
race_dict = {
	"White": 0,
	"Black": 0,
	"Indian": 0,
	"Latino_Hispanic": 0,
	"Southeast Asian": 0,
	"East Asian": 0,
	"Middle Eastern": 0
}
```

Identifica a raça correspondente e soma os "scores" de similaridade:

```python
for prompt, sim_score in sims_dict:
	race_label = get_race_from_prompt(prompt)
	race_dict[race_label] += sim_score
```

pega o maior valor e salva como a predição:

```python
prediction = max(race_dict, key=race_dict.get)
```

### Top K

Primeiro cria um dicionário de pontuações e um dicionário de scores por raça.

```python
race_dict = {
	"White": 0,
	"Black": 0,
	"Indian": 0,
	"Latino_Hispanic": 0,
	"Southeast Asian": 0,
	"East Asian": 0,
	"Middle Eastern": 0
}

race_list = {
	"White": [],
	"Black": [],
	"Indian": [],
	"Latino_Hispanic": [],
	"Southeast Asian": [],
	"East Asian": [],
	"Middle Eastern": []
}
```

Indentifica a raça e salva seu score na lista correspondente:

```python
for prompt, sim_score in sims_dict:
	race_label = get_race_from_prompt(prompt)
	race_list[race_label].append(sim_score)
```

Para cada uma das raças, ordena a lista de scores em ordem decrescente e soma os top K valores:

```python
for race in race_dict:
	scores = sorted(race_list[race], reverse=True)
	race_dict[race] = sum(scores[:k])
```

pega o maior valor e salva como a predição:

```python
prediction = max(race_dict, key=race_dict.get)
```
