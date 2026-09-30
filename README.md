# Preço de Kits LEGO com Redes Neurais Artificiais

Laboratório de Inteligência Artificial (PUC-SP) que usa uma rede neural
artificial (`MLPRegressor`, scikit-learn) para **completar o preço dos kits
LEGO que não têm preço** e descobrir **quais colunas mais pesam na previsão**,
a partir dos kits lançados entre 1970 e 2022.

**Dupla**

| Integrante | RA |
| --- | --- |
| Kauã Cavalheiro | RA00331660 |
| João Avila | RA00332195 |

## Como executar

```bash
python -m venv .venv && source .venv/bin/activate   # opcional
pip install -r requirements.txt
jupyter notebook PUCSP_CS_AI_10_AI_ML_ANN_LEGO_LAB.ipynb
```

Rode **Kernel → Restart & Run All** antes de salvar/publicar, para garantir
que o notebook roda do início ao fim em um kernel limpo.

> **Nota sobre os dados.** O notebook baixa os dados reais da *Maven Analytics*
> com os comandos `!wget` e `!unzip` (magics do Jupyter, que dependem de um
> terminal Linux/macOS ou de um ambiente como o Google Colab). Se a rede
> estiver indisponível ou o link mudar, a primeira etapa falha e o restante do
> notebook não roda. Nesse caso, baixe o ZIP manualmente em
> https://mavenanalytics.io/data-playground/lego-sets e coloque o arquivo
> `lego_sets.csv` na mesma pasta do notebook.

## Principais figuras e conclusão

- **Valores faltantes** (`missingno`: matriz, barras e heatmap) — mostra onde
  estão os nulos antes e depois da limpeza; ao final, só o preço permanece
  incompleto, porque são justamente os kits a completar.
- **Real vs. previsto e resíduos** — avalia a ANN no conjunto de teste
  (R² = 0,858; MAE = US$ 8,06; RMSE = US$ 21,32).
- **Importância das colunas** (permutation importance + correlação de
  Spearman) — responde à pergunta do laboratório sobre quais colunas importam
  para prever o preço.
- **Preços reais vs. imputados** — compara as distribuições e a relação com o
  número de peças, separando dado real de dado previsto.
- **Preço médio por ano e por grupo de tema** — mostra a evolução dos preços
  e quais grupos de kits são mais caros.

**Conclusão em poucas linhas:** dos 18.457 kits, 6.982 tinham preço e 11.475
não; a ANN completou todos eles (`price`, com a coluna `price_is_predicted`
marcando a origem). A quantidade de **peças** é, de longe, a coluna mais
importante (o MAE sobe US$ 24,11 ao embaralhá-la), seguida por **subtheme**
(US$ 6,61) e **theme** (US$ 5,40). Por grupo de tema, *Educational* tem o
maior preço médio (cerca de US$ 118) e *Girls* o menor (cerca de US$ 11).

## Limitações

- O modelo aprende só com os kits que **têm** preço (cerca de 38% do total) e
  prevê os outros 62%. Se os kits sem preço forem diferentes dos demais (por
  exemplo, mais antigos), as previsões podem ser enviesadas.
- Os preços imputados são **estimativas**, não preços reais de venda: as médias
  por ano e por tema misturam os dois tipos, e por isso o notebook também
  mostra a versão só com preços reais.
- `US_retailPrice` é o preço de varejo nos EUA e não foi corrigido pela
  inflação, o que dificulta comparar preços de décadas diferentes.
- O nº de peças ausente foi preenchido pela mediana da categoria (com o
  indicador `pieces_missing`), e as previsões foram limitadas à faixa de preços
  reais observados (US$ 1,49 a US$ 849,99; 2 previsões foram ajustadas ao
  teto).
- A importância por permutação pode dividir o crédito entre colunas
  correlacionadas (ex.: `theme`, `subtheme` e `themeGroup`), e a avaliação usa
  uma única divisão treino/teste (80/20), sem validação cruzada.
- A coluna `agerange_min` foi descartada por ter 63,2% de valores nulos, então
  a faixa etária recomendada não entra no modelo.

## Fontes citadas

- **Maven Analytics** — *LEGO Sets* (kits lançados de 1970 a 2022, com tema,
  peças, idade recomendada, preço de varejo e imagem),
  https://mavenanalytics.io/data-playground/lego-sets, arquivo baixado de
  https://maven-datasets.s3.amazonaws.com/LEGO+Sets/LEGO+Sets.zip.
- **scikit-learn** — `MLPRegressor`, `ColumnTransformer`,
  `TransformedTargetRegressor` e `permutation_importance`,
  https://scikit-learn.org/stable/.

## Estrutura do repositório

```text
IA-LEGO/
├── README.md
├── requirements.txt
├── PUCSP_CS_AI_10_AI_ML_ANN_LEGO_LAB.ipynb
```
