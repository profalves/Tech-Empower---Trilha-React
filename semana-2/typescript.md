# TypeScript: Introdução e Principais Conceitos

## O que é TypeScript?

**TypeScript** é uma linguagem criada pela Microsoft que adiciona tipagem estática ao JavaScript que por padrão é uma linguagem que possui tipagem dinâmica, ou seja, as variáveis e funções podem assumir tipos distintos durante o tempo de execução.

Vale lembrar o código TypeScript é utilizando somente em ambiente de desenvolvimento e é totalmente convertido para JavaScript no processo de build de produção, ou seja, o navegador ou o Node lerão somente código JS no fim das contas.

### Vantagens do TypeScript

- Descobrir erros durante o desenvolvimento;
- Recursos adicionais, tais como de incrementar a inteligência (IntelliSense) à IDE que estamos utilizando;
- Transpilação do código para que o mesmo seja lido por todas versões de browsers.

## Instalação do TypeScript
Para começar a usar o TypeScript, você precisa tê-lo instalado globalmente em sua máquina, o que pode ser feito através do Node.js e do gerenciador de pacotes npm. Abra o terminal e execute o seguinte comando:

```bash
npm i -g typescript #or yarn global add typescript
```

É muito recomendado usar o TS a partir do seu node-package, ou seja, instalando como dependencia de desenvolvimento:

```bash
npm i typescript --save-dev #or yarn add typescript -D
```

## Tipos Básicos

Para tipar variáveis adicionamos o `:tipo` após sua declaração.

```ts
let text:string = 'texto';
```

### Boolean

O tipo mais básico de dados são os valores **verdadeiro/falso**, que JavaScript e TypeScript chamam de valor booleano (`boolean`).

```ts
let isMobile: boolean = false
```

### Number

Assume qualquer número, como inteiro ou ponto flutuante.

```ts
const age: number = 20
```

### String

Como em outrass linguagens, usamos o tipo `string` para nos referir a textos. Assim como o JavaScript, o TypeScript também usa aspas duplas (") ou aspas simples (') para envolver os dados da string.

```ts
const text: string = "Hello World!"
```

### Array

O TypeScript, como o JavaScript, permite trabalhar com arrays de valores. Os tipos de array podem ser gravados de duas maneiras. Na primeira, você usa o tipo dos elementos seguido por `[]` para denotar uma array desse tipo de elemento:

```ts
const list: number[] = [1, 2, 3]
```

A segunda maneira usa um tipo genérico de array, `Array<elemType>`:

```ts
const list: Array<number> = [1, 2, 3];
```

### Tuple (Tupla)

Os tipos de tupla permitem expressar um array com um número fixo de elementos cujos tipos são conhecidos, mas não precisam ser os mesmos. Por exemplo, você pode representar um valor como um par de uma `string` e um `number`:

```ts
// Declare o tipo da tupla
let x: [string, number]
// Inicializa
x = ["hello", 10]; // OK
// Inicializa incorretamente
x = [10, "hello"]; // Erro
```

Um caso de uso comum para tuplas em TypeScript é quando você deseja representar um valor composto por diferentes tipos de dados, onde a ordem dos tipos é importante. Por exemplo, você pode usar uma tupla para representar as coordenadas de um ponto em um espaço bidimensional.

Aqui está um exemplo de como usar uma tupla para representar as coordenadas de um ponto:

```ts
// Declaração de uma tupla para coordenadas (x, y)
let point: [number, number] = [3, 7];

// Acessando os elementos da tupla
let xCoordinate: number = point[0];
let yCoordinate: number = point[1];

console.log(`Coordenadas: x = ${xCoordinate}, y = ${yCoordinate}`);
```

Neste exemplo, a tupla `point` contém dois elementos: o primeiro elemento é a coordenada x e o segundo é a coordenada y. O tipo da tupla é especificado como `[number, number]`, indicando que ambos os elementos são números.

As tuplas são úteis quando você precisa manter a estrutura fixa de um conjunto de valores relacionados e quer garantir que os tipos e a ordem dos elementos sejam mantidos corretamente.

### Enum

Uma adição útil ao conjunto padrão de tipos de dados do JavaScript é a enumeração. Como em idiomas como C#, uma enumeração é uma maneira de atribuir nomes mais amigáveis a conjuntos de valores numéricos.

```ts
enum Color {Red, Green, Blue}
let c: Color = Color.Green
```

Por padrão, as enumerações começam a numerar seus membros a partir de `0`. Você pode alterar isso definindo manualmente o valor de um de seus membros. Por exemplo, podemos iniciar o exemplo anterior em `1` vez de `0`:

```ts
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green
```

Ou, mesmo definir manualmente todos os valores na enumeração:

```ts
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green
```

### Any

Podemos precisar descrever o tipo de variáveis ​​que não sabemos qual valor será atribuido quando estamos escrevendo um aplicativo. Esses valores podem vir de conteúdo dinâmico, por exemplo, do usuário ou de uma biblioteca de terceiros. Nesses casos, queremos desativar a verificação de tipo e deixar que os valores passem pelas verificações em tempo de compilação. Para fazer isso, rotulamos estes com o tipo `any`.

```ts
let notSure: any = 2 // OK
notSure = "Hello World!" // OK
notSure = true // OK
notSure = [] // OK
```

### Void

O `void` é usado para determinar que uma função não retorna nenhum valor:

```ts
function warnMessage(message: string): void {
    alert(message)
}
```

### Null e Undefined

No TypeScript, tanto `null` quanto `undefined` têm seus próprios tipos nomeados como `null` e `undefined` respectivamente. Muito parecido com `void`, eles não são extremamente úteis por conta própria:

```ts
let u: undefined = undefined
let n: null = null
```

Nos casos em que você deseja passar uma `string` ou `null` ou `undefined`, você pode usar o tipo união `string | null | undefined`.

### Union (União)

Representa um valor que pode ser de vários tipos. Exemplo: `number | string`

```ts
let name: string | null | undefined = undefined
name = "Jonh Due" // OK
```

Ainda há o tipo `object` para valores não primitivos. Geralmente representados em `class`, `interface` ou `type`

### Tipo genérico

O uso de `<T>` em TypeScript refere-se ao uso de parâmetros de tipo genérico, também conhecidos como ***tipos genéricos***. Esses parâmetros permitem criar componentes e funções que podem trabalhar com diferentes tipos de dados de maneira flexível, sem perder a precisão do controle de tipos. O `T` é apenas um nome de variável usado para representar o tipo genérico, mas você pode usar qualquer outro nome que seja significativo para o contexto.

Vamos explorar mais sobre como e por que usar <T> em TypeScript:

1. **Uso Básico de Parâmetros Genéricos**

```ts
function identity<T>(arg: T): T {
    return arg;
}

let output = identity<string>("Hello, TypeScript!");  // Tipo inferido: string
```

Neste exemplo, a função `identity` recebe um argumento `arg` do tipo `T` e retorna o mesmo valor. O tipo `T` é um parâmetro genérico que permite que a função seja usada com diferentes tipos de entrada.

2. **Arrays Genéricos**

```ts
function reverse<T>(array: T[]): T[] {
    return array.reverse();
}

let numbers = [1, 2, 3, 4, 5];
let reversedNumbers = reverse(numbers);  // Tipo inferido: number[]

let strings = ["apple", "banana", "cherry"];
let reversedStrings = reverse(strings);  // Tipo inferido: string[]
```
Neste caso, a função `reverse` trabalha com arrays de qualquer tipo, pois o parâmetro genérico `T` é aplicado ao tipo de array passado como argumento.

3. **Interfaces Genéricas**

```ts
interface Box<T> {
    value: T;
}

let boxOfNumbers: Box<number> = { value: 42 };
let boxOfStrings: Box<string> = { value: "Hello" };
```
A interface `Box` é genérica, o que significa que ela pode ser usada para criar instâncias que armazenam diferentes tipos de valores.

4. **Funções Genéricas com Restrições**

```ts
function longest<T extends { length: number }>(a: T, b: T): T {
    return a.length >= b.length ? a : b;
}

let result = longest("apple", "banana");  // Tipo inferido: string
```
Neste exemplo, a função `longest` aceita dois argumentos genéricos `a` e `b`, desde que tenham uma propriedade `length`. A restrição `extends { length: number }` garante que os tipos `T` devem ser objetos que possuam a propriedade `length`.

5. **Classe Genérica**

```js
class Stack<T> {
    private items: T[] = [];

    push(item: T) {
        this.items.push(item);
    }

    pop(): T | undefined {
        return this.items.pop();
    }
}

let numberStack = new Stack<number>();
numberStack.push(1);
numberStack.push(2);

let poppedNumber = numberStack.pop();  // Tipo inferido: number
```

Nesse exemplo, a classe `Stack` é genérica e pode ser usada para criar pilhas de diferentes tipos de elementos.

#### Benefícios do Uso de Genéricos

O uso de parâmetros de tipo genérico fornece uma maneira poderosa de escrever código reutilizável e flexível, ao mesmo tempo em que mantém a segurança de tipos durante a execução.

Ao usar `<T>` em TypeScript, você está criando um componente ou função que pode trabalhar com uma ampla gama de tipos de dados, tornando seu código mais genérico e versátil.

### Conversão explicita de tipos com `as`

Em TypeScript, o operador `as` é usado para realizar uma ***conversão de tipo explícita***, também conhecida como _"type assertion"_. Ele permite que você indique ao compilador que você tem conhecimento sobre o tipo de uma variável ou expressão melhor do que o TypeScript pode inferir automaticamente. No entanto, o uso do `as` deve ser feito com cuidado, pois se você especificar um tipo que não é compatível com o valor real, isso pode levar a erros em tempo de execução.

Aqui estão algumas situações em que você pode querer usar o operador `as` em TypeScript:

1. **Quando você sabe mais sobre o tipo do que o compilador**: Às vezes, o TypeScript pode não conseguir inferir o tipo de uma variável de maneira adequada. Usar as nesses casos permite que você informe ao TypeScript qual tipo você acredita que a variável deve ter.

```ts
const valor: any = "123";
const numero: number = valor as number;
```

2. **Trabalhando com tipos mais abstratos**: Em alguns casos, você pode estar trabalhando com tipos mais abstratos, como any ou unknown, e deseja convertê-los em tipos mais específicos.

```ts
const valor: any = "123";
const numero: number = valor as number;
```

3. **Quando você está interoperando com JavaScript não tipado**: Se você estiver integrando código JavaScript não tipado em seu projeto TypeScript, pode usar as para dizer ao TypeScript como tratar os tipos.

```ts
// Exemplo de uma biblioteca JavaScript não tipada
const elemento = document.getElementById("meu-elemento") as HTMLInputElement;
```

É importante notar que o `as` não realiza nenhuma verificação em tempo de execução. Portanto, se você usar `as` para converter um valor em um tipo incompatível, pode ocorrer um erro em tempo de execução. É por isso que é recomendável usar *type assertion* apenas quando você tem **certeza** de que a conversão é **segura**.

No entanto, sempre que possível, é melhor evitar o uso do `as` e permitir que o TypeScript infira tipos de forma natural, pois isso ajuda a tirar o máximo proveito das verificações de tipo estático oferecidas pela linguagem. Use *type assertion* apenas quando você tem certeza do que está fazendo e precisa lidar com situações que o TypeScript não pode inferir automaticamente.

## Interface, Types e Classes

### Interfaces

Uma interface é, em essência, um tipo literal de objeto nomeada, ou seja, define a estrutura de um objeto.
Exemplo:

```ts
interface IPerson {
  name: string
  age: number | string
}
```

Com esta interface podemos falar que um objeto do tipo `IPerson` pode ser de duas maneiras:

```ts
const person: IPerson = {
  name: 'John',
  age: 20,
}

const person2: IPerson = {
  name: 'Joe',
  age: '21',
}
```

Podemos extender interfaces/classes para reaproveitar campos já exitentes, exemplo:

```ts
interface IStudent extends IPerson {
  school: string
}

const student: IStudent = {
  name: 'John',
  age: 20,
  school: 'Fatec',
}
```

Podemos também definir itens não obrigatórios com `?`, exemplo:

```ts
interface IPerson {
  name: string
  age: number
  city?: string
}

const person: IPerson = {
  name: 'John',
  age: 20,
}

const person2: IPerson = {
  name: 'Joe',
  age: 21,
  city: 'São Paulo',
}
```

### Types

Um tipo, definido com a palavra-chave `type`, é uma forma de criar um `alias` para tipos que podem ser usados ​​para representar não apenas tipos primitivos, mas também tipos de objetos, tipos de união, tuplas e interseções. Types são usados para criar tipos complexos, uniões de tipos, interseções de tipos e outras construções avançadas. Eles são mais flexíveis do que as interfaces. 

```ts
type Point = {
    x: number;
    y: number;
};

type ID = string | number;

const point: Point = { x: 5, y: 10 };
const userId: ID = "abc123";
```

### `Type` versus `Interface`

Se assim como eu, você quando conheceu o typescript ficou se perguntando: Qual a diferença entre "Type" e "Interface"? Quando usar um ou outro? Eles são bem parecidos e para a maioria dos casos funcionam da mesma forma.

```ts
type TipoPassaro = {
  asas: number;
};

interface InterfacePassaro {
  asas: 2;
}

const passaro1: TipoPassaro = { asas: 2 };
const passaro2: InterfacePassaro = { asas: 2 };
```

Porque o Typescript é um sistema de tipagem estrutural, é possível misturar o seu uso também.

```ts
const passaro3: InterfacePassaro = passaro1;
```

Ambos suportam a extensão de outras `interface`s e `type`s. Os types fazem isso através da interseção de tipos, enquanto interfaces possuem a palavra-chave `extends`.

```ts
type Coruja = { noturno: true } & TipoPassaro;
type Robin = { noturno: false } & InterfacePassaro;

interface Pavao extends TipoPassaro {
  colorido: true;
  voa: false;
}
interface Galinha extends InterfacePassaro {
  colorido: false;
  voa: false;
}

let coruja: Coruja = { asas: 2, noturno: true };
let galinha: Galinha = { asas: 2, colorido: false, voa: false };
```

Tendo dito isso, recomenda-se a usar interfaces ao invés de tipos. Especialmente porque você recebe melhores mensagens de erro. Se passar o mouse sobre o erro, você pode ver os erros que o Typescript mais detalhados para `interfaces` como a Galinha.

```ts
coruja = galinha; // aribuindo interface a um type
/* Error: Type 'Galinha' is not assignable to type 'Coruja'.
  Property 'noturno' is missing in type 'Galinha' but required in type '{ noturno: true; }'. (2322)
input.tsx(27, 17): 'noturno' is declared here.
let coruja: Coruja */
 
galinha = coruja; // atribuindo um type a uma interface
/* Error: Type 'Coruja' is missing the following properties from type 'Galinha': colorido, voa (2739)*/
```

Uma das maiores diferenças entre tipos e interfaces é que interfaces são abertas e tipos são fechados. Isso signifca que você pode extender interfaces declarando uma segunda vez.

```ts
interface Gato {
  ronrona: boolean;
}

interface Gato {
  cor: string;
}
```

Por outro lado tipos não podem ser alterados fora da própria declaração.

```ts
type Filhote = {
  cor: string;
};

type Filhote = { // Error: Duplicate identifier 'Filhote'
  brinquedos: number;
};
```

Dependendo dos seus objetivos essa diferença pode ser positiva ou negativa. No entando, para expor os tipos publicamente é melhor transformá-los em interfaces.

Como um dos melhores recursos para ver todos os casos de uso de `type` vs `interface`, recomendamos essa thread do Stackoverflow
como um bom ponto de partida: <https://stackoverflow.com/questions/37233735/typescript-interfaces-vs-types/52682220#52682220>

Outros pontos para observarmos são:

- Antes do `TypeScript` versão `4.2`, nomes de alias type podem aparecer em mensagens de erro, às vezes no lugar do tipo anônimo equivalente (que pode ou não ser desejável). 
- As interfaces sempre serão nomeadas nas mensagens de erro.
- Os aliases type não podem participar da mesclagem de declarações, mas as interfaces podem como vimos no exemplo acima. 
- As interfaces só podem ser usadas para declarar as formas dos objetos, mas não para renomear tipos primitivos. 
- Os nomes de interface sempre aparecerão em sua forma original nas mensagens de erro, mas apenas quando forem usados ​​por nome.

### Classes

Classes em JavaScript foram introduzidas no **ECMAScript 2015** e são simplificações da linguagem para as heranças baseadas nos protótipos. A sintaxe para classes não introduz um novo modelo de herança de orientação a objetos em JavaScript. Classes em JavaScript provêm uma maneira mais simples e clara de criar objetos e lidar com herança.

O sistema de classes no TypeScript usa um modelo de herança única que deve ser familiar a qualquer programador que tenha trabalhado com qualquer linguagem baseada em classes.

Exemplo:

```ts
class Person {
  public name: string
  public age: number

  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }

  public showMe() {
    console.log(`Hello, I am ${this.getName()}, and I am ${this.age} years old`)
  }

  private getName(): string {
    return this.name
  }
}

const person = new Person('John', 20)
const user: Person = person
user.showMe() // result => Hello, I am John and I am 20 years old
```

Classes podem também definir as propriedades como sendo `public`, `private`, `protected` e/ou `static`:

- **public:** Permite acessar a propriedade livremente.
- **private:** Quando uma propriedade é marcada como `private`, ela não pode ser acessada de fora da classe que o contém. 
- **static:** São visíveis na própria classe e não nas instâncias.

Em JavaScript, não existem modificadores de acesso como `public` e `private` como em algumas outras linguagens de programação orientada a objetos, como Java ou C++. No entanto, você pode simular esses conceitos usando convenções e recursos da linguagem. Vamos discutir como você pode alcançar isso em JavaScript:

1. **Public**: Em JavaScript, todas as propriedades e métodos de uma classe são, por padrão, públicas, o que significa que podem ser acessadas e modificadas de fora da classe. Por exemplo:

```ts
class MinhaClasse {
  constructor() {
    this.propriedadePublica = 10;
  }

  metodoPublico() {
    return 'Este é um método público';
  }
}

const instancia = new MinhaClasse();
console.log(instancia.propriedadePublica); // Acesso a uma propriedade pública
console.log(instancia.metodoPublico()); // Acesso a um método público
```

2. **Private**: Embora JavaScript não tenha suporte nativo para membros de classe privados, você pode simular privacidade usando convenções de nomenclatura e, a partir do **ECMAScript 2022 (ES12)**, por meio da utilização do prefixo `#`. Por exemplo:

```ts
class MinhaClasse {
  constructor() {
    this.propriedadePublica = 10;
    this.#propriedadePrivada = 20; // Membro privado com #
  }

  #metodoPrivado() { // Método privado com #
    return 'Este é um método privado';
  }

  acessoPropriedadePrivada() {
    return this.#propriedadePrivada;
  }

  acessoMetodoPrivado() {
    return this.#metodoPrivado();
  }
}

const instancia = new MinhaClasse();
console.log(instancia.propriedadePublica); // Acesso a uma propriedade pública
console.log(instancia.acessoPropriedadePrivada()); // Acesso a um membro privado
console.log(instancia.acessoMetodoPrivado()); // Acesso a um membro privado
```

3. **Static**: Quando você declara um membro de classe como static, ele pertence à própria classe em vez de instâncias individuais dessa classe. Isso significa que você pode acessar membros estáticos sem criar uma instância da classe. Aqui está um exemplo:

```ts
class MinhaClasse {
  constructor() {
    this.propriedade = 10;
  }

  static metodoEstatico() {
    return 'Este é um método estático';
  }
}

const instancia1 = new MinhaClasse();
console.log(instancia1.propriedade); // Acesso a uma propriedade de instância
console.log(MinhaClasse.metodoEstatico()); // Acesso a um método estático sem criar uma instância
```

### Quando Usar Cada Um

- `interface`: Use interfaces quando você deseja definir contratos para objetos e garantir uma estrutura consistente. Elas são ótimas para definir como diferentes partes do código se comunicam.

- `type`: Use tipos quando você precisa criar tipos complexos, uniões, interseções ou tipos que não sejam apenas objetos. Eles são úteis para criar tipos reutilizáveis e dar nomes a tipos primitivos ou personalizados.

- `class`: Use classes quando você precisa criar objetos com comportamento e estado. Elas são ideais para modelar entidades com métodos e propriedades.


## Uso do `Omit<>`

`Omit<>` é um utilitário de tipo genérico fornecido pelo `TypeScript` que permite criar um novo tipo excluindo determinadas propriedades de um tipo existente. Isso é especialmente útil quando você deseja criar um novo tipo baseado em um tipo existente, mas precisa omitir uma ou mais propriedades específicas.

A sintaxe geral do Omit<> é a seguinte:

```ts
type NewType = Omit<OriginalType, 'prop1' | 'prop2'>;
```

- `OriginalType`: O tipo do qual você deseja omitir as propriedades.
- `'prop1' | 'prop2'`: Uma lista de propriedades que você deseja omitir do tipo original.
`NewType`: O novo tipo resultante após a omissão das propriedades especificadas.

Vamos ver um exemplo para entender melhor como funciona:

```ts
type Person = {
    name: string;
    age: number;
    address: string;
};

// Vamos omitir a propriedade 'address' do tipo 'Person'
type PersonWithoutAddress = Omit<Person, 'address'>;

const person: PersonWithoutAddress = {
    name: "Alice",
    age: 30
};
```

Neste exemplo acima, criamos um novo tipo chamado `PersonWithoutAddress`, que é derivado do tipo `Person` excluindo a propriedade `address`. Isso nos permite criar objetos do tipo `PersonWithoutAddress` sem precisar incluir a propriedade `address`.

O `Omit<>` é particularmente útil quando você está lidando com tipos complexos e deseja criar variantes desses tipos com algumas propriedades omitidas. Ele ajuda a evitar a duplicação de código e a manter a consistência dos tipos.

Lembre-se de que o `Omit<>` é um dos muitos utilitários de tipos fornecidos pelo `TypeScript` para manipular e transformar tipos de maneira eficiente e segura.

## Compilando e Executando

Crie um arquivo com extensão `.ts`. Por exemplo, `app.ts`.

No terminal, execute o seguinte comando para compilar o arquivo TypeScript:

```bash
tsc app.ts
```

Isso irá gerar um arquivo app.js. Agora você pode executar o código JavaScript resultante usando o Node.js:

```bash
node app.js
```

---

## Docs

- <https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Classes>
- <https://www.typescriptlang.org/docs/handbook/classes.html>
- <https://www.typescriptlang.org/docs/handbook/interfaces.html>
- <https://lit.dev/docs/>
- <https://lit.dev/tutorials/intro-to-lit/>
- <https://levelup.gitconnected.com/learning-typescript-with-web-components-lit-4f38fae47e27>
- <https://gist.github.com/aelbore/d80c98bde558987c045e4798b570afdf>
- <https://medium.com/@mariusbongarts/build-your-own-blog-portfolio-with-web-components-typescript-adfbcd917d96>
- <https://www.thisdot.co/blog/web-components-integration-using-litelement-and-typescript>
- <https://gilfink.medium.com/creating-a-custom-element-decorator-using-typescript-302e7ed3a3d1>
- <https://www.freecodecamp.org/news/a-practical-guide-to-typescript-how-to-build-a-pokedex-app-using-html-css-and-typescript/>

---

## Desafio

Agora faça o desafio da aula anterior usando Typescript. Fique a vontade para pesquisar outras bibliotecas ou maneiras de desenvolver. O importante é se esforçar ao máximo para aplicar todos os conceito aprendidos atá agora.
