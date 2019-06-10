# Hierarquia de Memória

A memória corresponde ao dispisitivo capaz de armazenar dados. Atualmente, computadores de quanlquer tamanho contam com grandes quantidades de memória.

O acesso à memória costuma ser a etapa mais lenta durante o processamento. Por isso, existem diversas tecnologias de memória, cada uma com velocidade, capacidade e custos distintos.

Por via de regra, memótrias com maior velocidade de acesso são mais caras, por isso sempre possuem menor capacidade de armazenamento.

Isso sugere uma organização de memória no computador de acordo com a necessidade de armazenamento versus velocidade.

A organização da memória toma por base o princípio da localidade. Esse princípio diz que os programas acessam uma pequena porção de dados em qualquer instante de tempo. Para determinar uma porção de dados que serão acessados por um programa num intervalo de tempo, há 2 linhas de localidade:

1. **Localidade Temporal**: se um dado foi referenciado, ele tenderá ser referenciado novamente em breve.
2. **Localidade Espacial**: se um dado foi referenciado, itens com endereço próximo tendem a ser referenciados em breve.

Esse princípio sugere uma organização de memória que chamamos de **Hierarquia de Memória**, que consiste em múltiplos níveis de memória de diferente tamanho e velociade de acesso. Nesse sentido, procuramos, numa hierarquia de memória:

- custo-benefício equivalente nos níveis
- **memória mais rápida fica mais próxima ao processador**

![imagem de piramide]()

Os dados são mantidos todos no nível mais inferior, e blocos de dados são transferidos entre níveis adjacentes.

O processador acessa os dados do nível superior apenas. Se o dado requisitado pelo processador já estiver presente no nível 1, então dizemos que houve um **acerto**, caso contrário, houve uma **falha**.

A **taxa de acerto** é a fração de acessos à memória que resultaram em acerto, enquanto a taxa de falha é a fração de acesssos que resultaram em falhas. Um dos objetivos da organização dos dados na hierarquia é **maximizar a taxa de acertos**.

O **tempo de acerto** é o tempo necessário para a unidade de memória no nive; 1 determinar se o dado requisitado está presente e transferí-lo ao processador. A **penalidade de falha** é o tempo de busca e transferência dp dado, que está em algum dos níveis inferiores.
