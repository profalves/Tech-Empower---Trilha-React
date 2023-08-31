# Uma Reintrodução ao Javascript - Parte 2

## Objetos Personalizados

> **Nota:** Para uma discursão mais detalhada de programação orientada a objetos em JavaScript, veja [Introdução a JavaScript Orientado a Objeto](https://developer.mozilla.org/pt-BR/docs/Learn/JavaScript/Objects).

Na clássica Programação Orientada a Objetos, objetos são coleções de dados e métodos que operam sobre esses dados. JavaScript é uma linguagem baseada em protótipos que não contém a estrutura de classe, como tem em C++ e Java. *(Algumas vezes isso é algo confuso para o programador acostumado a linguagens com estrutura de classe)*. Em vez disso, **o JavaScript usa funções como classes**. Vamos considerar um objeto pessoa com os campos primeiro e último nome. Há duas formas em que o nome talvez possa ser exibido: como `"primeiro nome segundo nome"` ou como `"último nome, primeiro nome"`. Usando as funções e objetos que discutimos anteriormente, aqui está uma forma de fazer isso:

```js
function makePerson(first, last) {
    return {
        first: first,
        last: last
    }
}
function personFullName(person) {
    return person.first + ' ' + person.last;
}
function personFullNameReversed(person) {
    return person.last + ', ' + person.first
}
const s = makePerson("Simon", "Willison");
personFullName(s) // Simon Willison
personFullNameReversed(s) // Willison, Simon
```

Isso funciona, mas é muito feio. Você termina com dúzias de funções em seu escopo global. O que nós realmente precisamos é uma forma de anexar uma função a um objeto. Visto que funções são objetos, isso é fácil:

```js
function makePerson(first, last) {
    return {
        first: first,
        last: last,
        fullName: function() {
            return this.first + ' ' + this.last;
        },
        fullNameReversed: function() {
            return this.last + ', ' + this.first;
        }
    }
}
const s = makePerson("Simon", "Willison")
s.fullName() // Simon Willison
s.fullNameReversed() // Willison, Simon
```

Há algo aqui que não havíamos visto anteriormente: a palavra-chave '[`this`](/pt-BR/JavaScript/Reference/Operators/this)'. Usada dentro de uma função, '`this`' refere-se ao objeto corrente. O que aquilo de fato significa é especificado pelo modo em que você chamou aquela função. Se você chamou-a usando [notação ponto ou notação colchete](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/Property_accessors) em um objeto, aquele objeto torna-se '`this`'. Se a notação ponto não foi usada pela chamada, '`this`' refere-se ao objeto global. Isso é uma frequente causa de erros. Por exemplo:

```js
const s = makePerson("Simon", "Willison")
const fullName = s.fullName;
fullName() // undefined undefined 😵‍💫
```

Quando chamamos `fullName()`, '`this`' está ligado ao objeto global. Visto que não há variáveis globais chamadas `first` ou `last` obtemos `undefined` para cada um.

Podemos tirar vantagem da palavra chave '`this`' para melhorar nossa função `makePerson`:

```js
function Person(first, last) {
  this.first = first;
  this.last = last;
  this.fullName = function () {
    return this.first + " " + this.last;
  };
  this.fullNameReversed = function () {
    return this.last + ", " + this.first;
  };
}
const s = new Person("Simon", "Willison");
```

Nós introduzimos uma outra palavra-chave: '[`new`](/pt-BR/JavaScript/Reference/Operators/new)'. `new` é fortemente relacionada a '`this`'. O que ele faz é criar um novo objeto vazio, e então chamar a função especificada com '`this`' para atribuir aquele novo objeto. Funções que são desenhadas para ser chamadas pelo '`new`' são chamadas de funções construtoras. Uma prática comum é capitular essas funções como um lembrete de chamá-las com o `new`.

Nossos objetos pessoa estão ficando melhor mas ainda existem algumas arestas feias. Toda vez que criamos um objeto pessoa, criamos duas marcas de nova função dentro dele — não seria melhor se este código fosse compartilhado?

```js
function personFullName() {
  return this.first + " " + this.last;
}
function personFullNameReversed() {
  return this.last + ", " + this.first;
}
function Person(first, last) {
  this.first = first;
  this.last = last;
  this.fullName = personFullName;
  this.fullNameReversed = personFullNameReversed;
}
```

Assim está melhor? Estamos criando as funções de método apenas uma vez, e atribuimos referências para elas dentro do construtor. Podemos fazer algo melhor do que isso? A resposta é sim:

```js
function Person(first, last) {
  this.first = first;
  this.last = last;
}
Person.prototype.fullName = function () {
  return this.first + " " + this.last;
};
Person.prototype.fullNameReversed = function () {
  return this.last + ", " + this.first;
};
```

`Person.prototype` é um objeto compartilhado por todas as instâncias de `Person`. Este forma parte da cadeia de buscas (que tem um nome especial, cadeia de protótipos ou "prototype chain"): toda a vez que você tentar acessar uma propriedade de `Person` que não está configurada, Javascript irá verificar em `Person.prototype` para ver se esta propriedade existe por lá. Como resultado, qualquer coisa atribuída à `Person.prototype` torna-se disponível para todas as instâncias deste construtor, através do objeto `this`.

Esta é uma ferramenta incrivelmente poderosa. JavaScript permite a você modificar algo prototipado em qualquer momento no seu programa. Isto significa que você pode adicionar métodos extras para objetos pré-existentes, em tempo de execução:

```js
const s = new Person("Simon", "Willison");
s.firstNameCaps();
// TypeError on line 1: s.firstNameCaps is not a function

Person.prototype.firstNameCaps = function() {
    return this.first.toUpperCase()
}
s.firstNameCaps() // SIMON
```

Curiosamente, você pode também adicionar coisas para o protótipo de objetos built-in de Javascript. Vamos adicionar um método para `String` que retorna a string invertida:

```js
const s = "Simon";
s.reversed()
//TypeError on line 1: s.reversed is not a function

String.prototype.reversed = function() {
    let start = 0, end = this.length - 1
    result = new Array(this.length)
    while(start <= end) {
      result[start] = this[end]
      result[end] = this[start]
      start++, end--
    }
    return result.join('')
}
s.reversed() // nomiS
```

Nosso novo método funciona inclusive em string literais!

```js
"This can now be reversed".reversed()
desrever eb won nac sihT
```

Como antes mencionado, o protótipo forma parte de uma cadeia. A raiz dessa cadeia é `Object.prototype`, dos quais inclui o método `toString()` — este é o método que é chamado quando você tenta representar um objeto como uma string. Isto é útil para depurar os nossos objetos `Person`:

```js
const s = new Person("Simon", "Willison");
console.log(s.toString()) // [object Object]

Person.prototype.toString = function() {
    return '<Person: ' + this.fullName() + '>';
}

console.log(s.toString()) // <Person: Simon Willison>
```

## Contexto

Em JavaScript, **contexto** refere-se a um objeto. Dentro de um objeto, a palavra-chave **this** refere-se a esse objeto (ou seja, **“self”**) e fornece uma interface para as propriedades e métodos que são membros desse objeto. Quando uma função é executada, a palavra-chave *“this”* refere-se ao objeto no qual a função é executada.

### Aqui estão alguns cenários

- Quando uma função é executada no contexto global, **“this”** refere-se ao objeto **global** (nodejs) ou **window** (browsers)

```javascript
window === this // true
```

- Quando uma função é um método de um objeto, **“this”** refere-se a esse objeto (a menos que seja executado manualmente no contexto de um objeto diferente)

```javascript
var nome = "Paulo"
var sobrenome = "Viana"

var Pessoa = {
  nome: "José",
  sobrenome: "Augusto",
  exibeNomeCompleto: function() {
    return `${this.nome} ${this.sobrenome}`
  },
}

var exibeNomeCompleto = Pessoa.exibeNomeCompleto

console.log(Pessoa.exibeNomeCompleto()) // José Augusto
console.log(exibeNomeCompleto()) // Paulo Viana
```

- Quando você instancia uma função construtora, dentro do objeto de instância, **“this”** refere-se ao objeto de instância

```javascript
function Carro(marca, modelo, ano){
    this.marca = marca
    this.modelo = modelo
    this.ano = ano
}

const carro1 = new Carro('Charger', 'RT', 1970)

console.log(carro1)
```

- Como um manipulador de eventos DOM

Quando uma função é usada como um manipulador de eventos, seu this está definido para o elemento do evento a partir do qual foi disparado (alguns navegadores não seguem essa convenção para os listeners adicionados dinamicamente com métodos que não sejam addEventListener).

```javascript
// Quando chamado como listener, transforma o elemento 'blue'
function bluify(e){
  // sempre true
  console.log(this === e.currentTarget);
  // true quando currentTarget e target são o mesmo objeto
  console.log(this === e.target);
  this.style.backgroundColor = '#A5D9F3';
}

// Obtém uma lista de todo elemento no documento
const elements = document.getElementsByTagName('*');

// Adiciona bluify com um click listener (escutador de click)
// para que quando o elemento seja clicado se torne azul
for(let i=0 ; i<elements.length ; i++){
  elements[i].addEventListener('click', bluify, false);
}
```

> **Nota**: O mesmo pode ocorrer para um manipulador de evento in-line

```html
<button onclick="alert(this.tagName.toLowerCase());">
  Show this
</button>
```

A palavra-chave `this` comporta-se um pouco diferente em Javascript se comparado com outras linguagens. Também possui algumas diferenças entre o *modo estrito* e o *modo não estrito*.

O comportamento do `this` depende, então do:

1. Escopo e Contexto
2. Modo Estrito

### 1. Escopo vs Contexto

- **Escopo** (scope)

  - **Acesso** às variáveis, funções e objetos numa parte do código, durante o tempo de execução.
  - Determina a **visibilidade** desses recursos em alguma parte do código.
  - Sempre invocamos uma função, estamos criando um novo `scope`

- **Contexto** (context)

  - Enquanto o `scope` se refere às diversas variáveis, o `context` se refere ao valor do `this` no mesmo `scope`
  - Pode ser mudado com funções especiais como: `.apply()`, `.call()` e `.bind()`
  - No contexto de execução (execution context), o contexto já não é mais esse contexto que estamos discutindo. Ele será o `scope`

Temos 3 `scopes`:

1. **Global**
   1. No momento que começamos a escrever código, estamos nesse contexto.
   2. Existe enquanto existir a aplicação
2. **Local**
   1. Dentro de alguma função, variáveis estão no escopo (contexto) local.
   2. Existe equanto existir a função ou o objeto.
3. **Bloco**
   1. Quando elimitado com a declaração `let` ou `const`.

### 2. Modo Estrito (strict mode)

A diretiva "use strict" em JavaScript ativa o modo estrito, que ajuda a identificar e prevenir erros comuns e comportamentos indesejados. Ele:

- Muda a semântica do javascript
- É opcional
- `"use strict"` é habilitado para o contexto
- Elimina alguns erros silênciosos
- Evita algumas confusões

Aqui estão alguns erros que podem ser evitados com o uso de "use strict", juntamente com exemplos de código:

1. **Declaração de Variáveis Não Intencionais:**

Sem o modo estrito, você pode criar variáveis sem usar `var`, `let` ou `const`, o que pode levar a variáveis globais indesejadas.

```js
// Sem strict mode
x = 10;

console.log(x); // 10

// Com strict mode
"use strict";
y = 20; // Isso lançará um erro: y is not defined
```

2. **Atribuição a Propriedades de Só Leitura:**

Em modo estrito, você não pode atribuir valores a propriedades de objetos somente leitura.

```js
"use strict";
const objeto = {};
Object.defineProperty(objeto, "nome", { value: "Exemplo", writable: false });

objeto.nome = "Novo Exemplo"; // Isso lançará um erro: Cannot assign to read-only property 'nome' of object
```

3. **Deleção de Variáveis, Funções e Argumentos:**

No modo estrito, você não pode deletar variáveis, funções ou argumentos.

```js
"use strict";
var a = 10;
delete a; // Isso lançará um erro: Delete of an unqualified identifier in strict mode.

function foo() {
  delete arguments[0]; // Isso lançará um erro: Delete of an unqualified identifier in strict mode.
}
```

4. **Palavras Reservadas como Nomes de Variáveis:**

O modo estrito torna mais restritas as palavras que podem ser usadas como nomes de variáveis, evitando conflitos.

```js
"use strict";
var undefined = 5; // Isso lançará um erro: Unexpected strict mode reserved word
```

5. **Parâmetros Duplicados:**
No modo estrito, você não pode ter parâmetros duplicados em uma função.

```js
"use strict";
function soma(a, b, a) { // Isso lançará um erro: Duplicate parameter name not allowed in this context
  return a + b + a;
}
```

### Métodos de contexto

#### Call

O método call é responsável por executar uma função e em seu primeiro parâmetro é passado o contexto que será aplicado nesta execução, por exemplo:

```javascript
var nome = "Rodrigo"
var sobrenome = "Alves"

var Pessoa = {
  nome: "José",
  sobrenome: "Augusto",
  exibeNomeCompleto: function() {
    return `${this.nome} ${this.sobrenome}`
  },
}

console.log(Pessoa.exibeNomeCompleto.call(window)) // Rodrigo Alves
console.log(Pessoa.exibeNomeCompleto.call(Pessoa)) // José Augusto
```

Dado os exemplos anteriores, agora permitindo você configurar o '`this`'.

```js
function lastNameCaps() {
  return this.last.toUpperCase();
}
var s = new Person("Simon", "Willison");
lastNameCaps.call(s);
// Is the same as:
s.lastNameCaps = lastNameCaps;
s.lastNameCaps();
```

### Apply

O método apply é basicamente a mesma coisa que o método call, com a única diferença que ele aceita apenas dois parâmetros (no método call você pode passar qualquer número de parâmetros) sendo o primeiro o contexto em que será executada a função e o segundo um array com os parâmetros.

```javascript
var Calculadora = {
  soma: function(...numeros) {
    return numeros.reduce((acc, cur) => acc + cur, 0)
  },
}

var numeros = Array(8)
  .fill(1)
  .map((_, index) => index + 1)

Calculadora.soma(1, 1, 2) // 4
Calculadora.soma.apply(window, numeros) // 36
```

Nós podemos revisitar os exemplos anteriores agora. O primeiro argumento para `apply()` é o objeto que deve ser tratado como '`this`'. Por exemplo, aqui está uma implementação diferenciada de '`new`':

```js
function self(constructor, ...args) {
  const o = {}; // Create an object
  constructor.apply(o, args);
  return o;
}
```

`apply()` é difícil de ilustrar — não é algo que você usa com frequência, mas é útil conhecer a respeito. No código acima, `...args` é chamado de "[arguments rest](/pt-BR/docs/Web/JavaScript/Reference/Functions/rest_parameters)" — como o nome indica, isto contém o restante dos parâmetros.

Ao chamar

```js
var bill = self(Person, "Willian", "Orange");
```

é equivalente a

```js
var bill = new Person("Willian", "Orange");
```

### Bind

O método bind é um pouco diferente, mas nem tanto. A diferença é que ele não executa a função, porém retorna sua declaração contendo o contexto passado a ele como parâmetro. Usando um dos exemplos anteriores, temos:

```javascript
const nome = "Jorge"
const sobrenome = "Luiz"

const Pessoa = {
  nome: "Washington",
  sobrenome: "Luis"
  saudacoes: function(cidade, sigla) {
    return `Olá ${this.nome} ${this.sobrenome}, bem-vindo a ${cidade}/${sigla}`
  },
}

const saudarAlguem = Pessoa.saudacoes.bind(window)
saudarAlguem("Salvador", "BA") // // Olá Jorge Luiz, bem-vindo a Salvador/BA
```

## Clausuras (Closures)

Em JavaScript é permitido declarar uma função dentro de outras funções. Nós já vimos isso antes, com uma versão preliminar da função `makePerson()`. Um detalhe importante sobre funções aninhadas em JavaScript é que elas podem acessar as variáveis do escopo das funções parente:

```js
function parentFunc() {
  var a = 1;

  function nestedFunc() {
    var b = 4; // parentFunc can't use this
    return a + b;
  }
  return nestedFunc(); // 5
}
```

Isto permite um compromisso maior de utilidade ao escrever um código de melhor mantenibilidade. Se uma função depende de uma ou mais funções que não são úteis para outras parte do seu código, você pode aninhar estas funções utilitárias dentro da função que será chamada. Isto mantém o número de funções que estão no escopo global baixo, o que é sempre uma boa coisa.

Isto é também um ótimo contador de atração de variáveis globais. Quando se escreve um código complexo, é sempre tentador usar as variáveis globais para compartilhar valores entre múltiplas funções — do qual leva a um código que é difícil de manter. Funções aninhadas podem compartilhar variáveis em seus parentes, então você pode usar este mecanismo para acoplar e juntar funções, quando isto fizer sentido, sem poluir o seu "namespace" global — 'globais locais' se preferir. Esta técnica deve ser usada com cautela, mas é uma habilidade a se ter.

Um exemplo real, simples e muito usado de funções aninhadas em JavaScript é a criação de um contador utilizando closures. Vamos criar uma função externa que cria e retorna uma função interna para contar o número de vezes que a função interna é chamada. Isso ilustra como as funções aninhadas podem criar um escopo fechado para armazenar informações enquanto mantêm acesso às variáveis da função externa.

Aqui está o código:

```js
function criaContador() {
  let contador = 0;

  function incrementaContador() {
    contador++;
    console.log(`Contador: ${contador}`);
  }

  return incrementaContador;
}

const contador1 = criaContador();
contador1(); // Output: Contador: 1
contador1(); // Output: Contador: 2

const contador2 = criaContador();
contador2(); // Output: Contador: 1
```

Isto nos leva a uma das abstrações mais poderosas que JavaScript tem a oferecer — mas também a mais potencionalmente confusa. O que isto faz?

```js
function makeAdder(a) {
  return function (b) {
    return a + b;
  };
}
const x = makeAdder(5);
const y = makeAdder(20);
x(6); // ?
y(7); // ?
```

O nome da função `makeAdder` já diz tudo: ela cria novas funções 'adder', na qual, quando chamada com um argumento, adiciona o argumento com a que foi criada.

O que está acontecendo aqui é muito parecido com o que estava acontencedo com as funções internas vistas anterioremente: uma função definida dentro de uma outra função tem acessso às variáveis da função de fora. A única diferença aqui é que a função de fora retornou e, como consequência do senso comum, deve dizer que todas as variáveis locais não existem mais. Mas elas _ainda_ existem — caso contrário a função adicionadora não seria capaz de funcionar. Mais ainda, há duas "cópias" diferentes de variáveis locais para `makeAdder` — uma na qual o `a` é 5 e a outra na qual `a` é 20. Então, o resultado dessas chamadas de funções é o seguinte:

```js
x(6); // returns 11
y(7); // returns 27
```

Eis o que acontece na verdade: sempre que o JavaScript executa uma função, um objeto de 'escopo' é criado para guardar as variáveis locais criadas dentro desta função. Ela é inicializada com quaisquer variáveis passadas como parâmetros da função. Isto é similar ao objeto global, em que todas as variáveis globais e funções vivem, mas com algumas diferenças importantes: primeiro, um novo objeto de escopo é criado toda a vez que uma função começa a executar, e segundo, diferente do objeto global (que nos navegadores é acessado com `window`) estes objetos não podem ser diretamente acessados através do seu código JavaScript. Não há nenhum mecanismo para iterar sobre as propriedades do escopo corrente do objeto, por exemplo.

Então, quando `makeAdder` é chamado, um objeto de escopo é criado com uma única propriedade: `a`, a qual é o argumento passado para a função `makeAdder`. `makeAdder` então retorna uma nova função criada. Normalmente o coletor de lixo de JavaScript poderia limpar o objeto de escopo criado para `makeAdder` neste ponto, mas a função de retorno mantém uma referência ao objeto de escopo. Como resultado, o objeto de escopo não será coletado como lixo até que não haja mais referências para função objeto que `makeAdder` retornou.

Objetos de escopo formam uma cadeia chamada de cadeia de escopos, similar a cadeia de protótipos usadas no sistema de objetos de JavaScript.

Uma clausura é a combinação de uma função e o objeto de escopo na qual é criado. Clausuras permitem você guardar estado — de tal forma, elas podem ser frequentemente utilizadas no lugar de objetos.

## Docs

- <https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Language_Overview>

## Desafio

Acesse este repositório: <https://github.com/profalves/calculadora-template-sample>