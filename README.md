# 📊 Trabalho de Análise Exploratória de Dados

Este projeto foi desenvolvido como um trabalho acadêmico da disciplina de **Análise Exploratória de Dados (AED)**, utilizando as linguagens **Python e R**.

O trabalho está dividido em três exercícios, envolvendo análise descritiva, limpeza e tratamento de dados, estatística e visualização de informações.

---

# 📌 Exercício 1 — Análise Descritiva em R

No primeiro exercício, foi realizada a reescrita do notebook **AED-Lab-Aula 1** utilizando a linguagem **R**.

Além da adaptação do notebook, foi realizada uma **Análise Descritiva de Dados**, aplicando conceitos estudados nas aulas teóricas e práticas.

### Atividades realizadas

* Exploração inicial dos dados;
* Estatística descritiva;
* Cálculo de medidas de tendência central;
* Cálculo de medidas de dispersão;
* Análise da distribuição das variáveis;
* Construção de gráficos;
* Identificação de padrões nos dados;
* Interpretação dos resultados;
* Desenvolvimento de uma conclusão detalhada sobre as informações encontradas.

---

# 🧹 Exercício 2 — Limpeza e Tratamento de Dados em Python

O segundo exercício consiste na limpeza de um dataset contendo dados de **marketing de instituições financeiras**.

A atividade foi desenvolvida utilizando Python e bibliotecas voltadas para análise e visualização de dados.

### Etapas realizadas

* Importação das bibliotecas;
* Carregamento do dataset;
* Exploração inicial dos dados;
* Identificação de valores nulos;
* Análise da quantidade e porcentagem de dados ausentes;
* Remoção da variável `customerid`;
* Separação da variável `jobedu` em `job` e `edu`;
* Tratamento dos valores ausentes da variável `age`;
* Construção de histograma e boxplot para `age`;
* Cálculo da média, mediana e moda;
* Imputação dos valores ausentes de `age` utilizando a moda;
* Tratamento da variável `month`;
* Identificação da moda de `month`;
* Imputação dos valores ausentes de `month`;
* Conversão da variável `salary` para `float`;
* Construção de histograma e boxplot para `salary`;
* Cálculo da média, mediana e moda do salário;
* Imputação dos valores ausentes de `salary` utilizando a mediana;
* Identificação de salários iguais a zero;
* Substituição dos salários iguais a zero pela mediana;
* Tratamento da variável alvo `response`;
* Remoção dos registros que apresentavam valores ausentes em `response`;
* Identificação dos valores `-1` na variável `pdays`;
* Substituição de `-1` por valores ausentes;
* Análise da quantidade de dados ausentes em `pdays`;
* Remoção da variável `pdays` devido à grande quantidade de dados faltantes;
* Verificação final dos valores ausentes;
* Exportação do dataset tratado para `dataset_limpo.csv`.

---

# 📈 Exercício 3 — Análise de Pesquisas Eleitorais

O terceiro exercício analisa **pesquisas eleitorais públicas** e compara as estimativas publicadas com resultados reais. O trabalho também examina os registros brasileiros do TSE e apresenta, separadamente, um cenário eleitoral nacional de 2026.

### Bases utilizadas

* TSE/PesqEle: registros de pesquisas de 2014, 2018, 2022 e 2026;
* FiveThirtyEight: pesquisas presidenciais históricas dos Estados Unidos e avaliação dos institutos;
* AtlasIntel/Bloomberg: cenário brasileiro de julho de 2026;
* resultados eleitorais oficiais usados como referência na comparação histórica.

### Etapas realizadas

* preservação das bases originais em `data/raw`;
* limpeza, padronização e tratamento dos dados em `data/processed`;
* análise de valores ausentes, duplicatas, amostras, custos e metodologias;
* cálculo do erro assinado, erro absoluto, MAE e RMSE;
* comparação entre institutos, anos e distância até a eleição;
* construção de tabelas e gráficos estatísticos;
* geração das tabelas finais em `data/final`;
* documentação das fontes, limitações e critérios da análise.

O notebook, os datasets e a documentação estão em [`aula01/exercicio03`](aula01/exercicio03/README.md). Os relatórios em PDF são mantidos somente como referências locais e não fazem parte do repositório.

---

# 🛠️ Tecnologias e Ferramentas

### 🛠️ Tecnologias e Ferramentas

[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![R](https://img.shields.io/badge/R-276DC3?style=for-the-badge&logo=r&logoColor=white)](https://www.r-project.org/)
[![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)](https://numpy.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge&logo=matplotlib&logoColor=white)](https://matplotlib.org/)
[![Google Colab](https://img.shields.io/badge/Google%20Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white)](https://colab.research.google.com/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/)


---

#
