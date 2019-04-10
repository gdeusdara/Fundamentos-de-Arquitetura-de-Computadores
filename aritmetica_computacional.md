# Aritmética Computacional

Números podem ser representados em diversas bases. em qualquer base numérica, o valor do i-ésimo dígito d;, é:


    di X Base^i


Por exemplo, o número

| 1 | 0 | 1 | 1 |
|---|---|---|---|
| 3 | 2 | 1 | 0 |

representa

    = (1x2³)+(0x2²)+(1x2¹)+(1x2⁰)
    = 8 + 0 + 2 + 1
    = 11 dec


### Obs:

Para dintinguir bem, adicionaremos o subíndice denotando a base em que se encontra o número.
Usaremos *bin* para binário, *dec* para decimal.


Como temos 32 bits para representar uma palavra na arquitetura MIPS, podemos, representar 2³² padrões distintos, ou seja, números de 0 a 2³² - 1, ou ainda, de 0 a 4.294.967.265dec .

Note a importância do termo representação: um número qualquer possui infinitos algarismos, enquanto no computador, sempre há uma limitação de espaço.
Assim sendo, o computador é projetado para manipular essas representações numéricas de forma a realizar operações aritméticas.
Quando, em uma dessas operações, o resultado não couber no espaço de 32 bits, diz-se que houve um *overflow*. Cabe ao sistema operacional e ao programa decidir o que fazer nesses casos.

## Adição e Subtração

A adição binária é bem similar à adição decimal e é feita do bit menos significativo para o bit mais significativo.

### Exemplo

Somando 6dec a 7dec = 13dec

    0110
    0111
    ____
    1101

### Obs:
Quando se faz a soma entre operandos de sinais diferentes (ou subração entre operandos de mesmo sinal) *nunca* há overflow, pois, neste caso, o resultado será sempre menor que um dos operandos, que já cabia na memória.
Todavia, soma de operandos de mesmo sinal (ou subtração de operandos de sinais opostos) pode acarretar um overflow.

## Representação de Sinais

Nos 32 bits de uma palavra que representa um número, é necessário ainda representar o sinal do número. A maneira mais comum de se fazer isso é usando a estratégia
de *Complemento a dois*. Nesse modo, os 0s mais significativos representam positivos, e os 1s, negativos.

                32bits
    0000 0000 .... 0000bin = 0 dec
    0000 0000 .... 0001bin = 1 dec
    0000 0000 .... 0010bin = 2 dec
     .    .           .
     .    .           .
     .    .           .
    0111 1111 .... 1101bin = 2.147.483.645dec


    1000 0000 .... 0000bin = -2.147.483.648dec
    1000 0000 .... 0001bin = -2.147.483.647dec
    1000 0000 .... 0010bin = -2.147.483.646dec
     .    .           .
     .    .           .
     .    .           .
    1111 1111 .... 1101bin = -3dec
    1111 1111 .... 1110bin = -2dec
    1111 1111 .... 1111bin = -1dec

Usando complemento a 2, um número é convertido à decimal da seguinte forma:

    d31.(-2³¹) + Σdi.2^i  # somatório de i = 0 a i = 30

Sinais dos números refetem diretamente nas instruções de uma arquitetura.

* Para carregar um byte da memória temos as instruções load byte *lb* e unsignad *lbu*. A primeira considera o byte *com* sinal, a segunda, *sem*. Como *lb* considera sinal, ela estende o bit mais significativo dos 8 bits lidos para os 24 bits restantes do registrador de destino.
* Há várias situações que lidamos apenas com números não negativos (Caracteres da tabela ASCII e endereços de memória, por exemplo). Algumas linguagens refletem essa distinção, como em C, que há o modificador *unsigned*.

* Instruções de comparação também devem lidar com sinais. Considerando números com sinal, os que possuem 1 no bit mais significativo são menores que qualquer outro que tenha 0 (pois o primeiro é negativo e o último, positivo). Por outro lado, com números sem sinal, acontece o oposto.

### Exemplo

    $s0 = 1111 1111 .... 1111bin
    $s1 = 0000 0000 .... 0000bin

quanto valem $t0 e $t1?

    slt $t0, $s0, $s1  # $t0=1
    sltu $t1, $s0, $s1  # $t1=0
