# Guia de apresentação — Exercício 3

## 1. Resumo do trabalho

Este projeto analisa pesquisas eleitorais com duas finalidades:

1. estudar os registros públicos de pesquisas presidenciais do Brasil; e
2. comparar pesquisas históricas com resultados eleitorais reais para medir o tamanho dos erros.

Também apresentamos um gráfico com um cenário atual da eleição presidencial brasileira de 2026.

O notebook principal é o arquivo `ex3.ipynb`. Ele foi escrito com comandos básicos e usa apenas `pandas`, `numpy`, `matplotlib` e `pathlib`.

## 2. Fala curta para a apresentação

Uma forma simples de apresentar o trabalho é:

> Nosso grupo escolheu o tema das pesquisas eleitorais porque elas ajudam a acompanhar a opinião pública e influenciam o debate durante uma eleição. Primeiro, limpamos registros públicos do TSE para conhecer a quantidade de pesquisas, seus custos, amostras e metodologias. Depois, usamos uma base histórica que já relaciona pesquisas a resultados reais para calcular erros e comparar institutos. Por fim, mostramos um cenário atual do Brasil. É importante lembrar que uma pesquisa é uma fotografia do período da coleta, e não uma previsão garantida do resultado.

## 3. Por que essa análise é importante

Pesquisas eleitorais são usadas por eleitores, partidos, jornalistas e pesquisadores para acompanhar uma campanha. Analisar esses dados ajuda a entender como a opinião pública muda, quais métodos são usados, quantas pessoas são entrevistadas e quão próximas as estimativas anteriores ficaram do resultado real.

Essa análise também ajuda a evitar interpretações apressadas. Uma diferença pequena pode estar dentro da margem de erro, e uma pesquisa feita hoje pode mudar até o dia da votação. Por isso, o correto é observar a data, a amostra, a metodologia e o histórico, sem tratar uma pesquisa isolada como certeza.

## 4. Bases e função de cada uma

### TSE — PesqEle

Foram usados os arquivos públicos de 2014, 2018, 2022 e 2026. Eles informam, entre outras coisas:

- número de registro;
- data da pesquisa;
- empresa responsável;
- cargo pesquisado;
- tamanho da amostra;
- valor da pesquisa;
- metodologia; e
- contratante.

Os arquivos do TSE servem para estudar como as pesquisas foram registradas no Brasil. Eles não oferecem uma tabela simples e padronizada com o percentual de intenção de voto de cada candidato. Alguns detalhes podem aparecer em questionários ou documentos separados, o que dificulta uma comparação automática dos resultados de todos os anos.

### FiveThirtyEight

A base histórica do FiveThirtyEight reúne pesquisas eleitorais dos Estados Unidos e os resultados reais ligados a elas. Ela foi usada porque já possui, na mesma tabela, o percentual pesquisado e o percentual obtido na eleição. Isso permite calcular o erro de cada instituto de modo direto.

Os dados norte-americanos não representam o eleitorado brasileiro. Eles são usados somente como exercício estatístico de comparação entre pesquisa e resultado.

### MIT Election Data and Science Lab

É a referência indicada para os resultados eleitorais oficiais usados na base histórica.

### AtlasIntel/Bloomberg — julho de 2026

O gráfico atual usa um cenário estimulado de primeiro turno da pesquisa nacional AtlasIntel/Bloomberg. Foram entrevistadas 5.021 pessoas entre 22 e 27 de julho de 2026. A margem de erro informada é de 1 ponto percentual e o registro no TSE é BR-08602/2026.

Essa pesquisa é mostrada separadamente. Ela não participa do cálculo do ranking histórico e não é uma previsão criada pelo grupo.

## 5. Organização dos arquivos

Os arquivos originais foram preservados em `data/raw`, sem alteração. Isso permite voltar à fonte caso seja necessário conferir alguma informação.

- `data/raw`: bases originais baixadas.
- `data/processed`: bases depois da limpeza.
- `data/final`: tabelas finais do ranking histórico.

Essa divisão evita misturar os dados originais com os dados modificados pelo código.

## 6. Conceitos usados

### Média

Soma dos valores dividida pela quantidade de observações.

### Mediana

Valor central depois de ordenar os dados. Ela sofre menos influência de valores muito altos ou muito baixos.

### Desvio-padrão

Indica o quanto os valores variam em relação à média.

### Erro assinado

É calculado assim:

`percentual da pesquisa - percentual real`

- Valor positivo: a pesquisa superestimou o candidato.
- Valor negativo: a pesquisa subestimou o candidato.

### Erro absoluto

É o tamanho do erro sem considerar o sinal. Exemplo: erros de `-3` e `+3` viram `3` pontos percentuais.

### MAE

É a média dos erros absolutos. Quanto menor, mais próximas as pesquisas ficaram dos resultados nesse conjunto de dados.

### RMSE

Também mede o erro, mas dá peso maior aos erros grandes. Quanto menor, melhor foi a aproximação no conjunto analisado.

### Correlação

Varia de `-1` a `1` e indica se duas variáveis tendem a mudar juntas. Correlação não prova que uma variável causou a outra.

## 7. Explicação do código, parte por parte

As instruções que foram divididas em várias linhas no notebook formam um único comando. Parênteses, colchetes, vírgulas e linhas em branco apenas organizam a escrita. A explicação abaixo cobre cada comando executado.

### Célula 1 — bibliotecas e pastas

- `from pathlib import Path`: importa uma ferramenta simples para trabalhar com caminhos de arquivos.
- `import numpy as np`: importa o NumPy com o apelido `np`. Ele é usado em cálculos e valores ausentes.
- `import pandas as pd`: importa o pandas com o apelido `pd`. Ele lê, limpa e resume tabelas.
- `import matplotlib.pyplot as plt`: importa a parte do Matplotlib usada para criar gráficos.
- `pd.set_option("display.max_columns", 50)`: permite mostrar até 50 colunas quando uma tabela é exibida.
- `plt.style.use(...)`: escolhe um estilo visual limpo para os gráficos.
- `RAW = Path("data/raw")`: guarda o caminho dos arquivos originais.
- `PROCESSED = Path("data/processed")`: guarda o caminho dos arquivos limpos.
- `FINAL = Path("data/final")`: guarda o caminho dos resultados finais.
- `PROCESSED.mkdir(...)`: cria a pasta de dados limpos se ela não existir.
- `FINAL.mkdir(...)`: cria a pasta de resultados finais se ela não existir.

### Célula 2 — leitura da base histórica

- `arquivo_538 = RAW / ...`: monta o caminho do CSV histórico.
- `pd.read_csv(...)`: lê o CSV e transforma seu conteúdo em uma tabela do pandas.
- `low_memory=False`: pede ao pandas que examine o arquivo completo antes de decidir os tipos das colunas.
- `print(...shape)`: mostra a quantidade de linhas e colunas da base.
- `pesquisas_brutas.head()`: mostra as cinco primeiras linhas para uma conferência inicial.

### Célula 3 — diagnóstico da base histórica

- `pesquisas_brutas.info()`: mostra nomes das colunas, tipos e quantidade de valores preenchidos.
- `duplicated().sum()`: conta linhas totalmente repetidas.
- A lista entre colchetes escolhe apenas as colunas principais.
- `isna().sum()`: conta quantos valores ausentes existem em cada coluna.
- `to_frame("ausentes")`: transforma essa contagem em uma tabela chamada `ausentes`.
- `display(...)`: exibe a tabela no notebook.

### Célula 4 — recorte e limpeza da base histórica

- `institutos_escolhidos = [...]`: cria a lista dos quatro institutos estudados.
- `anos_escolhidos = [...]`: define as eleições de 2012, 2016 e 2020.
- `type_simple.eq("Pres-G")`: seleciona somente eleições presidenciais gerais.
- `cycle.isin(anos_escolhidos)`: mantém somente os anos escolhidos.
- `pollster.isin(institutos_escolhidos)`: mantém somente os quatro institutos.
- O símbolo `&` exige que as três condições sejam verdadeiras ao mesmo tempo.
- `pesquisas_brutas.loc[filtro].copy()`: cria uma cópia apenas das linhas selecionadas.
- `replace(...)`: abrevia `Marist College` para `Marist`.
- As duas chamadas de `pd.to_datetime(...)` transformam as datas, e `errors="coerce"` converte datas inválidas em valores ausentes.
- `colunas_numericas = [...]`: lista as colunas que precisam ser números.
- `for coluna in colunas_numericas`: repete o próximo comando para cada coluna da lista.
- `pd.to_numeric(...)`: converte texto em número; valores inválidos viram ausentes.
- `duplicated().sum()`: guarda a quantidade de duplicatas antes da remoção.
- `drop_duplicates()`: remove linhas totalmente repetidas.
- A subtração `electiondate - polldate` calcula o tempo entre a pesquisa e a eleição.
- `.dt.days`: transforma esse intervalo em número de dias.
- O filtro `days_to_election >= 0` remove datas posteriores à eleição.
- Os dois `print`: informam quantas duplicatas foram removidas e o tamanho final do recorte.

### Célula 5 — um candidato por linha

- `colunas_comuns = [...]`: lista as informações que são iguais para os dois candidatos da pesquisa.
- O primeiro recorte escolhe essas colunas e os dados do candidato 1.
- O primeiro `rename(...)` troca nomes como `cand1_pct` por nomes gerais como `poll_pct`.
- O segundo recorte e o segundo `rename(...)` fazem a mesma coisa para o candidato 2.
- `pd.concat(...)`: coloca as duas tabelas uma embaixo da outra.
- `ignore_index=True`: cria uma nova numeração de linhas.
- `head()`: exibe as primeiras linhas da tabela reorganizada.

### Célula 6 — cálculo dos erros

- `signed_error = poll_pct - actual_pct`: calcula quanto a pesquisa ficou acima ou abaixo do resultado.
- `.abs()`: retira o sinal e cria o erro absoluto.
- `source_url`: registra o link da fonte das pesquisas.
- `official_result_url`: registra o link da fonte dos resultados eleitorais.
- `margin_error = margin_poll - margin_actual`: compara a vantagem apontada pela pesquisa com a vantagem real.
- `.abs()`: calcula o tamanho absoluto desse erro de margem.
- `np.sign(...)`: verifica se a margem é positiva ou negativa.
- A comparação com `==` cria `winner_correct`: verdadeiro quando a pesquisa e o resultado apontam o mesmo lado vencedor.
- O último bloco conta valores ausentes nas colunas essenciais após a limpeza.

### Célula 7 — salvamento dos dados históricos

- O primeiro `to_csv(...)` salva uma linha por pesquisa.
- O segundo `to_csv(...)` salva uma linha por candidato.
- `index=False`: não grava a numeração interna do pandas no CSV.
- `print(...)`: confirma que os arquivos foram salvos.

### Célula 8 — leitura dos arquivos do TSE

- `arquivos_tse = {...}`: relaciona cada ano ao caminho de seu arquivo CSV.
- `partes_tse = []`: cria uma lista vazia para receber cada ano.
- `colunas_tse = [...]`: define somente as colunas úteis para o estudo.
- `for ano, caminho in arquivos_tse.items()`: repete a leitura para cada ano.
- `pd.read_csv(...)`: lê o arquivo; `sep=";"` informa o separador e `encoding="latin1"` permite ler os acentos desses CSVs.
- `rename(...)`: corrige nomes de colunas que mudaram entre anos.
- O segundo `for` verifica se cada coluna esperada existe.
- `if coluna not in parte.columns`: identifica uma coluna ausente.
- `parte[coluna] = np.nan`: cria a coluna vazia para manter todos os anos com o mesmo formato.
- `parte[colunas_tse].copy()`: mantém apenas as colunas escolhidas.
- `parte["ano_arquivo"] = ano`: registra de qual arquivo veio cada linha.
- `partes_tse.append(parte)`: adiciona a tabela daquele ano à lista.
- `pd.concat(...)`: une os quatro anos em uma tabela.
- `print(...shape)`: mostra a dimensão da tabela reunida.
- `head()`: mostra as primeiras linhas.

### Célula 9 — diagnóstico dos dados do TSE

- `info()`: mostra tipos, colunas e preenchimento.
- `duplicated().sum()`: conta linhas totalmente repetidas.
- `NR_PROTOCOLO_REGISTRO.duplicated().sum()`: conta números de protocolo repetidos.
- `isna().sum()`: conta valores ausentes em todas as colunas.
- `display(...)`: mostra a contagem como tabela.

### Célula 10 — limpeza do TSE

- `str.contains("Presidente", ...)`: mantém somente registros cujo cargo contém a palavra `Presidente`.
- `case=False`: ignora diferença entre maiúsculas e minúsculas.
- `na=False`: trata valores ausentes como não correspondentes.
- `astype(str)`: transforma o nome da empresa em texto.
- `str.strip()`: remove espaços no começo e no fim.
- `str.replace(r"\s+", " ", regex=True)`: troca vários espaços seguidos por um espaço.
- `instituto = instituto_original`: cria a coluna que receberá os nomes padronizados.
- `str.upper()`: cria uma versão em letras maiúsculas para facilitar a busca.
- As três instruções com `tse.loc[...]` padronizam variações dos nomes AtlasIntel, IBOPE e Ipec.
- O `for` percorre as três colunas de data.
- `pd.to_datetime(...)`: converte essas colunas para data.
- `dayfirst=True`: interpreta o dia antes do mês.
- `errors="coerce"`: datas inválidas viram ausentes.
- `pd.to_numeric(...)`: transforma a quantidade de entrevistados em número.
- A linha com `tse.loc[...] = np.nan` transforma amostras impossíveis, menores ou iguais a zero, em valor ausente.
- Os dois `str.replace(...)` retiram o ponto de milhar e trocam a vírgula decimal por ponto.
- O segundo `pd.to_numeric(...)` converte o valor da pesquisa para número.
- `DT_FIM_PESQUISA - DT_INICIO_PESQUISA`: calcula a duração da coleta.
- `.dt.days + 1`: converte para dias e inclui o dia inicial.
- `duplicated().sum()`: conta protocolos repetidos.
- `drop_duplicates(subset=...)`: mantém somente uma linha de cada protocolo.
- Os dois `print`: mostram a quantidade removida e o tamanho da base limpa.

### Célula 11 — resumo das metodologias

- `fillna("")`: troca metodologia ausente por texto vazio.
- `str.lower()`: transforma o texto em minúsculas.
- As variáveis `online`, `telefone` e `presencial` procuram palavras relacionadas a cada método.
- O símbolo `|` dentro do texto de busca significa “ou”.
- `astype(int)`: transforma verdadeiro em `1` e falso em `0`.
- A soma maior que `1` identifica pesquisas com mais de uma forma de coleta e cria a categoria `misto`.
- `np.select(...)`: escolhe a categoria de cada linha de acordo com as condições.
- `default="Não identificado"`: classifica textos que não combinaram com as palavras procuradas.
- `to_csv(...)`: salva a base limpa do TSE.
- O último recorte com `head()`: exibe quatro colunas importantes das primeiras linhas.

### Célula 12 — medidas descritivas do TSE

- O primeiro par de colchetes seleciona amostra, valor da pesquisa e duração.
- `describe()`: calcula quantidade, média, desvio-padrão, mínimo, quartis e máximo.
- `round(2)`: mostra duas casas decimais.

### Célula 13 — resumo por ano

- `groupby("ano_arquivo")`: separa os registros por ano.
- `agg(...)`: calcula várias medidas de uma vez.
- `nunique`: conta protocolos diferentes.
- `mean`: calcula a média da amostra e da duração.
- `median`: calcula a mediana da amostra e do custo.
- `round(2)`: arredonda os resultados.
- `resumo_tse_ano`: exibe a tabela criada.

### Célula 14 — gráficos por ano

- `plt.subplots(1, 2, figsize=(13, 4))`: cria uma figura com dois gráficos lado a lado.
- O primeiro `.plot(kind="bar")`: cria barras com a quantidade de registros por ano.
- `axes[0]`: indica o primeiro gráfico.
- `set_title`, `set_xlabel` e `set_ylabel`: definem título e nomes dos eixos.
- `tick_params(...)`: mantém os anos sem rotação.
- `boxplot(...)`: cria o gráfico de caixa do tamanho das amostras por ano.
- `showfliers=False`: esconde pontos extremos apenas para melhorar a visualização; eles continuam nos dados.
- `axes[1]`: indica o segundo gráfico.
- `plt.suptitle("")`: remove o título automático criado pelo boxplot.
- `tight_layout()`: ajusta os espaços.
- `show()`: mostra os gráficos.

### Célula 15 — gráfico das metodologias

- `value_counts()`: conta os registros em cada categoria de metodologia.
- `.plot(kind="bar")`: cria o gráfico de barras.
- `figsize` e `color`: definem tamanho e cor.
- `title`, `xlabel` e `ylabel`: definem título e eixos.
- `xticks(...)`: gira os nomes das categorias para facilitar a leitura.
- `tight_layout()` e `show()`: ajustam e exibem o gráfico.

### Célula 16 — institutos em foco no Brasil

- `isin([...])`: seleciona AtlasIntel, IBOPE e Ipec.
- `pd.crosstab(...)`: cruza instituto com ano e conta os registros.
- `tabela_foco_brasil`: exibe a tabela.

### Célula 17 — descrição dos erros históricos

- A lista seleciona percentuais, erros, amostra e distância até a eleição.
- `describe()`: calcula as medidas descritivas.
- `round(2)`: arredonda para duas casas.

### Célula 18 — descrição por instituto

- `groupby("pollster")`: separa os dados por instituto.
- `["absolute_error"]`: escolhe o erro absoluto.
- `describe()`: calcula contagem, média, desvio, mínimo, quartis e máximo.
- `sort_values("mean")`: ordena do menor para o maior erro médio.
- `round(2)`: arredonda a tabela.
- `descricao_institutos`: exibe o resultado.

### Célula 19 — MAE, RMSE e tendência

- A primeira linha calcula o MAE de cada instituto.
- A segunda agrupa os erros assinados e aplica a fórmula do RMSE.
- `erros ** 2`: eleva cada erro ao quadrado.
- `np.mean(...)`: calcula a média dos quadrados.
- `np.sqrt(...)`: tira a raiz quadrada.
- A linha de `tendencia_erro` calcula o erro assinado médio.
- `nunique()` conta quantas pesquisas diferentes cada instituto possui.
- `pd.DataFrame({...})`: reúne as quatro métricas em uma tabela.
- `sort_values("MAE")`: ordena pelo menor MAE.
- `round(2)`: arredonda os valores.
- `metricas_erro`: exibe a tabela.

### Célula 20 — pesquisa contra resultado real

- `plt.figure(...)`: cria a área do gráfico.
- O `for` percorre um instituto de cada vez.
- `plt.scatter(...)`: desenha um ponto para cada comparação entre pesquisa e resultado.
- `alpha=0.45`: deixa os pontos transparentes para mostrar sobreposições.
- As linhas `limite_min` e `limite_max` encontram os menores e maiores percentuais dos dois eixos.
- `plt.plot(...)`: desenha a linha tracejada em que pesquisa e resultado seriam iguais.
- `title`, `xlabel`, `ylabel` e `legend`: definem as informações do gráfico.
- `tight_layout()` e `show()`: ajustam e exibem.

### Célula 21 — comparação dos erros

- `subplots(1, 2, ...)`: cria dois gráficos lado a lado.
- O primeiro `plot(kind="bar")` mostra o MAE ordenado.
- Os comandos de `axes[0]` definem título e eixos do primeiro gráfico.
- `boxplot(...)` mostra a distribuição completa dos erros por instituto.
- Os comandos de `axes[1]` definem título e eixos do segundo gráfico.
- `suptitle("")`: remove o título automático.
- `tight_layout()` e `show()`: ajustam e exibem.

### Célula 22 — grupos de dias até a eleição

- `faixas = [...]`: define os limites dos intervalos de dias.
- `np.inf`: representa todos os valores acima de 30 dias.
- `rotulos = [...]`: define o nome de cada intervalo.
- `pd.cut(...)`: coloca cada pesquisa em uma faixa de dias.
- `groupby("faixa_dias")`: separa as pesquisas pelas faixas.
- `agg(["count", "mean", "median", "std"])`: calcula quantidade, média, mediana e desvio do erro.
- `round(2)`: arredonda a tabela.
- `erro_por_distancia`: exibe o resultado.

### Célula 23 — gráfico do erro por distância

- `erro_por_distancia["mean"]`: seleciona o erro médio de cada faixa.
- `.plot(kind="bar")`: cria barras.
- `title`, `xlabel` e `ylabel`: explicam o gráfico e seus eixos.
- `xticks(...)`: gira os rótulos.
- `tight_layout()` e `show()`: ajustam e exibem.

### Célula 24 — evolução do erro por ano

- `groupby(["cycle", "pollster"])`: agrupa por ano e instituto.
- `mean()`: calcula o erro médio de cada grupo.
- `unstack()`: transforma os institutos em colunas.
- `plot(marker="o")`: cria linhas com um ponto em cada ano.
- `title`, `xlabel`, `ylabel` e `xticks`: definem título, eixos e anos mostrados.
- `tight_layout()` e `show()`: ajustam e exibem.

### Célula 25 — última pesquisa antes de cada eleição

- `sort_values(...)`: ordena instituto, disputa, distância da eleição e data.
- `drop_duplicates(..., keep="first")`: mantém a pesquisa mais próxima da votação para cada instituto e disputa.
- `copy()`: cria uma cópia segura do resultado.
- `groupby("pollster")`: separa por instituto.
- `agg(...)`: calcula quantidade de eleições, erro médio da margem, tendência, desvio e taxa de acerto do vencedor.
- A multiplicação por `100` transforma a taxa de acerto em porcentagem.
- `sort_values(...)`: ordena pelo menor erro médio de margem.
- `round(2)`: arredonda os valores.
- Os dois `to_csv(...)`: salvam o ranking e as pesquisas finais.
- `ranking_final`: exibe a tabela.

### Célula 26 — metodologia histórica

- `assign(...)`: cria temporariamente uma coluna chamada `metodologia`.
- `fillna("Não informada")`: dá um nome aos valores ausentes.
- `groupby("metodologia")`: separa por método.
- `agg(...)`: calcula quantidade, média, mediana e desvio do erro.
- `query("count >= 20")`: mantém somente métodos com pelo menos 20 observações.
- `sort_values("mean")`: ordena pelo menor erro médio.
- `round(2)`: arredonda.
- `resumo_metodo`: exibe a tabela.

### Célula 27 — amostra e erro

- `plt.figure(...)`: cria a área do gráfico.
- `plt.scatter(samplesize, absolute_error)`: compara o tamanho da amostra com o erro.
- `alpha=0.35`: deixa pontos sobrepostos visíveis.
- `title`, `xlabel` e `ylabel`: definem título e eixos.
- `tight_layout()` e `show()`: ajustam e exibem.
- `corr()`: calcula a correlação entre amostra e erro.
- `.iloc[0, 1]`: pega o valor da relação entre as duas colunas.
- `print(f"...")`: mostra a correlação com três casas decimais.

### Célula 28 — pesquisa atual do Brasil

- `pd.DataFrame({...})`: cria uma pequena tabela com os nomes e percentuais publicados.
- A lista `candidato` contém os sete nomes do cenário escolhido.
- A lista `percentual` contém os valores correspondentes.
- `to_csv(...)`: salva essa tabela em `data/processed`.
- `pesquisa_atual.plot(...)`: cria o gráfico de barras.
- `x="candidato"` e `y="percentual"`: escolhem as colunas dos eixos.
- `legend=False`: remove uma legenda desnecessária.
- `figsize` e `color`: definem tamanho e cor.
- `set_title`, `set_xlabel` e `set_ylabel`: definem título e eixos.
- `tick_params(...)`: gira os nomes para melhorar a leitura.
- O `for` percorre cada percentual.
- `ax.text(...)`: escreve o valor acima de cada barra.
- `valor + 0.6`: posiciona o texto um pouco acima da barra.
- `f"{valor:.1f}%"`: mostra uma casa decimal e o símbolo de porcentagem.
- `tight_layout()` e `show()`: ajustam e exibem o gráfico.
- `pesquisa_atual`: exibe a tabela usada no gráfico.

## 8. Principais resultados para comentar

- Foram reunidos 3.359 registros presidenciais válidos do TSE depois da remoção de protocolos repetidos.
- Na base histórica, ficaram 582 pesquisas e 1.164 observações de candidatos.
- O erro absoluto médio foi menor nas pesquisas mais próximas da eleição: 2,04 pontos percentuais entre 0 e 3 dias, contra 4,49 entre 16 e 30 dias.
- AtlasIntel apresentou o menor MAE no recorte histórico, mas possui somente 10 pesquisas nesse recorte. Por isso, não é correto declarar um vencedor definitivo sem considerar a diferença na quantidade de observações.
- A correlação entre tamanho da amostra e erro foi aproximadamente `-0,006`, ou seja, praticamente nula nesse recorte. Isso não significa que a amostra não importa; significa apenas que, sozinha, ela não explicou o erro observado.
- No cenário atual mostrado, Lula aparece com 44,9% e Flávio Bolsonaro com 35,8%. Esses valores pertencem a uma única pesquisa feita em julho de 2026.

## 9. Como explicar os gráficos

### Registros no TSE

Mostra quantas pesquisas presidenciais foram registradas em cada ano analisado. Mais registros não significam automaticamente maior qualidade.

### Boxplot das amostras

A linha dentro da caixa é a mediana. A caixa mostra a parte central dos tamanhos de amostra. Ela ajuda a comparar os anos sem olhar somente para a média.

### Pesquisa versus resultado

Quanto mais próximo um ponto estiver da linha tracejada, mais próxima a pesquisa ficou do resultado real.

### MAE e boxplot dos erros

O gráfico de barras facilita comparar o erro médio. O boxplot mostra que um mesmo instituto pode ter erros pequenos e grandes.

### Distância até a eleição

No conjunto estudado, as pesquisas feitas mais perto da votação tiveram erro médio menor. Isso é uma associação observada, não uma regra garantida.

### Pesquisa atual

As barras apenas reproduzem os percentuais publicados no cenário escolhido. A diferença entre candidatos deve ser lida junto da margem de erro, da data e da metodologia.

## 10. Limitações que devem ser ditas

- O TSE oferece registros e documentos, mas não uma única coluna padronizada com a intenção de voto de todos os candidatos.
- O estudo de precisão usa eleições norte-americanas porque essa base já liga pesquisas a resultados reais.
- Os institutos têm quantidades diferentes de pesquisas no recorte.
- A base histórica disponível não inclui a eleição de 2024.
- Uma pesquisa atual não mostra uma tendência sozinha. Para falar em crescimento ou queda, seriam necessárias várias pesquisas comparáveis ao longo do tempo.
- Correlação não prova manipulação, influência sobre votos ou relação de causa e efeito.

## 11. Perguntas que podem aparecer

### Por que foram usadas bases dos Estados Unidos?

Porque elas já possuem o percentual pesquisado e o resultado real na mesma estrutura. Isso permite praticar o cálculo de erro. Os registros do TSE foram usados para analisar o contexto brasileiro.

### O instituto com menor MAE é sempre o melhor?

Não. É preciso considerar quantidade de pesquisas, anos, disputas, método e tamanho da amostra. No recorte, os institutos não possuem o mesmo número de observações.

### Uma amostra maior garante menor erro?

Não garante. O modo de selecionar os entrevistados, a metodologia, a data e outras decisões também importam.

### O gráfico de 2026 prevê o vencedor?

Não. Ele reproduz uma pesquisa feita entre 22 e 27 de julho de 2026. É uma fotografia daquele momento.

### Podemos afirmar uma tendência de crescimento?

Ainda não com esse único gráfico. Uma tendência precisa de vários levantamentos comparáveis, preferencialmente do mesmo instituto e com a mesma pergunta.

## 12. Links das fontes

1. [TSE — Pesquisas Eleitorais de 2014](https://dadosabertos.tse.jus.br/dataset/pesquisas-eleitorais-2014)
2. [TSE — Pesquisas Eleitorais de 2018](https://dadosabertos.tse.jus.br/dataset/pesquisas-eleitorais-2018)
3. [TSE — Pesquisas Eleitorais de 2022](https://dadosabertos.tse.jus.br/dataset/pesquisas-eleitorais-2022)
4. [TSE — Pesquisas Eleitorais de 2026](https://dadosabertos.tse.jus.br/dataset/pesquisas-eleitorais-2026)
5. [FiveThirtyEight — dados históricos](https://github.com/fivethirtyeight/data/tree/master/pollster-ratings)
6. [MIT Election Data and Science Lab](https://electionlab.mit.edu/data)
7. [AtlasIntel/Bloomberg — relatório de julho de 2026](https://cdn.atlasintel.org/498dd172-4381-4192-977c-c4af9787434f.pdf)
