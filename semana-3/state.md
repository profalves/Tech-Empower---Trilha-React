# State no Javascript e no React

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

## Usando `state` no React

No `React`, o _"estado"_ de uma aplicação se refere a um objeto JavaScript que armazena informações ou dados relevantes para um componente. Esses dados podem ser dinâmicos e podem ser atualizados ao longo do tempo em resposta a eventos, interações do usuário ou solicitações de dados externos. **O estado é uma parte fundamental do ciclo de vida de um componente React, pois controla como o componente é renderizado e reage a mudanças**.

O estado é gerenciado internamente pelo React e pode ser acessado e atualizado por meio de funções específicas fornecidas pelo mesmo, como o `useState` hook para componentes funcionais ou o método setState para componentes de classe. Quando o estado de um componente é atualizado, o React reage automaticamente, re-renderizando o componente e refletindo as mudanças na interface do usuário para manter a aplicação em sincronia com os dados mais recentes.

O uso de estados permite que os desenvolvedores do React criem componentes interativos e dinâmicos, onde as informações podem ser atualizadas e exibidas em tempo real, proporcionando uma experiência de usuário mais rica e responsiva.

## Mão na Massa: Trabalhando com `state` no React 

### Configurar um Projeto React

Primeiro, certifique-se de ter o `Node.js` instalado em seu sistema operacional. Em seguida, você pode criar um novo projeto React usando o Next.js, como recomendado recentemente na [documentação oficial](https://react.dev/learn/start-a-new-react-project#nextjs) 

```bash
npx create-next-app // agora vem com TypeScript por padrão.
```

**Next.js é uma estrutura React full-stack**. É versátil e permite criar aplicativos React de qualquer tamanho, desde um blog quase estático até um aplicativo dinâmico complexo. Mas se você nunca usou `Next.js`, confira o [tutorial do Next.js](https://nextjs.org/learn/foundations/about-nextjs).

Podemos usar também qualquer outra estrutura de projeto da comunidade: 

- [Remix](https://react.dev/learn/start-a-new-react-project#remix)
- [Gatsby](https://react.dev/learn/start-a-new-react-project#gatsby)
- [Expo (para aplicativos nativos)](https://react.dev/learn/start-a-new-react-project#expo)

### Entendendo o useState Hook

No React, o estado (`state`) é um dos principais conceitos. Ele permite que você armazene e gerencie dados que podem ser atualizados e refletidos na interface do usuário. O estado é usado para rastrear informações que podem mudar ao longo do tempo de vida do componente.

Para entender melhor como funciona o estado no React, pode dar um exemplo com o uso de classe. 

```js
import React, { Component } from 'react';

class App extends Component {
  constructor() {
    super();
    this.state = {
      contador: 0
    };
  }

  aumentarContador = () => {
    this.setState({ contador: this.state.contador + 1 });
  };

  diminuirContador = () => {
    this.setState({ contador: this.state.contador - 1 });
  };

  render() {
    return (
      <div>
        <h1>Contador: {this.state.contador}</h1>
        <button onClick={this.aumentarContador}>Aumentar</button>
        <button onClick={this.diminuirContador}>Diminuir</button>
      </div>
    );
  }
}

export default App;
```

O `useState` é um dos hooks mais usados. Ele permite que você adicione estado a componentes funcionais (o que antes dele só era possível com o uso de classes). O useState é uma função que retorna um par de valores: o estado atual e uma função para atualizar esse estado.

```js
const [state, setState] = useState()
```

A realizar o mesmo código acima com o useState hook agora vamos observar que o uso de `class` pode ser substituido por `function` e simplifica mais o uso do estado no código.

```js
import React, { useState } from 'react';

function App() {
  const [contador, setContador] = useState(0);

  const aumentarContador = () => {
    setContador(contador + 1);
  };

  const diminuirContador = () => {
    setContador(contador - 1);
  };

  return (
    <div>
      <h1>Contador: {contador}</h1>
      <button onClick={aumentarContador}>Aumentar</button>
      <button onClick={diminuirContador}>Diminuir</button>
    </div>
  );
}

export default App;
```

Neste exemplo, importamos o hook `useState` do `React` e o usamos dentro do componente funcional `App()`. O estado `contador` é inicializado com um valor de `0` usando `useState(0)`.

- `aumentarContador()` e `diminuirContador()` são funções que usam `setContador` para atualizar o estado.

No `return` (saída) do componente, exibimos o valor do estado contador e dois botões que chamam as funções para atualizar o estado quando clicados.

### Executar a Aplicação

Agora que você tem um componente React funcional com estado usando o `useState` hook, você pode executar sua aplicação:

```bash
npm start
```

Acesse o aplicativo em seu navegador e você verá o contador e os botões de aumento e diminuição. O estado está sendo usado para controlar o valor exibido na interface do usuário e é atualizado quando os botões são clicados.

Isso é um exemplo simples de como usar o **useState hook** no React. Você pode usar o useState para gerenciar estados em componentes funcionais de maneira simples e eficaz. À medida que você se torna mais confortável com o React, pode começar a usar o useState e outros hooks em componentes mais complexos para controlar estados mais elaborados.

## Docs

* <https://dev.to/carlosrafael22/de-volta-ao-basico-tipos-primitivos-e-objetos-em-javascript-h9g>
* <https://blog.codecasts.com.br/pure-finctions-immutability-clean-code-quality-31825b0d7516>
* <https://medium.com/opensanca/imutabilidade-eis-a-quest%C3%A3o-507fde8c6686>
* <https://www.youtube.com/watch?v=n5uiJr-v0KQ>
* <https://morioh.com/p/d86c97c99528>
* <https://kentcdodds.com/blog/dont-sync-state-derive-it>

## Desafio

Crie um relógio digital usando `useState`.