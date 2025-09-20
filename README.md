# Análise de GAP (Cálculo e Análise de GAP com base no ano e nível de maturidade)
Autor: Rogério R R Coelho

Estruturar um estudo estatístico usando Python (em Jupyter Notebook), com dados de diagnósticos aplicados entre 2022 e 2024.

# 📊 Objetivo da Análise

Objetivos:

* Identificar padrões entre os participantes com melhor desempenho em cada nível de maturidade (são 4 níveis), com base em variáveis como tempo de registro, porte, faturamento, número de pessoas envolvidas, etc.
* Indicar o momento ideal para transição entre níveis, com base em evidências estatísticas.
* Calcular e analisar o gap (nota máxima esperada menos nota observada), considerando o ano e o nível.
* Apresentar/sugerir as metodologias estatísticas/matemáticas mais adequadas para esse tipo de análise.

Observações: Gerei dados aleatórios para preservar as informações do cliente.

# 📂 Base de Dados
Total 11 colunas:
# 📑 Dicionário de Dados

| **Coluna**               | **Tipo sugerido**                           | **Descrição / Significado**                                                                                                                                                        | **Propósito na Análise** |
|--------------------------|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------|
| **id_participante**      | `int` ou `string` (ID único)                 | Identificador único de cada participante da pesquisa/diagnóstico.                                                                                                                   | Permite diferenciar e rastrear cada participante de forma individual. |
| **ano**                  | `int` (2022–2024)                            | Ano em que o diagnóstico foi aplicado.                                                                                                                                              | Permite analisar evolução temporal (ex.: mudanças de desempenho ao longo dos anos). |
| **nivel_maturidade**     | `int` ou `categorical` (1–4)                 | Nível de maturidade atribuído ao participante (ex.: 1 = inicial, 4 = avançado).                                                                                                     | Essencial para identificar padrões de desempenho em cada estágio de maturidade e avaliar a transição entre níveis. |
| **tempo_registro**       | `float` ou `int` (meses ou anos)             | Tempo de registro formal da organização (ou tempo de atuação) no mercado ou no programa, em meses ou anos.                                                                          | Avaliar se a experiência/tempo influencia o nível de maturidade e o desempenho. |
| **porte**                | `categorical` (ex.: pequeno, médio, grande)  | Classificação do porte da organização (ex.: com base em número de funcionários ou faturamento).                                                                                     | Verificar correlação entre porte e desempenho/maturidade. |
| **faturamento**          | `float` (valor monetário)                    | Faturamento anual da organização, em moeda local.                                                                                                                                   | Investigar se há relação entre faturamento e nível de maturidade ou desempenho. |
| **n_pessoas**            | `int`                                       | Número de pessoas envolvidas no processo ou equipe avaliada.                                                                                                                        | Avaliar se equipes maiores ou menores têm relação com o nível de maturidade ou desempenho. |
| **nota_maxima_esperada** | `float`                                      | Nota máxima teórica ou esperada para o diagnóstico (benchmark de referência).                                                                                                       | Base para cálculo do **gap** (diferença entre o esperado e o observado). |
| **nota_observada**       | `float`                                      | Nota obtida pelo participante no diagnóstico.                                                                                                                                       | Indicador real de desempenho para análise do gap e evolução do participante. |
| **nota**                 | `float`                                      | Nota final ponderada ou ajustada, caso haja normalização ou cálculo derivado (se não houver ajuste, pode ser redundante com `nota_observada`).                                      | Pode ser usada em análises comparativas ou validação da consistência dos dados. |
| **transicao**            | `boolean` (`True/False`) ou `int` (0/1)      | Indicador de que houve transição de nível de maturidade (ex.: passou do nível 2 para 3).                                                                                            | Fundamental para identificar o momento ideal da transição e associar a variáveis explicativas. |


## Metodologias Estatísticas e Ferramentas Utilizadas
O documento detalha o uso das seguintes bibliotecas e técnicas:

*   **`pandas` e `numpy`:** Para manipulação e análise de dados.
*   **`matplotlib` e `seaborn`:** Para visualização de dados (boxplots, histogramas, scatter plots).
*   **`scipy.stats`:** Para testes estatísticos, como ANOVA (`stats.f_oneway`).
*   **`sklearn.preprocessing.LabelEncoder`:** Para codificação de variáveis categóricas (ex: 'porte') para uso em algoritmos de machine learning.
*   **`sklearn.cluster.KMeans`:** Para clusterização, visando identificar padrões não evidentes nos dados.
*   **`lifelines.KaplanMeierFitter`:** Para análise de sobrevivência, utilizada para prever o momento ideal de transição entre os níveis de maturidade.

## Fluxo de Análise
1.  **Exploração dos Dados:** Visualização das primeiras linhas do DataFrame, informações gerais (`df.info()`) e estatísticas descritivas (`df.describe()`).
2.  **Visualização Inicial:** Boxplots para entender a relação entre `nivel_maturidade` e variáveis como `faturamento`, `n_pessoas`, `nota_observada` e `tempo_registro`. Histogramas para distribuição geral das variáveis.
3.  **Identificação de Alto Desempenho:** Definição de 'alto desempenho' (ex: top 10% das notas em cada nível) e análise de como variáveis como faturamento se comportam para esses grupos.
4.  **Testes Estatísticos:** Aplicação de testes ANOVA para comparar variáveis contínuas entre os níveis de maturidade.
5.  **Clusterização:** Uso do algoritmo K-Means para agrupar participantes com base em variáveis numéricas (`tempo_registro`, `porte_encoded`, `faturamento`, `n_pessoas`), buscando padrões ocultos. A variável 'porte' é codificada antes da clusterização.
6.  **Análise de Sobrevivência:** Utilização do modelo Kaplan-Meier para analisar a probabilidade de permanência em um nível antes da transição, com base no `tempo_registro`.
7.  **Análise de GAP:** Cálculo do `gap` (`nota_maxima_esperada` - `nota_observada`) e análise de sua distribuição por ano e nível de maturidade através de estatísticas descritivas e boxplots.


