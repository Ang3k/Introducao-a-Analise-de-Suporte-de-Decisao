# Seminário: Regressão Logística

## Ideia

Roteiro de como o seminário vai ficar nos slides:

1. Início do trabalho
2. Explicação sobre regressão logística
3. Apresentação do problema 1
4. Resolução do problema 1
5. Apresentação do problema 2
6. Resolução do problema 2
7. Conclusão do trabalho

## Requisitos do trabalho

- Arquivo em PDF
- Duração entre 20 e 30 minutos
- Pelo menos um exercício resolvido
- Todos os 4 do grupo participam

## Explicação sobre regressão logística

A regressão logística prevê a **probabilidade** de algo acontecer quando a resposta é sim ou não (1 ou 0).
Exemplos: doente ou saudável, spam ou não spam, aprovado ou reprovado.

A regressão linear não serve para isso, porque ela pode prever valores menores que 0 ou maiores que 1.
A logística resolve isso passando a reta por uma função que sempre fica entre 0 e 1.

### 1. Combinação linear

$$z = b_0 + b_1 x_1 + b_2 x_2 + \dots$$

### 2. Função sigmoide

Transforma qualquer número $z$ em uma probabilidade entre 0 e 1:

$$p = \sigma(z) = \frac{1}{1 + e^{-z}}$$

### 3. Chance e logit

A chance (odds) é $\frac{p}{1-p}$. O log da chance volta a ser a reta:

$$\ln\left(\frac{p}{1-p}\right) = b_0 + b_1 x_1 + \dots$$

Por isso cada coeficiente tem uma leitura simples: $e^{b_1}$ é quanto a chance é multiplicada quando $x_1$ aumenta 1 unidade.

### 4. Custo (log-loss)

Mede o quanto o modelo erra. Quanto menor, melhor:

$$J = -\frac{1}{n} \sum \left[ y \ln(p) + (1-y) \ln(1-p) \right]$$

### 5. Gradiente descendente

O gradiente do custo é:

$$\nabla J = \frac{1}{n} X^T (p - y)$$

E a cada passo os coeficientes são atualizados com uma taxa de aprendizado $\alpha$:

$$b \leftarrow b - \alpha \, \nabla J$$

### 6. Decisão

Se $p \geq 0{,}5$, o modelo diz 1. Se não, diz 0.

## Exercício 1: glicose e diabetes

### Enunciado

Um posto de saúde mediu a glicose em jejum de 12 pacientes e anotou se cada um tem diabetes (1) ou não (0).
Qual a probabilidade de um paciente com glicose de 112 mg/dL ter diabetes?

### Dados

| Paciente | Glicose (mg/dL) | Diabetes |
|----------|-----------------|----------|
| 1        | 80              | 0        |
| 2        | 85              | 0        |
| 3        | 90              | 0        |
| 4        | 95              | 0        |
| 5        | 100             | 1        |
| 6        | 105             | 0        |
| 7        | 110             | 1        |
| 8        | 115             | 0        |
| 9        | 120             | 1        |
| 10       | 130             | 1        |
| 11       | 140             | 1        |
| 12       | 160             | 1        |

### O que mostrar

- A primeira iteração feita na mão: com todos os coeficientes em 0, todo mundo tem $p = 0{,}5$
- Os coeficientes finais $b_0$ e $b_1$
- A leitura de $e^{b_1}$: quanto a chance de diabetes aumenta a cada 1 mg/dL de glicose
- A partir de qual glicose o modelo passa a dizer "diabetes" (quando $z = 0$, ou seja, $x = -b_0 / b_1$)
- A conta da probabilidade do paciente com 112 mg/dL
- Quantos pacientes o modelo acertou e quais ele errou

## Exercício 2: detector de spam

### Enunciado

Um serviço de e-mail quer saber a chance de uma mensagem ser spam (spam = 1).
Ele usa duas informações: quantos links o e-mail tem e quantas palavras suspeitas aparecem nele ("grátis", "ganhe", "promoção").
Qual a probabilidade de um e-mail com 2 links e 2 palavras suspeitas ser spam?

### Dados

| E-mail | Links | Palavras suspeitas | Spam |
|--------|-------|--------------------|------|
| 1      | 0     | 0                  | 0    |
| 2      | 1     | 0                  | 0    |
| 3      | 0     | 1                  | 0    |
| 4      | 2     | 1                  | 0    |
| 5      | 1     | 2                  | 1    |
| 6      | 3     | 0                  | 0    |
| 7      | 4     | 1                  | 1    |
| 8      | 2     | 3                  | 1    |
| 9      | 5     | 2                  | 1    |
| 10     | 1     | 4                  | 1    |
| 11     | 3     | 2                  | 0    |
| 12     | 6     | 3                  | 1    |

### O que mostrar

- Agora são duas variáveis, então $z = b_0 + b_1 \cdot \text{links} + b_2 \cdot \text{palavras}$
- Os coeficientes finais $b_0$, $b_1$ e $b_2$
- A leitura dos sinais: links e palavras suspeitas devem aumentar a chance de spam
- A leitura de $e^{b_1}$ e $e^{b_2}$: qual das duas pesa mais
- A conta da probabilidade do e-mail novo
- Quantos e-mails o modelo acertou e quais ele errou
