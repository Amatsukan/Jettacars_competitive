# Documentação da Estrutura `gameState`

Este documento descreve a estrutura do objeto `gameState` que é recebido a cada tick do jogo.

### Objeto Raiz: `gameState`

O objeto principal que contém todo o estado do jogo.

| **Chave**      | **Tipo**        | **Descrição**                                                                                       |
| -------------- | --------------- | --------------------------------------------------------------------------------------------------- |
| tick           | Number          | O número do turno (tick) atual da simulação.                                                        |
| profit         | Number          | O lucro total acumulado até o momento.                                                              |
| peakProfit     | Number          | O lucro máximo atingido durante a simulação.                                                        |
| tickCost       | Number          | O custo operacional do tick atual.                                                                  |
| inactiveTicks  | Number          | O número de ticks consecutivos em que nenhuma ação foi tomada.                                      |
| lossTicks      | Number          | O número de ticks consecutivos com lucro negativo.                                                  |
| orders         | Array<Order>    | Uma lista de todos os pedidos de clientes ativos. Veja a estrutura Order abaixo.                    |
| availableParts | Array<Part>     | Uma lista de todas as peças disponíveis no chão de fábrica. Veja a estrutura Part abaixo.           |
| docks          | Array<Dock>     | Uma lista de todas as docas disponíveis para montagem e carregamento. Veja a estrutura Dock abaixo. |
| log            | Array<LogEntry> | Um histórico de eventos da simulação.                                                               |

### Estrutura `Order`

Representa um pedido de um cliente.

| **Chave** | **Tipo** | **Descrição**                                                                                           | **Exemplo**  |
| --------- | -------- | ------------------------------------------------------------------------------------------------------- | ------------ |
| id        | Number   | Identificador único do pedido.                                                                          | 120          |
| models    | Object   | Um objeto onde as chaves são os tipos de modelo (ex: "small") e os valores são a quantidade necessária. | {"small": 2} |
| maxProfit | Number   | O lucro máximo que pode ser obtido com este pedido se entregue no prazo.                                | 4000         |
| deadline  | Number   | O tick em que o pedido expira.                                                                          | 70           |

### Estrutura `Part`

Representa uma peça de veículo disponível para montagem.

| **Chave** | **Tipo** | **Descrição**                | **Exemplo**          |
| --------- | -------- | ---------------------------- | -------------------- |
| id        | String   | Identificador único da peça. | "ch\_233"            |
| type      | String   | O tipo da peça.              | "chassis\_3\_wheels" |

### Estrutura `Dock`

Representa uma doca de trabalho para montagem ou carregamento.

| **Chave** | **Tipo**       | **Descrição**                                                          | **Valores Possíveis (Exemplos)** |
| --------- | -------------- | ---------------------------------------------------------------------- | -------------------------------- |
| id        | Number         | Identificador único da doca.                                           | 1                                |
| status    | String         | O estado atual da doca.                                                | "empty", "assembling", "loading" |
| truck     | Object \| null | O objeto do caminhão que está na doca. É null se a doca estiver vazia. |                                  |

### Estrutura `Truck` (Dentro de `Dock`)

Representa um caminhão presente em uma doca.

| **Chave**       | **Tipo** | **Descrição**                                                     |
| --------------- | -------- | ----------------------------------------------------------------- |
| id              | Number   | Identificador único do caminhão.                                  |
| size            | String   | O tamanho/capacidade do caminhão (ex: "small").                   |
| contents        | Array    | Uma lista dos veículos montados que foram carregados no caminhão. |
| readyToDispatch | Boolean  | Indica se o caminhão está pronto para ser despachado.             |
