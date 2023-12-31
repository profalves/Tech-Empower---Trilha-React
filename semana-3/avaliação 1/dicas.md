# Avaliação I - Dicas

Este é um tutorial passo a passo para ajudá-lo a construir o código completo da calculadora em TypeScript. Certifique-se de seguir cada passo com atenção.

## Passo 1: Estrutura HTML
Primeiramente precisa observar o HTML do projeto e certifique-se de que seu arquivo contenha a estrutura básica da calculadora, conforme já fornecido. Certifique-se de ter os elementos corretos com IDs e classes para interação com a mesma. 

## Passo 2: Configuração do TypeScript
Certifique-se de ter o TypeScript instalado. Caso não tenha, você pode instalá-lo usando o seguinte comando:

```bash
yarn init -y # inicializa um node package no seu projeto
yarn add typescript -D # instala o typescript como dependencia de desenvolvimento
```

## Passo 3: Crie um arquivo TypeScript

Crie um novo arquivo TypeScript chamado `app.ts` na pasta `js/`. Este arquivo conterá todo o código TypeScript da calculadora.

```bash
js/app.ts
```

## Passo 4: Escreva o código TypeScript
Aqui está o código TypeScript com uma estrutura inicial:

```ts
// Variáveis globais
let displayValue: string = "0";
let previousValue: string;
let operator: string | null = null;
let waitingForSecondOperand: boolean = false;
let decimalEntered: boolean = false;

// Elementos DOM
const display = document.getElementById("display") as HTMLSpanElement;
const buttons = document.querySelectorAll(".tecla");

// Função para atualizar o display
function updateDisplay() {
  display.textContent = displayValue;
}

// Função para adicionar um dígito ao display
function inputDigit(digit: string) {
  // Implementação da função inputDigit aqui
}

// Função para adicionar um ponto decimal
function inputDecimal() {
  // Implementação da função inputDecimal aqui
}

// Função para executar operações
function performOperation(nextOperator: string) {
  // Implementação da função performOperation aqui
}

// Função para limpar o display
function clearDisplay() {
  // Implementação da função clearDisplay aqui
}

// Adiciona os event listeners aos botões
buttons.forEach((button) => {
  button.addEventListener("click", () => {
    const buttonText = button.getAttribute("alt");

    // Implementação dos event listeners aqui
  });
});
```

## Passo 5: Implemente as Funções
Aqui estão as funções que precisam ser implementadas: `inputDisplay`, `inputDecimal`, `prepareOperation`, `performOperation` e `clearDisplay`. Estas são responsáveis por adicionar dígitos no display da calculadora, adicionar um ponto decimal, executar operações e limpar o display, respectivamente.

### Função `inputDisplay`
A função inputDisplay é responsável por adicionar dígitos ao display quando os botões numéricos são clicados. Ela deve verificar se o limite de 8 dígitos é atingido e se estamos esperando o segundo operando para evitar a substituição do valor atual.

```ts
function inputDisplay(digit: string) {
  // limita a 8 caracteres na tela, imperdindo a digitação de mais
  if (displayValue.length === 8) return;

  switch (digit) {
    case "mais":
    case "menos":
    case "por":
    case "dividido": // todos os cases até aqui vão disparar a função abaixo
      prepareOperation(digit);
      break;
    case "igual":
      performOperation();
      break;
    case "ponto":
      inputDecimal();
      break;
    default:
      // Elimina o zero à esquerda na digitação
      if (displayValue === "0") displayValue = "";
      // Insere os valores e caso seja um novo operando, o valor antigo é limpo da tela
      if (waitingForSecondOperand) {
        displayValue = digit;
        waitingForSecondOperand = false;
      } else {
        displayValue += digit;
      }
  }
  // atualiza a tela com os numeros digitados e o resultado de seus calculos
  updateDisplay();
}
```

### Função `inputDecimal`
A função inputDecimal adiciona um ponto decimal ao número atual no display. Ela verifica se um ponto decimal já foi inserido para evitar múltiplos pontos.

```ts
function inputDecimal() {
  if (!decimalEntered) {
    displayValue += ".";
    decimalEntered = true;
    updateDisplay();
  }
}
```

### Função `prepareOperation`

Função para captar todos os dados necessários para efetuar um cálculo.

```ts
function prepareOperation(operatorSign: string) {
  operator = operatorSign; // atualiza o operador digitado
  previousValue = displayValue; // salva o primeiro valor para receber o segundo valor a ser calculado
  waitingForSecondOperand = true; // indica que o `displayValue` pode ser atualizado com o novo valor
}
```

### Função `performOperation`
A função performOperation é responsável por executar as operações quando os botões de operação são clicados. Ela verifica se um operador já está definido e, se estiver, realiza a operação com base no operador anterior. Cabe usar um `switch case` ou `object literals` ao analisar qual operador foi digitado. Pense também na conversão dos numeros para realizar as operações. Aqui está um exemplo da implementação:

```ts
function performOperation() {
  switch (operator) {
    case "mais":
      // implementa a operação de soma
      displayValue = (
        parseFloat(previousValue) + parseFloat(displayValue)
      ).toString();
      break; // tenha sempre atenção de não esquecer o break para não permitir a execução dos demais cases
    case "menos":
      // implementa a operação de substração
      displayValue = (
        parseFloat(previousValue) - parseFloat(displayValue)
      ).toString();
      break;
    case "por":
      // implementa a operação de multiplicação
      displayValue = (
        parseFloat(previousValue) * parseFloat(displayValue)
      ).toString();
      break;
    case "dividido":
      // implementa a operação dee divisão. Vale lembrar de validar a divisão por zero
      if (displayValue === "0") {
        alert("não existe divisão por zero");
        return;
      }
      displayValue = (
        parseFloat(previousValue) / parseFloat(displayValue)
      ).toString();
    default:
      break;
  }

  previousValue = displayValue;
}


```

### Função sd

Função para limpar o display e o estado, rreplicando o comportamento da calculadora física. 

```ts
function clearDisplay() {
  // Implementação do botão ON/C da calculadora
  displayValue = "0";
  previousValue = "";
  operator = null;
  waitingForSecondOperand = false;
  decimalEntered = false;
  updateDisplay();
}
```

## Extra: Estrutura Modular (Classes)

Se desejar trabalhar com a organização do código usando `class`, poderá seguir essa estrutura:

```ts
class Calculator {
  static buttons = document.querySelectorAll(".tecla");
  static display = document.getElementById("display") as HTMLSpanElement;

  static displayValue: string = "0";

  static updateDisplay = () => {
    this.display.textContent = this.displayValue;
  };

  static inputDisplay = (digit: string) => {
    if (this.displayValue.length >= 8) {
      return; // Limite máximo de 8 dígitos
    }

    if (this.displayValue === "0") this.displayValue = "";
    this.displayValue += digit;

    this.updateDisplay();
  };

  // As demais funções vem aqui.

  static start = () => {
    this.buttons.forEach((button) => {
      button.addEventListener("click", () => {
        const buttonText = button.getAttribute("id");

        this.inputDisplay(buttonText!);
      });
    });
  };
}

Calculator.start();
```

## Passo 6: Compilação do TypeScript
Abra um terminal na pasta onde está o arquivo `js/app.ts` e execute o comando TypeScript para compilar o código:

```bash
npx tsc app.ts
```

Isso criará/atualizará o arquivo `app.js`.

## Passo 7: Vincule o arquivo JavaScript ao HTML
No seu arquivo HTML, certifique-se de incluir o arquivo JavaScript gerado no trecho de código `<script src="js/app.js"></script>` existente no elemento `<body>`.

## Passo 8: Teste a Calculadora
Agora, abra o seu arquivo HTML em um navegador da web. Você deve ver a calculadora funcional e pode começar a realizar cálculos.

Certifique-se de testar cada funcionalidade para garantir que tudo funcione conforme o esperado.

Este tutorial deve ajudá-lo a construir a calculadora com base no código que você forneceu. Certifique-se de seguir cada passo com cuidado e implementar as funções restantes para tornar a calculadora completamente funcional.