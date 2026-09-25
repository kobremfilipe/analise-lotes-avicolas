# Análise de desempenho de lotes avícolas

Projeto de Ciência de Dados aplicado à investigação de **mortalidade**, **conversão alimentar (CA)** e **consumo de ração** em 5.813 lotes avícolas.

O trabalho combina análise exploratória, qualidade de dados, benchmarking interno e Machine Learning para identificar sinais precoces de risco e propor uma rotina operacional de acompanhamento dos lotes.

## Problema de negócio

A mortalidade elevada e a piora da conversão alimentar aumentam o consumo de recursos e reduzem a eficiência da operação. O projeto procura responder:

- Quais fatores estão associados à mortalidade e à CA?
- Em qual fase do ciclo as perdas se concentram?
- Existem regiões que podem servir como referência interna?
- É possível identificar lotes de maior risco antes do encerramento do ciclo?
- Como transformar os resultados em uma ação operacional?

## Base de dados

A análise utilizou dados operacionais reais, previamente anonimizados, contendo informações sobre:

- Região, técnico e características da instalação;
- Origem, genética, densidade e alojamento;
- Peso e mortalidade acumulada por semana;
- Ração por fase, sobra de ração e conversão alimentar;
- Peso e quantidade de aves abatidas.

> A base original não é disponibilizada neste repositório por questões de confidencialidade. Os resultados são apresentados de forma agregada e os identificadores utilizados na análise foram anonimizados.

## Etapas do projeto

1. Verificação da estrutura e da qualidade dos dados;
2. Tratamento de valores extremos e fisicamente implausíveis;
3. Análise da relação entre mortalidade, CA e consumo de ração;
4. Comparação de desempenho entre regiões e técnicos;
5. Decomposição da mortalidade acumulada por período;
6. Modelagem da mortalidade final com Random Forest;
7. Avaliação da capacidade preditiva nos dias 7, 14, 21, 28, 35 e 42;
8. Criação de uma proposta de priorização operacional no D+7;
9. Dimensionamento físico da oportunidade frente ao benchmark interno.

## Principais resultados

- A correlação entre `%Mort49` e `CA` foi **0,5007** na base bruta, mas caiu para **0,1433** após a retirada dos casos extremos. Isso mostra que os extremos influenciavam fortemente a relação.
- A `Regiao_13` apresentou a menor CA média do recorte: **1,7483**, além de mortalidade média de aproximadamente **3,34%**.
- A primeira semana concentrou a maior mortalidade incremental média: **1,3263 ponto percentual**, equivalente a aproximadamente 31% da mortalidade final média.
- O modelo baseado apenas em informações disponíveis antes das medições semanais apresentou **R² de 0,0878**.
- Com informações disponíveis no D+7, o R² aumentou para **0,4882**, indicando utilidade para classificação precoce de risco.
- O quartil previsto de maior risco no D+7 apresentou mortalidade final média próxima de **6%**.
- Os 20% dos lotes com maior gap físico concentraram aproximadamente **66,82%** do excesso estimado de ração frente ao benchmark.

## Solução proposta

O projeto recomenda uma **Central de Controle de Lotes Críticos**, formada por:

- Score de risco executado quando o lote completa sete dias;
- Classificação operacional em níveis de prioridade;
- Acompanhamento dos lotes de maior risco pela equipe técnica;
- Investigação das práticas da região de melhor desempenho;
- Monitoramento de mortalidade, gap de CA e sobra de ração;
- Piloto de 90 dias para medir impacto antes da expansão.

O score é utilizado para **ordenar prioridade**, e não para determinar sozinho a causa da mortalidade.

## Estrutura do repositório

```text
analise-lotes-avicolas/
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── analise_consultiva_lotes_avicolas.ipynb
├── data/
│   └── README.md
├── images/
└── reports/
```

## Notebook principal

- [Análise consultiva dos lotes avícolas](notebooks/analise_consultiva_lotes_avicolas.ipynb): preparação e qualidade dos dados, investigação de hipóteses, benchmarks, modelos preditivos, priorização D+7, dimensionamento da oportunidade física e proposta de implantação.

## Tecnologias utilizadas

- Python;
- pandas e NumPy;
- Matplotlib;
- scikit-learn;
- Jupyter Notebook;
- Excel.

## Como executar

Clone o repositório e instale as dependências:

```bash
git clone URL_DO_REPOSITORIO
cd analise-lotes-avicolas
pip install -r requirements.txt
```

Para uma reprodução autorizada, a planilha deve ser inserida na pasta `data` com o nome esperado pelos notebooks. Sem a base confidencial, os notebooks podem ser consultados com as saídas previamente registradas, mas não reexecutados integralmente.

## Limitações

- Os resultados representam associações observacionais, não relações causais comprovadas.
- Rankings regionais podem ser influenciados pelo perfil dos produtores, instalações e períodos analisados.
- A avaliação preditiva deve ser complementada por validação temporal antes de uso em produção.
- As estimativas de ração e aves representam um gap físico teórico frente ao benchmark, não uma economia financeira garantida.
- Preços, custos de intervenção e margem operacional não estavam disponíveis.

## Autor

**Filipe Kobrem dos Santos**  
Estudante de Ciência de Dados — PUC-SP
