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


    