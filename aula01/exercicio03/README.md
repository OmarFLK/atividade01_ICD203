# Exercício 3 — pesquisas eleitorais

Este projeto, desenvolvido por Andrey Salomão Almeida e Omar Habdalah Halada Saliba, analisa pesquisas eleitorais públicas com foco em estatística descritiva e na comparação entre estimativas e resultados reais.

## Arquivos principais

- `ex3.ipynb`: notebook da atividade.
- `GUIA_APRESENTACAO.md`: roteiro e explicação detalhada da análise.
- `fontes_eleicoes_manifest.json`: catálogo das fontes planejadas.
- `data/raw/`: arquivos originais, preservados sem alterações.
- `data/processed/`: arquivos produzidos durante a limpeza.
- `data/final/`: tabelas finais usadas nas comparações.

## Fontes já validadas e baixadas

- TSE/PesqEle: registros de pesquisas de 2014, 2018, 2022 e 2026.
- FiveThirtyEight: `raw_polls.csv`, documentação e tabela de avaliação dos institutos, obtidos do repositório oficial no GitHub.
- AtlasIntel/Bloomberg: cenário nacional de julho de 2026, tratado separadamente da comparação histórica.

## Análises realizadas

- Estatísticas descritivas dos registros brasileiros, incluindo amostra, custo e duração da coleta.
- Padronização de institutos e classificação resumida das metodologias.
- Comparação histórica por erro assinado, erro absoluto, MAE e RMSE.
- Análise por instituto, ano, distância até a eleição e tamanho da amostra.
- Análise específica da AtlasIntel nos Estados Unidos e nos registros brasileiros do TSE.
- Projeção exploratória do cenário brasileiro de 2026 com ajuste pelo viés histórico.
- Simulação de primeiro turno e intervalo exploratório baseado no MAE histórico.
- Ranking baseado na pesquisa mais próxima de cada eleição.
- Gráficos comparativos e exportação das tabelas tratadas e finais.

## Organização e reprodução

Execute o notebook a partir desta pasta para que os caminhos relativos em `data/` sejam encontrados. O projeto utiliza `pandas`, `numpy`, `matplotlib` e `pathlib`.

Os datasets em CSV e ZIP fazem parte do repositório para permitir a reprodução da análise. Os arquivos PDF são referências auxiliares mantidas apenas localmente.

## Limitações encontradas

- O PesqEle registra instituto, datas, amostra e metodologia, mas não oferece em uma única coluna os percentuais de intenção de voto de cada candidato.
- O antigo endereço `president_polls_historical.csv` do FiveThirtyEight atualmente devolve uma página HTML, e não o CSV esperado. Por isso foi usado o `raw_polls.csv` oficial.
- O registro do Europepolls no Zenodo está acessível, mas seus arquivos aparecem como restritos. Ele não foi usado nesta primeira versão.
- A análise brasileira de 2026 ainda não pode ser tratada como previsão: a eleição não ocorreu e os percentuais dos candidatos precisam ser obtidos nos relatórios de cada pesquisa.
- A projeção de 2026 usa somente o histórico da AtlasIntel nos Estados Unidos em 2020; portanto, serve para contextualização exploratória e não como previsão eleitoral.

Essas lacunas são documentadas para que nenhum valor seja inventado ou apresentado como se viesse diretamente das bases.
