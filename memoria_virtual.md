# Memória Virtual

Da mesma forma que a memória cache é usada como um _buffer_ da memória principal, a memória virtual é o _buffer_ entre memória secundária (disco magnético, ssd) e a memória principal. A memória virtual tem 3 objetivos básicos:

1. **swapping**: fornecer uma memória principal maior que a física.

2. **Proteção**: permitir um compartilhamento eficiente e seguro entre os programas e

3. **Relocação**: Mapeamento lógico de endereços físicos, melhor aproveitamento da memória física.

A memória virtual, assim como a cache, é composta por blocos indexados.

- um bloco é chamado **página**.
- uma falha é chamada **falta de página**(_page fault_).
- Há **endereços virtuais**, que são mapeados para endereços físicos.

Deste modo, igual no cache, o endereço virtual é desmembrado em:

| número da Página      | offset |
| --------------------- | ------ |
| 0 1 2 3 4 5 ... 30 31 |

| número de pag. físico | offset |
| --------------------- | ------ |
| Endereço físico       |

O custo de uma falta de página é muito alto já que a memória secundária é algo na casa de 100 mil vezes mais lenta que a principal. Por isso, a alocação dos dados na memória principal isando a memória virtual é otimizado para poupar falhas. neste contexto,

- As páginas devem ser grandes os suficiente para minimizar falhas. tamanhos típicos variam entre 4 e 16 kb.

- memória virtual usa um **mapeamento associativo**, em contraste ao direto usado na cache.

- Escritas são feitas usando **write-back**. O método **write-through** é inviável
