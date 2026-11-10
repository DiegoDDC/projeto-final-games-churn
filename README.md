# Projeto Final - Análise de Dados e BI (Games)

Churn e monetização em jogos digitais no modelo Games as a Service (GaaS).

## Equipe
- Carla Mylena Calixto Rodrigues - RGM 41887867
- Diego Dantas Cavalcanti - RGM 42215285

## Objetivo
Analisar a evolução da base ativa de jogadores no setor de Games e identificar sinais de queda de engajamento que aumentam o risco de encerramento do jogo, propondo recomendações de retenção e monetização baseadas em dados.

Contexto: ao comprar um jogo digital, o consumidor adquire uma licença de uso e não a propriedade. Se o jogo for encerrado, o acesso é perdido.

## Fonte dos dados
- Dataset: Steam Player Data (Kaggle, autor jackogozaly)
- Origem: SteamCharts
- Conteúdo: média e pico mensal de jogadores dos 100 principais jogos da Steam
- Período: 2012 a 2021
- Tamanho original: 5.271 linhas e 8 colunas
- Detalhes e limitações: `docs/ficha_fonte_steam.xlsx`

## Métricas
- Média mensal de jogadores
- Pico mensal de jogadores
- Variação percentual mensal da base ativa
- Taxa de queda (proxy de churn): perda em relação ao pico do jogo
- Meses consecutivos de queda

## Estrutura do repositório
```
projeto-final-games-churn/
├── README.md
├── data/
│   ├── raw/         dados originais do Kaggle, sem alteração
│   └── processed/   base limpa (steam_players_limpo.csv)
├── notebooks/
│   └── 01_limpeza_steam_players.ipynb
└── docs/
    ├── Documentacao_Projeto_Final_Games.docx
    ├── dicionario_dados.xlsx
    └── ficha_fonte_steam.xlsx
```

## Etapas
- [x] Objetivo e métricas definidos
- [x] Fontes identificadas
- [x] Limpeza da base
- [ ] Análise exploratória
- [ ] Dashboard interativo
- [ ] Apresentação executiva e recomendações

## Limpeza dos dados (resumo)
- Padronização de nomes de colunas e conversão de tipos
- Remoção de duplicados (um registro por jogo por mês)
- Nulos: remoção de linhas sem campos obrigatórios; `gain` e `percent_gain` recalculados por jogo
- Validação lógica: pico de jogadores maior ou igual à média
- Outliers sinalizados por IQR em `outlier_avg` e mantidos na base
- Jogos com menos de 12 meses de histórico removidos
- Resultado: 5.205 linhas e 85 jogos

## Como executar
Abrir `notebooks/01_limpeza_steam_players.ipynb` no Google Colab e enviar o arquivo de `data/raw/` quando solicitado.
