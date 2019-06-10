# Tipos de memória

Atualmente, há 4 tecnologias de memória:
**SRAM**, **DRAM**, **memória flesh** e **discos magnéticos**.

## 1. memória SRAM

SRAM (static ramdom access memory) é a memória de acesso mais rápido. Geralmente, é a que se encontra mais próxima ao
processador, ocupando até mesmo o mesmo chip que a CPU. Memórias SRAM podem armazenar dados enquanto houver energia passando
por elas. Por isso, são ditas **memórias voláreis**.

## 2. Memória DRAM

DRAM (dynamic random access memory) é uma memória de velocidade mais baixa que a SRAM. Usam um único transistor por bit de
armazenamento, por isso, são mais sensas que a SRAM e, consequentemente, mais baratas. Os dados de uma célular na DRAM são
armazenados como uma carga num capacitor, por isso, ao contrário da SRAM, não armazenam os dados indefinidamente enquanto
houver energia, e precisam de atualizações periódicas. Por isso, são chamadas de **dinâmicas**.

O acesso aos dados na DRAM é feito por linhas, e há um buffer que armazenam linhas inteiras e funcionam como uma SRAM. Por
isso, acesso abitds aleatórios proximos um ao outro é feito de forma bastante eficiente.

Além disso, as DRAMs contam com um clock interno. A vantagem disso é que não é necessário perder tempo de sincronização com o
processador. Essa tecnologia é chamada de _Synchronas DRAM_ (SDRAM). A transferência de linhas entre um banco de memória DRAM
e seu buffer é feita em um ciclo de clock.

Versões mais atuais da SDRAM transferem bits tantno no início quanto no final de um ciclo de clock; essa tecnologia chama-se
_Double Data Rate_ (DDR).

Uma memória DDR4-3200 SDRAM, por exemplo, usa a versão 4 da tecnologia e é capaz de fazer 3200 milhões de transferências
de bits (ou 3200 mb) por segundo, ou seja, tem um clock de 1600 MHz.

## 3. Memória Flash

A memória flash é do tipo EEPROM (\_Electrically Erasable Programmable Read-only Memory). Semelhantes à tecnologia
DRAM, permitem que múltiplos bits seja, lidos e esqueitos numa única operação. A diferença é que nesse tipo de memória a esquita desgasta os bits da memória flash. Por isso, elas incluem um controlador que "espalha" as operações de escrita, remapeando os blocos mais escritas da memória para os outros menos escritos. Esemplos de tecnologias que implementam memória flash são os cartões de memória microSD e os HDs SSD.
