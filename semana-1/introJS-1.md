# Uma Reintrodução ao Javascript

## Introdução

Por que uma reintrodução? Porque [JavaScript](/pt-BR/JavaScript) é conhecida como [a mais incompreendida linguagem de programação do mundo](http://javascript.crockford.com/javascript.html). Embora muitas vezes ridicularizada como um brinquedo, por baixo de sua simplicidade enganosa estão alguns recursos poderosos da linguagem, que agora é usado por um número incrível de aplicações de alto nível, mostrando que o conhecimento mais profundo desta tecnologia é uma habilidade importante para qualquer desenvolvedor web, mobile ou desktop.

É sempre bom começar com a história da linguagem. A JavaScript foi criada em 1995 por ***Brendan Eich***, um engenheiro da **Netscape**, e lançada pela primeira vez com o Netscape 2 no início de 1996. Foi inicialmente chamada de **LiveScript**, mas logo foi rebatizada, em uma decisão de marketing ~~maliciosa~~ *"malfeita"*, para tentar crescer sobre a popularidade da linguagem Java da Sun Microsystem - apesar das duas terem muito pouco em comum. Esta tem sido uma fonte de confusão desde então. 

É uma das linguagens de programação mais populares do mundo. Praticamente todos os computadores pessoais do mundo possuem pelo menos um interpretador JavaScript instalado e em uso ativo. A popularidade do JavaScript se deve inteiramente ao seu papel como linguagem de script da WWW.

Apesar de sua popularidade, poucos sabem que JavaScript é uma linguagem de programação de uso geral orientada a objetos dinâmica muito agradável. Como isso pode ser um segredo? Por que essa linguagem é tão mal compreendida?

### Lisp na roupa de C

A sintaxe semelhante a **C** do JavaScript, incluindo chaves e a desajeitada instrução `for`, faz com que pareça uma linguagem processual comum. Isso é enganoso porque JavaScript tem mais em comum com linguagens funcionais como **Lisp** ou **Scheme** do que com **C** ou **Java**. Possui arrays em vez de listas e objetos em vez de listas de propriedades.

Para entender melhor a diferença fundamental entre linguagem funcional e procedural, podemos dizer que reside em suas abordagens para resolução de problemas. As linguagens procedurais, como **C** e **Pascal**, organizam o código em procedimentos e funções que manipulam variáveis compartilhadas, seguindo uma abordagem passo a passo. Já as linguagens funcionais, como **Haskell** e **Lisp**, enfatizam a avaliação de expressões e o uso de funções puras, evitando estados compartilhados e mutabilidade. Isso permite um código mais conciso, legível e seguro, uma vez que as operações são tratadas como transformações de dados imutáveis, resultando em maior paralelismo e modularidade.

**Leia mais sobre em:** 
- <https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Language_overview>
- <https://terminalroot.com.br/2019/10/diferencas-entre-programacao-procedural-funcional-orientada-a-objetos-e-eventos.html>
- <https://guia.dev/pt/pillars/languages-and-tools/programming-paradigms.html>

## Visão Geral

### Estrutura de Dados

JavaScript é uma linguagem de tipagem dinâmica. Isso significa que você não necessita declarar o tipo de uma variável antes de sua atribuição. O Javascript usa em geral os tipos de dados primitivos e alguns deles são: 

- *Boolean*
- *String*
- *Number*
- *Object*

Temos outros tipos de dados utilizados, tais como `Null`, `Undefined`, `Symbol` e etc, onde saberá com mais detalhes em: https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Data_structures

Para complementar seus estudos: 

 - [Sobre tipos primitivos](https://developer.mozilla.org/pt-BR/docs/Glossary/Primitive)
 - [Sintaxe e tipos](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Grammar_and_types)

### Trabalhando com Números

Números em JavaScript são "valores de precisão dupla no formato IEEE 754", de acordo com a especificação. Isto tem algumas consequências interessantes. Não existe essa coisa de inteiro em JavaScript, então você deve ser um pouco cuidadoso com seus cálculos se você está acostumado com a matemática em C ou Java. Cuidado com coisas como:

```
0.1 + 0.2 == 0.30000000000000004
```

Na prática, valores inteiros são tratados como inteiros de 32 bits (e são armazenados dessa forma em algumas implementações do navegador), que podem ser importantes para as operações bit a bit.

Você pode converter uma string em um inteiro usando a função embutida [`parseInt()`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/parseInt). Ela tem um segundo parâmetro opcional para a base da conversão, parâmetro esse que você deveria sempre prover. Veja:

```js
parseInt("123") // 123)
```

Até ai tudo normal, certo? Mas se caso a minha string venha com `0` na frente?

```js
let octalNumber = "010"; // String que parece octal
let number = parseInt(octalNumber);
console.log(number); // Saída: 10 (interpretação incorreta como decimal)
```

Neste exemplo agora, a string `"010"` se parece com um número octal devido ao zero à esquerda. Sem especificar a base no segundo parâmetro, `parseInt` interpretará a string como base 10, resultando em um número 10. Isso pode não ser o resultado esperado, já que a intenção pode ter sido interpretar a string como octal (base 8).

E isso acontece por que? Se base é `undefined` ou `0` (ou ausente), JavaScript assume o seguinte:

- Se a string de entrada começa com "0x" ou "0X", a base é 16 (hexadecimal) e o restante da string é analisado.
- Se a string de entrada começa com "0", a base é oito (octal) ou 10 (decimal). Exatamente qual base é escolhida é dependente da implementação. **O ECMAScript 5 especifica que 10 (decimal) seja utilizado, mas nem todos os browsers suportam isso ainda.** Por essa razão sempre especifique uma base quando estiver usando parseInt.
- Se a string de entrada começa com qualquer outro valor, a base é 10 (decimal).

Se você quiser converter um número binário em um inteiro, basta mudar a base:

```js
parseInt("11", 2) // 3
```

Similarmente, você pode fazer a conversão de números de ponto flutuante usando a função embutida [`parseFloat()`](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/parseFloat) **que usa a base 10 sempre**, ao contrário de seu primo `parseInt()`.

Você também pode usar o operador unário `+` para converter valores em números:

```js
+ "42" // 42
```

Um valor especial chamado [`NaN`](/pt-BR/JavaScript/Reference/Global_Objects/NaN) (sigla de *"Not a Number"* ou *"Não é um Número"*) é retornado se a string não é um valor numérico:

```js
parseInt("hello", 10) // NaN
```

`NaN` é tóxico: Se você provê-lo como uma entrada para qualquer operação matemática o resultado também será `NaN`:

```js
NaN + 5 // NaN
```

Você pode testar se é `NaN` usando a função embutida [`isNaN()`](/pt-BR/JavaScript/Reference/Global_Objects/isNaN):

```js
isNaN(NaN + 5) // true
```

> **Nota:** As funções `parseInt()` e `parseFloat()` fazem a conversão da string até alcançarem um caracter que não é válido para o formato numérico especificado, então elas retornam o número convertido até encontrar o ponto. Contudo, o operador "+" simplesmente converte a string em `NaN` se tiver algum caracter inválido nela. Apenas tente por si mesmo converter a string "10.2abc" usando cada um desses métodos no console e entenderá melhor essas diferenças.
> ```js
> parseInt('10.2abc') // 10
> parseFloat('10.2abc') // 10.2
> const convert = + "10.2abc" // NaN
> ```
> ___

### Trabalhando com Strings

Strings em JavaScript são sequências de caracteres. Para ser mais exato, elas são sequências de [Unicode characters](/pt-BR/JavaScript/Guide/Obsolete_Pages/Unicode), em que cada um deles é representado por um número de 16-bits. Isso deveria ser uma notícia bem-vinda para aqueles que tiveram que lidar com internacionalização.

Se você quiser representar um único caractere, você só tem que usar uma string de tamanho 1.

Para obter o tamanho de uma string, acesse sua propriedade [`length`](/pt-BR/JavaScript/Reference/Global_Objects/String/length):

```js
"hello".length // 5
```

Essa é nossa primeira pincelada com objetos JavaScript! Eu mencionei que strings também são objetos? De modo que elas têm métodos:

```js
"hello".charAt(0) // h
"hello, world".replace("hello", "goodbye") // goodbye, world
"hello".toUpperCase() // HELLO
```

E por falar nisso, tem esse vídedo muito bacana que explica como no Javascript é tudo objeto: <https://www.youtube.com/watch?v=n5uiJr-v0KQ>

### Trabalhando com outros tipos

No JavaScript há distinção entre `null`, que é um objeto do tipo 'object' para indicar deliberadamente uma ausência de valor, do `undefined`, que é um objeto do tipo 'undefined' para indicar um valor não inicializado — isto é, que um valor não foi atribuído ainda. Vamos falar sobre variáveis depois, mas em JavaScript é possível declarar uma variável sem atribuir um valor para a mesma. Se você fizer isso, a variável será do tipo `undefined`.

JavaScript tem um tipo boolean, ao qual são possíveis os valores `true` e `false` (ambos são palavras-chave). Qualquer valor pode ser convertido em um boolean, de acordo com as seguintes regras:

1. `false`, `0`, a string vazia(`""`), `NaN`, `null`, e `undefined` todos tornam-se `false`
2. todos os outros valores tornam-se `true`

Você pode fazer essa conversão explicitamente usando a função `Boolean()`:

```js
Boolean("") // false
Boolean(234) // true
```

Contudo, essa é uma necessidade rara, uma vez que JavaScript silenciosamente fará essa conversão quando for esperado um boolean, como em uma instrução `if`. Por isso, algumas vezes falamos simplesmente em "valores true" e "valores false" nos referindo a valores que se tornaram `true` e `false`, respectivamente, quando convertidos em boolean. Alternativamente, esses valores podem ser chamados de *"truthy"* (verdade/verdadeiro) e *"falsy"* (falso/falsidade), respectivamente.

Operações booleanas como `&&` (_and_ lógico), `||` (_or_ lógico), e `!` (_not_ lógico) são suportadas.

### Variaveis

Existem três tipos de declarações em JavaScript.

- **var**: Declara uma variável, opcionalmente, inicializando-a com um valor. Esta tem um escopo global e pode ser redeclarada. 👎
- **let**: Declara uma variável local de escopo do bloco, opcionalmente, inicializando-a com um valor. 👍
- **const**: Declara uma constante de escopo de bloco, apenas para leitura. 👍

> **Escopo**: Quando você declara uma váriavel fora de qualquer função, ela é chamada de variável global, porque está disponível para qualquer outro código no documento atual. Quando você declara uma variável dentro de uma função, é chamada de variável local,  pois ela está disponível somente dentro dessa função.

### Constantes
Você pode criar uma constante apenas de leitura por meio da palavra-chave `const`, ou seja, não pode ser reatribuída. A sintaxe de uma declaração de uma constante é semelhante ao identificador de uma variável: deve começar com uma letra, sublinhado ou cifrão e pode conter caractere alfabético, numérico ou sublinhado.

``` javascript
const PI = 3.14;
```

Veja mais detalhes: https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Values,_variables,_and_literals

## Escopo
Quando você declara uma váriavel fora de qualquer função, ela é chamada de variável global, porque está disponível para qualquer outro código no documento atual. Quando você declara uma variável dentro de uma função, é chamada de variável local,  pois ela está disponível somente dentro dessa função.

Uma diferença importante de outras linguagens como Java é que em JavaScript, blocos não tem escopo; somente funções tem escopo. De modo que se uma variável é definida usando `var` numa instrução composta (por exemplo dentro de uma estrutura de controle `if`), ela será visível por toda a função.

Obs: A definição de variáveis usando o `let` foi introduzida no **ECMAScript 6 (ES6)**. O `let` permite escopo de bloco, ou seja, é possível definir uma variável em um bloco `if`, e esta variável ter escopo limitado ao bloco `if`- por exemplo.

## Operadores

Operadores numéricos de JavaScript são `+`, `-`, `*`, `/` e `%` - que é o operador resto. Valores são atribuídos usando `=`, e temos também as instruções de atribuição compostas, como `+=` e `-=`. Essas são o mesmo que `x = x operador y`.

```js
x += 5;
x = x + 5;
```

Se você adicionar uma string a uma número (ou outro valor) tudo será convertido em uma string primeiro. Isso talvez seja uma pegadinha para você:

```js
"3" + 4 + 5 // 345
3 + 4 + "5" // 75
```

> ***Adicionar uma string em branco a algo é uma maneira melhor de fazer a conversão.***

O JavaScript possui os tipos de operadores a seguir.

> Veja mais sobre cada um abaixo, nosso conselho é que apenas *CONHEÇA* e tente pelo menos utiliza-los de alguma forma para que se lembre no futuro.

- [Operadores de atribuição](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Expressions_and_Operators#operador_atribuicao)
- [Operadores de comparação](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Expressions_and_Operators#operador_comparacao)
- [Operadores aritméticos](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Expressions_and_Operators#operadores_aritmeticos)
- [Operadores relacionais](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Expressions_and_Operators#operador_virgula)
- [Operadores bit a bit](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Expressions_and_Operators#operadores_bit_a_bit)
- [Operadores lógicos](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Expressions_and_Operators#operadores_logicos)
- [Operadores de string](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Expressions_and_Operators#operadores_string)
- [Operador condicional (ternário)](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Expressions_and_Operators#operador_condicional_ternario)
- [Operador vírgula](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Expressions_and_Operators#operador_virgula)
- [Operadores unário](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Expressions_and_Operators#operadores_unario)

**Para saber mais:**
- https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Expressions_and_operators
- https://tableless.github.io/iniciantes/manual/js/controles-de-fluxo-e-controles-de-repeticao.html


## Funções/Métodos

Funções são blocos de construção fundamentais em JavaScript. Uma função é um procedimento de JavaScript - um conjunto de instruções que executa uma tarefa ou calcula um valor. Para usar uma função, você deve defini-la em algum lugar no escopo do qual você quiser chamá-la.

``` javascript
function square(numero) { 
  return numero * numero; 
}
```

- https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Fun%C3%A7%C3%B5es
- https://medium.com/reactbrasil/como-o-javascript-funciona-entendendo-as-fun%C3%A7%C3%B5es-e-suas-formas-de-uso-eb387c7fa138

## Estruturas de Controle

JavaScript tem um conjunto de estruturas de controle similar à outras linguagens na família do C. Instruções condicionais são suportadas pelo `if` e `else`; você pode encadeá-los se quiser:

```js
var name = "kittens";

if (name == "puppies") {
  name += "!";
} else if (name == "kittens") {
  name += "!!";
} else {
  name = "!" + name;
}
name == "kittens!!";
```

Os operadores `&&` e `||` usam a lógica de curto-circuito, o que quer dizer que, o segundo operando ser executado dependerá do primeiro operando. Isso é útil para checagem de objetos null antes de acessar seus atributos:

```js
const name = user && user.getName();
```

Ou para configuração de valores-padrões:

```js
const name = otherName || "default";
```

> É importante entender como esses operadores funcionam e como se comportam. Por exemplo: O operador `||` retorna o primeiro valor e o `&&` retorna o ultimo

JavaScript tem um operador ternário para expressões condicionais:

```js
let allowed = age > 18 ? "yes" : "no";
```

A instrução switch pode ser usada para múltiplas ramificações baseadas em um número ou uma string:

```js
switch (action) {
  case "draw":
    drawit();
    break;
  case "eat":
    eatit();
    break;
  default:
    donothing();
}
```

Se você não adicionar a instrução `break`, a execução irá "cair" no próximo nível. Isso é algo que raramente vai querer fazer — de fato vale mais a pena colocar um comentário especificando essa "queda" para o próximo nível, pois isso o ajudará na hora de fazer a depuração de seu código:

```js
switch (a) {
  case 1: // queda
  case 2:
    eatit();
    break;
  default:
    donothing();
}
```

A cláusula default é opcional. Se quiser, pode colocar expressões tanto no switch como nos cases; Comparações acontecem entre os dois usando o operador `===`:

```js
switch (1 + 3) {
  case 2 + 2:
    yay();
    break;
  default:
    neverhappens();
}
```

Agora suponhamos que temos uma função que recebe um dia da semana como entrada e retorna uma mensagem correspondente. Aqui está como poderíamos fazer isso usando um **objeto literal** em vez de **switch case**:

### Usando switch case:

```js
function getMessageUsingSwitch(day) {
    let message;
    switch (day) {
        case 'Monday':
            message = 'It\'s Monday, time to start the week!';
            break;
        case 'Tuesday':
            message = 'Tuesday is here, keep up the momentum!';
            break;
        case 'Wednesday':
            message = 'Wednesday - halfway through!';
            break;
        case 'Thursday':
            message = 'Thursday, almost there!';
            break;
        case 'Friday':
            message = 'It\'s Friday, time to celebrate!';
            break;
        default:
            message = 'Weekend vibes, enjoy!';
            break;
    }
    return message;
}
```

### Usando objeto literal:

```js
function getMessageUsingObjectLiteral(day) {
    const messages = {
        'Monday': 'It\'s Monday, time to start the week!',
        'Tuesday': 'Tuesday is here, keep up the momentum!',
        'Wednesday': 'Wednesday - halfway through!',
        'Thursday': 'Thursday, almost there!',
        'Friday': 'It\'s Friday, time to celebrate!',
    };

    return messages[day] || 'Weekend vibes, enjoy!';
}

```

## Loops e Iterações

Rapidamente, o que é um loop? É um código que vai ser repetindo até que uma determinada condição seja alcançada, ou até mesmo que não haja condição de parada, estes são conhecidos como loops infinitos.

O JavaScript tem as estruturas de repetição com os laços `while` e `do-while`. O primeiro é bom para repetições básicas; o segundo é para os casos em que você queira que o corpo da repetição seja executado pelo menos uma vez:

```js
while (true) {
  // an infinite loop!
}

let input;
do {
  input = get_input();
} while (inputIsNotValid(input));
```

O laço `for` do JavaScript é o mesmo que no C e Java: ele lhe permite prover as informações para o seu laço em uma única linha. Formado por três partes: inicialização, condição e incremento. A sintaxe é:

```js
for (let i = 0; i < 5; i++) {
  // Will execute 5 times
}
```

### FOR IN

É utilizado quando não sabemos ou ter que determinar quantas vezes temos que interar sobre um array ou objeto.

```js
let arr = [1,2,3];

for(let n in arr) {
  console.log(n);
}
```

É possivel interar objetos também:

```js
let obj = {
  name: 'Rodrigo',
  age: 21
};

for(let n in obj) {
  console.log(n);
}
```

### FOR OF

É utilizado quando não sabemos ou ter que determinar quantas vezes temos que interar sobre um array. Não se aplica a objetos como no `for in`, que retorna as `keys` ou `indexes`

```js
let arr = [1,2,3];

for(let n of arr) {
  console.log(n);
}
```

### FOREACH

Utilizamos o foreach quando queremos percorrer as propriedades de um objeto ou os itens de um array, sem precisamos nos preocupar em contar quantos são.

```js
let arr = [1,2,3];
arr.forEach(function(each){
  console.log(each);
});
```

Leia esta matéria interessante sobre performance de loops no javascript: <https://greenonsoftware.com/articles/thoughts/loops-performance-in-javascript/> 

### Outros Manipuladores de Arrays

Vetores vem com vários métodos:

| Nome do método                       | Descrição                                                                                     |
| ------------------------------------ | --------------------------------------------------------------------------------------------- |
| `a.toString()`                       | Retorna uma string com o toString()de cada elemento separado por vírgulas.                    |
| `a.toLocaleString()`                 | Retorna uma string com o toLocaleString()de cada elemento separado por vírgulas.              |
| `a.concat(item[, itemN])`            | Retorna um novo vetor com os itens adicionados nele.                                          |
| `a.join(sep)`                        | Converte um vetor em uma string com os valores do vetor separados pelo valor do parâmetro sep |
| `a.pop()`                            | Remove e retorna o último item.                                                               |
| `a.push(item[, itemN])`              | `Push` adiciona um ou mais itens ao final.                                                    |
| `a.reverse()`                        | Reverte o vetor.                                                                              |
| `a.shift()`                          | Remove e retorna o primeiro item.                                                             |
| `a.slice(start, end)`                | Retorna um sub-vetor.                                                                         |
| `a.sort([cmpfn])`                    | Prover uma função opcional para fazer a comparação.                                           |
| `a.splice(start, delcount[, itemN])` | Permite que você modifique um vetor por apagar uma seção e substituí-lo com mais itens.       |
| `a.unshift([item])`                  | Acrescenta itens ao começo do vetor.                                                          |

## Objetos

Objetos JavaScript são simplesmente coleções de pares nome-valor. Como tal, eles são similares à:

- *Dicionários* em `Python`
- *Hashes* em `Perl` e `Ruby`
- *Hash tables* em `C` e `C++`
- *HashMaps* em `Java`
- *Vetores associativos* em `PHP`

Esse tipo de estrutura de dados é largamente utilizada, o que atesta sua versatilidade. ***Uma vez que tudo em JavaScript é um objeto***, qualquer programa JavaScript naturalmente envolve uma grande quantidade de buscas em tabelas hash, o que é algo bom, pois elas são bem rápidas!

A parte `key` é uma string JavaScript, enquanto o `value` pode ser qualquer valor JavaScript — incluindo mais objetos. Isso permite que você construa estruturas de dados de qualquer complexidade.

Há basicamente duas formas de criar um objeto vazio:

```js
var obj = new Object();
```

e:

```js
var obj = {};
```

Elas são semanticamente equivalentes; a segunda forma é chamada de sintaxe de objeto literal e é mais conveniente. Essa sintaxe é também o coração do formato JSON e deveria ser sempre preferida.

Uma vez criada, as propriedades de um objeto podem novamente ser acessadas de uma das seguintes formas:

```js
obj.name = "Simon";
var name = obj.name;
```

E...

```js
obj["name"] = "Simon";
var name = obj["name"];
```

Estas também são semânticamente equivalentes. A segunda forma tem a vantagem de que o valor da chave é passado através de uma string, que pode ser calculada em tempo de execução, muito embora esse método previna o uso de alguns mecanismos tais como a otimização e a minificação. Outra vantagem é a possibilidade de se atribuir [palavras-reservadas](/pt-BR/JavaScript/Reference/Reserved_Words) aos nomes das propriedades:

```js
obj.for = "Simon"; // Erro de sintaxe, pois 'for' é uma palavra reservada
obj["for"] = "Simon"; // Funciona bem
```

A sintaxe de objeto literal pode ser usada para inicializar completamente um objeto:

```js
var obj = {
  name: "Carrot",
  for: "Max",
  details: {
    color: "orange",
    size: 12,
  },
};
```

O acesso aos atributos podem ser encadeados:

```js
> obj.details.color
orange
> obj["details"]["size"]
12
```

## Arrays (Vetores)

Vetores em JavaScript são, de fato, um tipo especial de objeto. Eles funcionam de forma muito similar à objetos regulares (propriedades numéricas podem naturalmente ser acessadas somente usando a sintaxe `[]`, colchetes ), porém eles tem uma propriedade mágica chamada '`length`'. Ela sempre é o maior índice de um vetor mais 1.

A forma tradicional de se criar um vetor em JavaScript é a seguinte:

```js
var a = new Array(); // Também pode ser []
a[0] = "dog";
a[1] = "cat";
a[2] = "hen";
a.length // 3
```

Existe uma notação mais conveniente usando um vetor literal:

```js
var a = ["dog", "cat", "hen"];
a.length // 3
```

> Deixar uma vírgula à direita no final de um vetor literal gerará inconsistência entre os navegadores, portanto não faça isso.

**Entenda que `array.length` não é necessariamente o número de itens em um vetor**. 

Considere o seguinte:

```js
var a = ["dog", "cat", "hen"];
a[100] = "fox";
a.length // 101
```

> Lembre-se — o `length` de um `array` é o maior índice mais 1.

Se você fizer referência a um índice de vetor inexistente, obterá um `undefined`:

```js
typeof a[90]
undefined
```

Você pode iterar sobre um vetor da seguinte forma:

```js
for (let i = 0; i < a.length; i++) {
  // Faça algo com a[i]
}
```

Isso é um pouco ineficaz visto que você está procurando a propriedade length uma vez a cada iteração. Uma melhoria poderia ser:

```js
for (let i = 0, len = a.length; i < len; i++) {
  // Faça algo com a[i]
}
```

Uma forma mais elegante ainda poderia ser:

```js
for (let i = 0, item; (item = a[i++]); ) {
  // Faça algo com item
}

// Aqui nós estamos declarando duas variáveis. A atribuição na parte do meio do laço `for` é também testada — se for verdadeira, o laço continuará. Uma vez que o `i` é incrementado toda vez, os itens do array serão atribuídos a variável item sequencialmente. A iteração é finalizada quando item "falsy" é encontrado (tal como o `undefined`, false ou zero).
```

Note que esse truque só deveria ser usado em vetores que você sabe não conter valores "falsy" (vetores de objeto ou nós [DOM](/pt-BR/DOM) por exemplo). Se você iterar sobre dados numéricos que possam ter o 0 ou sobre dados string que possam ter uma string vazia, você deveria usar a segunda forma como alternativa.

Se quiser adicionar um item a um vetor, simplesmente faça desse jeito:

```js
a[a.length] = item; // é o mesmo que a.push(item) ou
[...a, item]
```

## Funções

Junto com objetos, funções são os componentes principais para o entendimento do JavaScript. A função mais básica não poderia ser mais simples:

```js
function add(x, y) {
  var total = x + y;
  return total;
}
```

Isso demonstra tudo o que há para se saber sobre funções básicas. Uma função JavaScript pode ter 0 ou mais parâmetros declarados. O corpo da função pode conter tantas instruções quantas quiser e pode declarar suas próprias variáveis que são de escopo local àquela função. A instrução `return` pode ser usada para retornar um valor em qualquer parte da função, finalizando a função. Se nenhuma instrução de retorno for usada (ou um retorno vazio sem valor), o JavaScript retorna `undefined`.

Os parâmetros nomeados se parecem mais com orientações do que com outra coisa. Você pode chamar a função sem passar o parâmetro esperado, nesse caso eles receberão o valor `undefined`.

```js
add() // NaN 
// Você não pode executar adição em undefined
```

Você também pode passar mais argumentos do que a função está esperando:

```js
add(2, 3, 4) //5 
// adicionado os dois primeiros; 4 foi ignorado
```

### Variável `arguments`

Pode parecer um pouco bobo, mas no corpo da função você tem acesso a uma variável adicional chamada [`arguments`](/pt-BR/JavaScript/Reference/Functions_and_function_scope/arguments), que é um objeto parecido com um vetor que contém todos os valores passados para a função. Vamos rescrever a função add para tomarmos tantos valores quanto quisermos:

```js
function add() {
    let sum = 0;
    for (let i = 0, j = arguments.length; i < j; i++) {
        sum += arguments[i];
    }
    return sum;
}

add(2, 3, 4, 5) // 14
```

Isso realmente não é muito mais útil do que escrever `2 + 3 + 4 + 5`. Vamos criar uma função para calcular média:

```js
function avg() {
    let sum = 0;
    for (let i = 0, j = arguments.length; i < j; i++) {
        sum += arguments[i];
    }
    return sum / arguments.length;
}
avg(2, 3, 4, 5)
3.5
```

Isso é muito útil. A função `avg()` precisa de uma lista de argumentos separados por vírgula — mas e se o que quiser for procurar a média de um vetor? Você poderia apenas rescrever a função da seguinte forma:

```js
function avgArray(arr) {
    let sum = 0;
    for (let i = 0, j = arr.length; i < j; i++) {
        sum += arr[i];
    }
    return sum / arr.length;
}
avgArray([2, 3, 4, 5]) // 3.5
```

### funções anônimas

JavaScript também lhe permite criar funções anônimas.

```js
const avg = function () {
  let sum = 0;
  for (let i = 0, j = arguments.length; i < j; i++) {
    sum += arguments[i];
  }
  return sum / arguments.length;
};
```

Isso é semanticamente equivalente a forma `function avg()`. É extremamente poderoso como ele lhe permite colocar a definição completa de uma função em qualquer lugar, que você normalmente colocaria numa expressão. 

### Funções recursiva

> JavaScript lhe permite chamar funções recursivamente. Uma função recursiva é uma função que se chama a si mesma durante a sua execução para resolver um problema, dividindo-o em instâncias menores e mais simples do mesmo problema, até atingir um ponto onde a solução é trivial, permitindo então a construção da solução para o problema original.

Para um exemplo simples de uma função recursiva em JavaScript. Vamos criar uma função recursiva para calcular o fatorial de um número. O fatorial de um número inteiro não negativo n (denotado como n!) é o produto de todos os números inteiros positivos de 1 até n. Por exemplo, o fatorial de 5 (representado como 5!) é igual a 5 × 4 × 3 × 2 × 1, resultando em 120.

```js
function fatorial(n) {
  // Caso base: o fatorial de 0 é 1
  if (n === 0) {
    return 1;
  }
  // Caso recursivo: fatorial de n é n multiplicado pelo fatorial de (n-1)
  else {
    return n * fatorial(n - 1);
  }
}

// Testando a função com um valor
const numero = 5;
console.log(`O fatorial de ${numero} é ${fatorial(numero)}`);
```

Neste exemplo, a função fatorial é definida de forma recursiva. Ela verifica se o valor de entrada n é igual a 0 (caso base). Se for, retorna 1. Caso contrário, calcula o fatorial de n multiplicando n pelo fatorial de n - 1, o que é feito chamando a própria função fatorial com o argumento n - 1.

Quando a função é chamada com numero igual a 5, ela funciona da seguinte forma:

```js
fatorial(5)
5 * fatorial(4)
5 * (4 * fatorial(3))
5 * (4 * (3 * fatorial(2)))
5 * (4 * (3 * (2 * fatorial(1))))
5 * (4 * (3 * (2 * (1 * fatorial(0)))))
5 * (4 * (3 * (2 * (1 * 1))))
5 * (4 * (3 * (2 * 1)))
5 * (4 * (3 * 2))
5 * (4 * 6)
5 * 24
120
```

Portanto, o `fatorial` de `5` é calculado corretamente como `120`.

Isso é muito útil quando estamos lidando com estruturas de árvore, como quando estamos navegando no [DOM](/pt-BR/DOM).

```js
function countChars(element) {
  if (element.nodeType == 3) {
    // TEXT_NODE
    return element.nodeValue.length;
  }
  let count = 0;
  for (let i = 0, child; (child = element.childNodes[i]); i++) {
    count += countChars(child);
  }
  return count;
}
```

### Funções autoinvocáveis

Isso destaca um problema potencial com funções anônimas: Como chamá-las recursivamente se elas não tem um nome? O JavaScript lhe permite nomear expressões de função para isso. Você pode usar EFIIs nomeadas (Expressões Funcionais Imediatamente Invocadas), conforme abaixo:

```js
(function counter(element) {
  if (element.nodeType == 3) {
    // TEXT_NODE
    return element.nodeValue.length;
  }
  let count = 0;
  for (let i = 0, child; (child = element.childNodes[i]); i++) {
    count += counter(child);
  }
  return count;
})(document.body);
```

O nome provido para a função anônima conforme acima só é (ou no mínimo só deveria ser) visível ao escopo da própria função. Isso tanto permite que mais otimizações sejam feitas pela engine como deixa o código mais legível.
