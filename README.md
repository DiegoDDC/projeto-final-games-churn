# Projeto Final - Análise de Dados e BI (Games)

Churn e risco de encerramento em jogos digitais no modelo Games as a Service (GaaS).

## Equipe
- Carla Mylena Calixto Rodrigues - RGM 41887867
- Diego Dantas Cavalcanti - RGM 42215285

## Dashboard
Painel interativo (Looker Studio / Data Studio): https://datastudio.google.com/reporting/b23a141c-989b-4d6d-8ade-59a4f450ec05

## Objetivo
Analisar a evolução da base ativa de jogadores no setor de Games e identificar sinais de queda de engajamento que aumentam o risco de encerramento do jogo, propondo recomendações de retenção e monetização baseadas em dados.

Contexto: ao comprar um jogo digital, o consumidor adquire uma licença de uso e não a propriedade. Se o jogo for encerrado, o acesso é perdido.

O projeto indica risco, não prevê encerramento: a base só contém jogos que ainda estão no ranking atual da Steam.

## Perguntas de negócio
1. Quais jogos têm risco alto de perder a base de jogadores?
2. Quanto os jogos já perderam em relação ao seu melhor momento?
3. Existe sazonalidade que não deve ser confundida com sinal de risco?

## Fonte dos dados
- Dataset: Steam Player Data (Kaggle, autor jackogozaly)
- Origem: SteamCharts
- Conteúdo: média e pico mensal de jogadores de 98 jogos do ranking dos mais jogados da Steam
- Período: julho de 2012 a setembro de 2021
- Tamanho original: 5.271 linhas e 8 colunas
- Detalhes e limitações: `docs/ficha_fonte_steam.xlsx`

## Métricas
- Média mensal e pico mensal de jogadores
- Variação percentual mensal da base ativa
- Pico histórico: maior média mensal do jogo até aquele mês
- Taxa de queda (proxy de churn): `(1 - média do mês / pico histórico) x 100`
- Meses consecutivos de queda

## Regra de risco
- **Alto:** 3 ou mais meses seguidos de queda e queda de 50% ou mais em relação ao pico
- **Atenção:** apenas uma das duas condições
- **Estável:** nenhuma das duas

Os limiares são definidos pelo grupo e podem ser ajustados no notebook 02. Resultado em set/2021: 8 jogos em Alto, 30 em Atenção e 44 em Estável.

## Estrutura do repositório
```
projeto-final-games-churn/
├── README.md
├── data/
│   ├── raw/         dados originais do Kaggle, sem alteração
│   └── processed/   base limpa e arquivos de métricas usados no dashboard
├── notebooks/
│   ├── 01_limpeza_steam_players.ipynb
│   └── 02_exploracao_metricas.ipynb
└── docs/
    ├── Documentacao_Projeto_Final_Games.docx
    ├── Explicacao_e_Interpretacao_Projeto_Games.pdf
    ├── Resumo_limpeza_de_dados.docx
    ├── dicionario_dados.xlsx
    └── ficha_fonte_steam.xlsx
```

## Etapas
- [x] Objetivo e métricas definidos
- [x] Fontes identificadas
- [x] Limpeza da base
- [x] Análise exploratória e métricas de risco
- [x] Dashboard interativo
- [x] Apresentação e recomendações

## Limpeza dos dados (resumo)
- Padronização de nomes de colunas e conversão de tipos (datas e `percent_gain`)
- Nomes de jogos: remoção de espaços extras (124 linhas) e correção de erros de digitação do dataset
- Dados sensíveis: verificação de colunas e de padrões de e-mail, CPF e telefone; nenhum encontrado (base pública e agregada)
- Remoção de registros que não são jogos (Spacewar, Wallpaper Engine, Soundpad): 220 linhas
- Duplicados (um registro por jogo por mês): nenhum encontrado
- Remoção de meses com 0 jogadores: 14 linhas
- Nulos: `gain` e `percent_gain` vazios apenas no primeiro mês de cada jogo (esperado); variação recalculada por jogo e sem divergência em relação ao original
- Validação lógica: pico de jogadores maior ou igual à média e sem valores negativos
- Outliers sinalizados por IQR em `outlier_avg` e mantidos na base
- Jogos com menos de 12 meses de histórico removidos: 13 jogos e 66 linhas
- Resultado: 5.271 linhas e 98 jogos na base original; 4.971 linhas e 82 jogos na base limpa

## Limitações
- Viés de sobrevivência: jogos já encerrados não aparecem na base
- Limiares de risco definidos pelo grupo, sem jogos encerrados para validá-los
- Dados apenas da Steam e até 2021
- Conferência de uma amostra com o SteamCharts ainda pendente

## Como executar
1. Abrir `notebooks/01_limpeza_steam_players.ipynb` no Google Colab e enviar `data/raw/Valve_Player_Data.csv` quando solicitado. Gera `steam_players_limpo.csv`.
2. Abrir `notebooks/02_exploracao_metricas.ipynb` no Google Colab e enviar `data/processed/steam_players_limpo.csv`. Gera `steam_metricas_mensal.csv` e `steam_resumo_jogos.csv`, usados no dashboard.
