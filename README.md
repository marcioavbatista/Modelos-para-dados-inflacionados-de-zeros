# Modelos para dados inflacionados de zeros: Teoria, Diagnóstico e Aplicação

Este repositório é dedicado à publicação e consulta do projeto de pesquisa voltado para a análise estatística de dados de contagem com excesso de observações nulas.

📄 **[Clique aqui para acessar e fazer o download do documento completo (PDF)](Modelos%20para%20dados%20inflacionados%20de%20zeros.pdf)**

---

## Autoria e Afiliação

* **Márcio A. V. Batista** (Aluno)
* **Lourdes C. C. Montenegro** (Orientadora)
  
**Instituição:** Departamento de Estatística - Instituto de Ciências Exatas, Universidade Federal de Minas Gerais (UFMG).

**Apoio Institucional:** Este projeto foi desenvolvido com o suporte da Pró-Reitoria de Pesquisa (PRPq) da UFMG, por meio do Programa de Bolsas de Iniciação Científica (PROBIC).
---

## Resumo do Projeto

### Introdução e Revisão Teórica
A análise de dados de contagem constitui um pilar fundamental da estatística aplicada, sendo amplamente utilizada para descrever a frequência de ocorrência de eventos em um determinado intervalo. Historicamente, os Modelos Lineares Generalizados (MLG) estabeleceram o arcabouço metodológico padrão para lidar com variáveis respostas discretas, tendo a distribuição de Poisson como a principal escolha teórica. Contudo, a aplicação desses modelos clássicos frequentemente revela limitações importantes, destacando-se a presença de excesso de zeros. Em muitos fenômenos reais, a frequência de observações nulas é substancialmente maior do que a prevista pelo modelo, sugerindo que a estrutura dos dados é proveniente de múltiplos mecanismos geradores. 

Para compreender esse problema, é fundamental distinguir dois tipos de zeros: os zeros amostrais, que correspondem a situações em que o evento poderia ter ocorrido, mas não foi observado, e os zeros estruturais, correspondentes a situações em que a unidade experimental não possui possibilidade de gerar o evento. Quando modelos tradicionais ignoram que os zeros podem ter origens distintas e tratam todas essas observações como resultado de um único processo, ocorre uma distorção na estimação dos parâmetros. Para contornar essa limitação, a literatura desenvolveu os modelos inflacionados de zeros. Trata-se de modelos de mistura que combinam um processo degenerado, responsável pela geração de zeros estruturais, e um processo de contagem padrão. A análise abrange desde a extensão natural do modelo Poisson (ZIP), passando por formulações de caudas mais pesadas para lidar com variabilidade adicional, como o Geométrico (ZIG) e o Binomial Negativo (ZINB), até a elevada flexibilidade do modelo Poisson Generalizado (ZIGP). 

### Objetivos
O presente trabalho tem como objetivo explorar, de forma integrada, a teoria, o diagnóstico e a aplicação de modelos de contagem com inflação de zeros. Busca-se detalhar as propriedades estatísticas e o processo de estimação da extensão básica do modelo Poisson (ZIP), bem como das formulações mais robustas para dados complexos: o modelo Geométrico (ZIG), o Binomial Negativo (ZINB) e o Poisson Generalizada (ZIGP).

O estudo confere especial atenção aos desafios computacionais da estimação, objetivando explicar a maximização da log-verossimilhança por meio do algoritmo EM. O objetivo metodológico é demonstrar como a introdução de uma variável latente permite contornar a dificuldade de não se conhecer, a priori, a origem de cada valor zero (estrutural ou amostral). Adicionalmente, o projeto visa demonstrar a limitação dos resíduos clássicos para dados discretos inflacionados, implementando técnicas de diagnóstico baseadas em resíduos quantílicos randomizados (DHARMa) operacionalizados via simulação. Por fim, propõe-se consolidar a teoria desenvolvida por meio de quatro aplicações práticas a conjuntos de dados reais, demonstrando a adequação dos modelos frente a diferentes comportamentos empíricos.

### Metodologia
A estrutura metodológica baseia-se em modelos de contagem inflacionados de zeros, que constituem uma classe de modelos de mistura com dois processos geradores. O primeiro é um processo degenerado que produz apenas zeros estruturais, onde a probabilidade de a observação pertencer a esse grupo (π) é modelada por meio da função de ligação logit. O segundo é um processo de contagem que pode gerar zeros amostrais e valores positivos, cuja média (μ) é modelada por uma função de ligação logarítmica relacionando-a ao preditor linear. 

A estimação dos parâmetros dos modelos ZIP, ZIG, ZINB e ZIGP é realizada predominantemente pelo método de Máxima Verossimilhança. Devido à dificuldade numérica de maximizar diretamente a função de log-verossimilhança pela incerteza da origem do zero, introduz-se uma variável latente Wi, que assume valor 1 se a observação pertence ao estado de inflação estrutural e 0 se pertence ao processo de contagem. O procedimento iterativo proposto na literatura para obter os estimadores diante dessa variável não observável é o algoritmo EM. Esse algoritmo alterna entre o Passo E (Expectation), no qual se calcula a probabilidade a posteriori da variável latente, e o Passo M (Maximization), que maximiza a log-verossimilhança dos dados completos por meio de regressões ponderadas. Computacionalmente, o ajuste prático dos modelos aos conjuntos de dados reais foi realizado no ambiente R utilizando o pacote glmmTMB. A avaliação da qualidade de ajuste superou as limitações dos resíduos tradicionais através do pacote DHARMa, empregando resíduos quantílicos randomizados que transformam a função discreta em uma variável contínua uniforme via simulação, avaliada posteriormente pelo teste de Kolmogorov-Smirnov (KS) e gráficos QQ Plot.

### Resultados e Discussão
As aplicações práticas evidenciaram que a escolha entre os modelos é dependente da estrutura dos dados e do fenômeno em análise. Na Aplicação I (dados bioChemists), o modelo ZIP demonstrou ser superior à distribuição de Poisson clássica ao identificar que a produtividade do orientador atua como um fator de proteção acadêmica. Cada artigo adicional do orientador reduz em aproximadamente 12% a chance de o aluno pertencer ao grupo de zeros estruturais. Contudo, a análise visual do pacote DHARMa revelou sobredispersão residual (p=0) e falha no teste de uniformidade KS (p=0,0084), indicando que o modelo ZIP ainda se mostrou restritivo. 

Na Aplicação II (fishing), a limitação de variância foi contornada pelo modelo ZIG, cuja restrição teórica no parâmetro de dispersão garantiu uma estrutura de variância quadrática fixa. O modelo capturou efeitos divergentes da mesma covariável: cada criança adicional aumenta em cerca de 11 vezes a chance de o grupo não realizar capturas (zero estrutural), mas reduz a taxa esperada de captura em aproximadamente 67%. Na Aplicação III (Student Performance), o modelo ZINB lidou com o absenteísmo escolar permitindo a variância flexível por meio da estimação do parâmetro de dispersão em θ=1,67. O ajuste confirmou que estudantes em relacionamentos românticos possuem uma taxa de faltas aproximadamente 52,6% maior em comparação aos demais, com diagnóstico impecável nos resíduos simulados. Na Aplicação IV (Salamanders), o modelo ZIGP atingiu a máxima flexibilidade acomodando de forma unificada os dados ecológicos complexos. O modelo evidenciou o drástico impacto da mineração, onde áreas preservadas apresentam uma taxa de ocorrência aproximadamente 7,23 vezes maior. A componente logística do ZIGP resultou em uma OR de 0,139 para a inflação, atestando de forma rigorosa que a maior parte das ausências era proveniente do próprio processo de contagem da Poisson Generalizada e não de impossibilidades estruturais de ocorrência.
