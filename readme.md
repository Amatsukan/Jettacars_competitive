# Jettacars Competitive Challenge

Bem-vindo ao Jettacars Competitive, um desafio de programação onde você deve criar um bot para gerenciar a logística e a linha de produção de uma montadora de veículos. O objetivo é tomar as melhores decisões a cada momento para maximizar o lucro.

## Como Funciona

O desafio opera em um modelo de execução **stateless (sem estado)**. A cada "tick" (turno) da simulação, seu script de bot é executado do zero, sem memória do tick anterior.

O fluxo de interação é simples:

1. **Ler o Estado:** Seu bot lê o estado atual do jogo usando a função `readline()`.
2. **Tomar uma Decisão:** Com base no estado recebido, sua lógica decide qual a melhor ação a ser tomada.
3. **Enviar um Comando:** Seu bot envia um único comando de ação para o motor do jogo usando a função `print()`.

## Como Começar

1. **Clone o Repositório:**  
```  
git clone https://github.com/Amatsukan/Jettacars_competitive.git  
```
2. **Abra o Juiz:** Abra o arquivo `index.html` em seu navegador.
3. **Desenvolva seu Bot:** Escreva seu código JavaScript em um editor de sua preferência.
4. **Cole seu Código:** Cole o código do seu bot na área de texto designada na página e inicie a simulação.
5. **Observe e Depure:** Acompanhe a execução e as mensagens de debug no log de eventos da página.

## API de Interação do Bot

Seu bot interage com o jogo através de três funções globais:

### `readline()`

Lê o estado atual do jogo. Retorna uma **string JSON** que você deve parsear.

```
const gameState = JSON.parse(readline());


```

### `print(command)`

Envia um comando de ação para o motor do jogo. O comando deve ser uma string formatada. Apenas um comando de ação é processado por tick.

**Exemplos de Comandos:**

* `request_truck <tamanho> <id_da_doca>` \- Solicita um caminhão de um determinado tamanho para uma doca vazia.
* `assemble <tipo_modelo> <id_da_doca>` \- Inicia a montagem de um modelo de veículo em uma doca.
* `load <id_da_doca>` \- Carrega um veículo pronto da doca para o caminhão.
* `dispatch <id_da_doca>` \- Despacha um caminhão pronto para entrega.

### `debug(message)`

Envia uma mensagem de debug para o log de eventos. Não afeta o jogo e é útil para acompanhar a lógica do seu bot.

```
debug(`Analisando ${gameState.orders.length} pedidos...`);


```

## Estrutura do `gameState`

O objeto `gameState` contém todas as informações que seu bot precisa para tomar decisões. A estrutura detalhada pode ser encontrada no documento [**Documentação da Estrutura gameState**](https://github.com/Amatsukan/Jettacars_competitive/blob/main/game-state.md).

**Resumo das Chaves Principais:**

* `tick`: O turno atual.
* `profit`: O lucro acumulado.
* `orders`: Lista de pedidos dos clientes.
* `availableParts`: Lista de peças disponíveis para montagem.
* `docks`: Lista de docas, que podem conter caminhões e informações sobre a montagem.

## Bot de Exemplo (Ponto de Partida)

Este bot simples lê o estado do jogo e solicita um caminhão na primeira doca vazia que encontrar.

```
// Bot de Exemplo para Jettacars Competitive

// 1. Ler o estado do jogo
const gameState = JSON.parse(readline());
debug(`Tick ${gameState.tick}: Iniciando análise.`);

// 2. Lógica de Decisão
const docks = gameState.docks || [];
const trucks = docks.map(d => d.truck).filter(t => t !== null);

// Se não houver caminhões e houver pedidos, peça um caminhão.
if (trucks.length === 0 && (gameState.orders || []).length > 0) {
    const emptyDock = docks.find(d => d.status === 'empty');
    if (emptyDock) {
        debug(`Nenhum caminhão encontrado. Solicitando um para a doca #${emptyDock.id}.`);
        print(`request_truck small ${emptyDock.id}`);
    } else {
        debug("Nenhuma doca vazia para solicitar um caminhão.");
    }
} else {
    debug("Nenhuma ação tomada neste tick.");
}


```

Agora, o desafio é seu. Boa sorte!
