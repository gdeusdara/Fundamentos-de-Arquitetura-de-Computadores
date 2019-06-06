# Pipeline

Na prática, uma implementação com ciclos de clock variáveis é inviável, mas uma possível abordagem consiste em dividir instruções em **etapas**, tomando por período de clock o tempo da etama mais demorada. Com isso, é possível executar mais de uma etapa por ciclo, sobrescrevendo instruções.

Essa técnica de implementação é chamada de **pipeline**.

Para entender bem o conceito, consideremos a seguinte analogia: a tarefa de lavar n trouxas de roupa. Veja o slide 13.

A instruções MIPS podem ser divididas em 5 etapas:

1. Buscar a instrução na memória de instruções,
2. Ler dados de registradores
3. Executar uma operação aritmética
4. Acessa um operando na memória de dados
5. Escrever o resultado em um registrador.

Usando essas etapas, é possível aplicar a técnica de pipeline.

### Exemplo

Consideremos as instruções: lw, sw, add, sub, slt, and, or e beq. Sejam ps tempos:

* 200ps para acesso à memória
* 200ps para operação com ULA
* 100ps para acesso aos registradores.

Vamos comparar o tempo médio entre as instruções numa implementação monociclo e pipepline.

O tempo que cada instrução consome é:

| Instrução | Etapas | | | | |
|---|---|---|---|---|---|
| | 1 | 2 | 3 | 4 | 5| 
| lw | 200ps | 100ps | 200ps | 200ps | 100ps |
| sw | 200ps | 100ps | 200ps | 200ps | - |
| Tipo R | 200ps | 100ps | 200ps | - | 100ps | 
| beq | 200ps | 100ps | 200ps | - | - |

 Logo, lw = 800 ps, sw = 700 ps, tipo R = 600 ps e beq = 500 ps.

Numa implementação monociclo, o período de clock seria 800 ps.
No pipeline, o período seria 200 ps, que corresponde à etapa mais lenta.

Consideremos:

* (a) Uma camada de um lw:
    * monociclo: 800 ps
    * pipeline: 1000 ps

* (b) 3 camadas de lw:
    * monociclo = 3 x 800 ps = 2400 ps
    * pipeline = 1000 + 2 x 200 ps = 1400 ps

Nesse caso,

    Desempenho pipeline / Desempenho monociclo = 2400/1400 = 1,2

* (c) 3 + 1 000 000 de camadas de lw:
    * monociclo: 8 002 400 ps
    * pipeline: 1400 + 1 000 000 x 200 ps = 2 000 001 400 ps.

E nesse caso,

    Desempenho pipeline / Desempenho monociclo = 800 002 400 / 200 001 400 = 4

Portanto, a implementação pipeline tende a ser 4 vezes mais rápida que a monociclo

pelo exemplo, podemos concluir que:

* a) O pipeline melhora o desempenho aumentando a vazão do processador, mas não melhora o tempo individual das instruções
* b) Melhorar a vazão do processador é interessante: programas podem executar bilhões de instruções por segundo
* c) Em geral, podemos observar que a melhora total de tempo que temos na implementação pipeline é algo que tende a ser proporcional à diminuição no período de clock se comparado à implementação monociclo. Neste contexto, se todas as etapas da pipeline consumissem o mesmo tempo, então a melhora relativa seria igual à quantidade de etapas.

## Propriedade do conjunto de instruções que viabilizam o pipeline

Há 4 pontos que merecem destaque para uma arquitetura de instruções adequada ao pipeline

1. **Todas as instruções possuem o mesmo tamanho**. Isso faz com que a base de instruções possa ser feita em um único estágio.
2. **Os diversos formatos de instruções possuem pontos em comum**. Por exemplo, no MIPS, os registradores de leitura ocupam sempre os mesmos campos. Isso faz com que a leitura de dados possa ser feita em um único estágio do pipeline.
3. **Operações só podem ser feitas no processador, e não na memória de dados**. Deste modo, um único acesso à memória de dados basta a cada etapa do pipeline. Em caso contrario, seriam necessários diversos acessos, o que poderia afetar o período de clock ou a quantidade de etapas.
4. **Há a restrição de alinhamento de dados**. Com isso, é possível ler na memória de dados numa única etapa de pipeline.

## Hazards de Pipeline

Hazards em pipeline acontecem quando uma etapa de uma instrução não pode ser executada.

Há 3 tipos: estruturais, de dados e de controle.

### Estruturais

Acontecem quando há a;go na estrutura da arquitetura que impeçam a execução de uma etapa

Na arquitetura MIPS não há hazards estruturais, mas poderia ter, por exemplo, se tivéssemos apenas 1 módulo para instruções, e tivessemos que executar uma quarta lw no Slide 14. Haveria um conflito de meória com a primeira instrução.

### De Dados

Acontecem quando os dados necessários para executar uma instrução ainda não estão disponíveis, por exemplo, nas instruções:

    add $s0, $t0, $t1
    sub $t2, $s0, $t3

A subtração usa o resultado da adição que estaria disponível apenas no quinto estágio de execucão da instrução add.

Uma possível solução para esse problema seria passar o resultado da adição para a subtração antes de terminar sua execução.

![imagem de add e sub]()

Para tanto, é necessário criar um novo dispositivo de Hardware que encaminhe o resultado da ULA diretamente como entrada para a próxima instrução. Essa técnica é chamada **forwarding** ou **bypassing**.

O **Forwarding** só é válido se o estágio de destino estiver mais adiante no tempo que o estado de origem. Por isso, nem sempre resolve os Hazards de dados. Por Exemplo

    lw $s0, 20($t1)
    sub $t2, $s0, $t3

Nesse caso, teríamos que atrasar a execução da sub. Esse atraso é conhecido como **pipeline stall**, ou **bolha**. Veja o slide 15.

Outra estratégia para resolução deste problema seria a reordenação das instruções. Por exemplo, considere o código a seguir:

    a = b + c;
    c = b + f;

Suponha que b,c e f estejam armazenadas sequencialmente na memória. O código MIPS:

    lw $t1, 0($t0) # carrega b
    lw $t2, 4($t0) # carrega c
    add $t3, $t1, $t2 # a = b + c
    sw $t3, 12($t0) # armazena a
    lw $t4, 8($t0) # carrega f
    add $t5, $t1, $t4 # c = b + f
    sw $t5, 16($t0) # armazena c

![imagem de troca]()

### De Controle

O Hazard de controle ocorre em instruções de desvio condicional. Por não saber qual a próxima instrução a ser executada, pode-se gerar uma bolha.

Possíveis soluções:

* **prever desvios**
* **decisão adiada**: mudar a ordem das instruções e sempre executar a instrução seguinte ao desvio.

Exemplo:

    add $t0, $s0, $s1
    beq $s3, $s4, label

## O caminho de dados e controle

O projeto consiste em dividir o caminho de dados da implementação monociclo nas 5 etapas:

1. IF (*instruction Fetch*): busca de instrução.
2. ID (*instruction Decode*): decodificação da instrução (ou seja, a divisão nos caminhos adequados) e a leitura de registradores
3. WB (*Write Back*): escrita do resultado nos registradores.




    