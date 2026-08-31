# Classificador de Notícias Naive-Bayes
Um classificador de editorias com Naive-Bayes com utilização de suavização de Laplace e ponderação TF-IDF

## Fundamentos

Seja o Teorema de Bayes: $P(c|d) = \frac{P(c) \cdot P(d|c)}{P(d)}$, em que c é a categoria (editoria) e d é um documento com palavras a serem analisadas.

A ideia é utilizar a simplificação de Naive na análise dessa probabilidade, uma vez que o termo $P(d|c)$ pode representar todas as possíveis combinações entre palavras. A simplificação consiste em dizer que cada palavra é independete da outra, ou seja, cada probabilidade pode ser analisada individualmente, gerando essa configuração: $P(d|c) = P(w_{1}|c) \cdot P(w_{2}|c) \cdot ... \cdot P(w_{n}|c)$

Assim sendo, a análise será com base na comparação dos termos $P(c|d)$, podendo, então, serem desprezados as probabilidades $P(d)$ no denominador. A maior probabilidade seleciona a categoria.

Agora, analisando individualmente $P(d|c)$, o termo poderá ser escrito como: $P(d|c) = \prod_{i=1}^{n} P(w_{i}|c)$, sendo $P(w_{i}|c) = \frac{n_{i|c}}{N_{c}}$ ($n_{i|c}$ é o número de ocorrências da palavra na categoria e $N_{c} é o total de palavras daquela categoria no documento). No entanto, há um problema quando $n_{i|c}$ é zero, pois ele zera o produtório junto e análise cessa. Para mitigar essa situação, utiliza-se a suavização de Laplace em que o termo vira: $P(w_{i}|c) = \frac{n_{i|c}+1}{N_{c} + V}$, em que V é o número de palavras distintas no documento (vocabulário total).

Considerando que haverão probabilidades que serão quase infinitamente pequenas (tão pequenas que o próprio computador aproxima para 0.0), deve se fazer um ajuste na análise comparativa. Sabendo que uma função logarítmica é crescente, podemos fazer a analise apenas com `log`. Então será representado da seguinte maneira: $S(c|d) = \log(P(c)) + \sum_{i=1}^{n} \log(P(w_{i}|c))$

A formulação está quase pronta, apenas mais um refinamento é necessário: TF-IDF (Term Frequency, Inverse Document Frequency), que serve para mensurar a importância de certa palavra em um texto. A ideia é visualizável pela equação: $\frac{TF}{IDF}$, em que quanto maior TF (aparição da palavra no texto) e menor IDF (raridade da palavra no documento), maior o fator TF-IDF. Representa-se por: $TF-IDF(w_{i}|d) = n_{i|d} \cdot \log(\frac{M}{1+m_{i}})$, sendo $n_{i|d}$ o número de ocorrências da palavra no documento $d$, $M$ é o total de documentos da base e $m_{i}$ é o número de documentos em que a palavra $w_{i}$ aparece.

A ideação final será, portanto: $P_{aprox}(w_{i}|c) = \frac{n_{aprox, i|c} + 1}{N_{c} + V}, n_{aprox, i|c} = \log(\frac{M}{m_{i} + 1}) \cdot n_{i|c}$.
