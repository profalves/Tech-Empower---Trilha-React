# State no Javascript

## Voltando ao início de tudo: Tipos Primitivos X Não Primitivos

O por quê do javascript ser uma linguagem de tipagem dinamica? Qual é a diferença entre tipos primitivos e os não-primitivos? O que é conceito particular do javascript e geral em outras linguagens neste assunto?

## Imutabilidade

Imutabilidade em seu conceito mais simples é algo que não pode ser alterado. No conceito de programação, especialmente em linguagens funcionais como JavaScript, os dados imutáveis têm várias vantagens, como prevenção de efeitos colaterais indesejados e facilitação da previsibilidade do código. No JavaScript, os tipos primitivos (como números, strings e booleanos) são imutáveis por natureza, o que significa que seus valores não podem ser alterados após a criação. No entanto, quando se trata de objetos e arrays, que são tipos de referência, é possível alcançar imutabilidade usando abordagens específicas. A imutabilidade se aplica normalmente a objetos que não podem ter seu estado modificado após serem criados, mas isso não garante que os seus valores serão sempre os mesmos.

Evita que erros assim aconteçam:

```javascript
// Exemplo: Referência Compartilhada
let person1 = { name: 'Alice' };
let person2 = person1; // person2 agora referencia o mesmo objeto

person2.name = 'Bob'; // Isso também afeta o objeto person1

console.log(person1.name); // Output: 'Bob'
console.log(person2.name); // Output: 'Bob'
```

Esta é a principal razão pelo qual um estado jamais deverá ser alterado diretamente.

## O que é state (estado) no Javascript?

Gerenciamento de estados tem sido um hype da atualidade frontend, principalmente se você está envolvido com Javascript e toda velocidade do nosso ecossistema gigantesco, com certeza, pelo menos já se deparou com algo sobre.

O conceito geral de estado na programação é uma fonte única de verdade. Na época do JQuery, o html era o state da aplicação e o que os famosos frameworks fizeram foi trazer o state para o javascript para gerenciá-lo e facilitar o reloading, melhorando a experiência de desenvolvimento.

Aprender a trabalhar com estados (ou state) em JavaScript puro é fundamental para a criação de aplicações interativas. O conceito de estado refere-se aos dados que podem mudar ao longo do tempo e influenciar a renderização da interface do usuário. Aqui está um tutorial passo a passo sobre como trabalhar com estados usando JavaScript puro:

Vamos criar um exemplo simples de uma contagem, onde clicar em um botão aumentará o valor da contagem exibida na página.

1. **Preparação do HTML:**

Comece criando um arquivo HTML básico que inclua um botão e um elemento para mostrar a contagem. O HTML deve ter um identificador para o elemento da contagem, por exemplo, `id="count-display"`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>State com JavaScript Puro</title>
</head>
<body>
  <h1>Contagem: <span id="count-display">0</span></h1>
  <button id="increment-btn">Incrementar</button>

  <script src="app.js"></script>
</body>
</html>
```

2. **JavaScript para manipulação do estado:**
Crie um arquivo app.js (conforme referenciado no `<script>` no HTML acima) para lidar com a lógica de estado e interatividade.

```js
// Selecionar elementos do DOM
const countDisplay = document.getElementById('count-display');
const incrementBtn = document.getElementById('increment-btn');

// Inicialização do estado
let count = 0;

// Função para atualizar a exibição da contagem
function updateCountDisplay() {
  countDisplay.textContent = count;
}

// Manipulador de clique para o botão
incrementBtn.addEventListener('click', () => {
  // Atualizar o estado
  count++;
  
  // Atualizar a exibição
  updateCountDisplay();
});
```

Neste exemplo, começamos selecionando os elementos do DOM que precisamos manipular. Em seguida, inicializamos o estado (aqui, a variável count) com o valor inicial de 0. Criamos a função updateCountDisplay para atualizar a exibição da contagem no elemento HTML.

O evento de clique no botão (addEventListener) dispara a função que incrementa o estado da contagem e, em seguida, chama a função updateCountDisplay para refletir essa mudança na interface do usuário.

Para ter uma melhor manutenção e organização do código, podemos usar a classe no javascript 

```js
class CounterApp {
  constructor() {
    // Selecionar elementos do DOM
    this.countDisplay = document.getElementById('count-display');
    this.incrementBtn = document.getElementById('increment-btn');

    // Inicialização do estado
    this.count = 0;

    // Vincular métodos de classe ao objeto
    this.updateCountDisplay = this.updateCountDisplay.bind(this);
    this.handleIncrementClick = this.handleIncrementClick.bind(this);

    // Adicionar ouvintes de eventos
    this.incrementBtn.addEventListener('click', this.handleIncrementClick);

    // Atualizar a exibição inicial
    this.updateCountDisplay();
  }

  updateCountDisplay() {
    this.countDisplay.textContent = this.count;
  }

  handleIncrementClick() {
    // Atualizar o estado
    this.count++;

    // Atualizar a exibição
    this.updateCountDisplay();
  }
}

// Iniciar a aplicação
const counterApp = new CounterApp();
```

### Controlando o estado

Como Vanilla é cada um por si, não tem maneira certa pra fazer não... se quiser usar classes, desde que seus testes cubram bem a usabilidade, por que não? A diferença entre ser mais e menos funcional é só como vai guardar as informações.

Se quiser algo mais funcional é só ficar mais atento em relação a imutabilidade (e evitar usar os this).

```javascript
// factory
const createStore = function (initialState = {}) {
  let state = initialState

  return {
    get: () => state,
    update: (newState) => {
      // usando como exemplo um shallow copy, mas você pode utilizar algum algoritmo
      // ou algo pronto pra deep copy, atentando em relação a imutabilidade
      state = {
        ...state,
        ...newState
      }
    },
    unset: (key) => {
      const { [key]: _, ...rest } = state
      state = rest
    }
  }
}

const myStore = createStore(/* tua store inicial */)

// e ficar atento a sempre chamar `myStore.get` pra pegar sempre
// a store atualizada, e repassando o `myStore` pra onde precisar.
```

### Implementações

Em Javascript nós possuímos algumas implementações famosas que gerenciam estados. A que eu acredito ser a mais conhecida, é a [Redux](https://redux.js.org/), que se define como uma máquina de estados previsíveis para aplicativos JavaScript. O objetivo dela é te ajudar a escrever aplicações que se comportam de forma consistente, funcionam em diferentes ambientes de desenvolvimento, como clientes, servidores e ambientes nativos que são fáceis de serem testadas. Você pode usar o Redux diretamente com **React** ou de forma **agnóstica** em qualquer aplicação JS.

Outra implementação conhecida atualmente é exclusiva para **VueJS**, a [Vuex](https://vuex.vuejs.org/ptbr/). Ela se define como um padrão de gerenciamento de estados que serve como um centralizador das informações para todos os componentes. Suas regras internas garantem que o estado da aplicação só possa ser mudado de forma previsível. Assim como Redux, Vuex possui uma extensão para o DevTools do Firefox e do Chorme. Além de você ‘viajar no tempo’ da aplicação, você pode exportar e importar estados prévios.

A terceira mais famosa implementação de gerência de estados (e a mais recente) é a [ngRX](https://ngrx.io/) (que é a junção do Redux com RxJS) e é exclusiva para **Angular**. ***Aliás é nela que precisaremos focar em nossos estudos***. A biblioteca elenca suas qualidades começando pelo fato de que o estado é uma estrutura de dados única e imutável, que as ações definem as mudanças no estado, e que as funções responsáveis pelas alterações no estado são puras. Como por baixo do capô dela nós temos o Redux, quem já está acostumado com ela e está trabalhando com Angular, não terá problema algum em usar a biblioteca. Da mesma maneira, você poderá usar a extensão do Redux para o DevTools dos navegadores.

### Mão na Massa: Gerenciamento De Estado Simples Em JavaScript Com Nanny State

Vamos realizar uma breve implementação com [Nanny State](https://github.com/daz4126/Nanny-State), que é uma pequena biblioteca para ajudar a criar aplicativos da Web baseados em estado usando JS puro. É semelhante ao React, mas com muito menos sobrecarga e nova sintaxe simples para aprender. Ele também usa um único objeto de estado em todo o aplicativo em vez de cada componente individual ter seu próprio estado. Foi inspirado no [HyperApp](https://github.com/jorgebucaran/hyperapp) e tem muitas semelhanças com o [Elm](https://elm-lang.org/).

*Nanny State* usa um modelo de fluxo de dados unidirecional, composto de 3 partes:

* State  – um objeto que armazena todos os dados do aplicativo
* View  – uma função que retorna uma string de HTML com base no estado atual
* Update  – uma função que é a única maneira de alterar o estado e renderizar novamente a exibição

![Nanny State](https://user-images.githubusercontent.com/16646/171490664-2859a7ea-f4c4-47c3-8b96-edfa6f196bbb.png)

Segue abaixo um exemplo simples de um componente usando essa biblioteca:

```javascript
import { Nanny,html } from 'https://cdn.skypack.dev/nanny-state';

const View = state => html`<h1>Hello ${state.name}</h1>`

const State = {
  name: "Nanny State",
  View
}

const Update = Nanny(State)
```

## Docs

* <https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Data_structures>
* <https://developer.mozilla.org/pt-BR/docs/Glossary/Primitive>
* <https://dev.to/carlosrafael22/de-volta-ao-basico-tipos-primitivos-e-objetos-em-javascript-h9g>
* <https://blog.codecasts.com.br/pure-finctions-immutability-clean-code-quality-31825b0d7516>
* <https://medium.com/opensanca/imutabilidade-eis-a-quest%C3%A3o-507fde8c6686>
* <https://www.youtube.com/watch?v=n5uiJr-v0KQ>
* <https://morioh.com/p/d86c97c99528>

## Desafio

Crie uma simulação simples de carrinho de compras de loja virtual. Use o state para armazenar informações e poder calcular o valor total do pedido.