# Funções Assíncronas no TypeScript: Promises, Async/Await e Mais

## Introdução

Programação assíncrona é fundamental em `JavaScript` e `TypeScript` para lidar com *operações não bloqueantes*, como solicitações de rede, leitura de arquivos e outras tarefas que podem levar tempo para serem concluídas. Isso não é exclusivo da linguagem, muitas linguagens dependem deste recurso. 

## Funções Síncronas vs. Funções Assíncronas

Funções síncronas bloqueiam a execução do código até que a operação seja concluída, o que pode causar atrasos em operações demoradas. Funções assíncronas, por outro lado, permitem que o programa continue executando outras operações enquanto aguarda a conclusão da operação assíncrona.

## Promises

As `Promise`s são objetos que representam o resultado de uma operação assíncrona. Elas podem estar em um dos três estados: pendente(`<pending>`), resolvida (`<fulfilled>`) ou rejeitada (`<rejected>`). As Promises podem ser criadas usando o construtor Promise.

```typescript
function minhaPromise(): Promise<string> {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const sucesso = true; // false para simular a falha
      if (sucesso) {
        resolve("Operação bem-sucedida");
      } else {
        reject("Operação falhou");
      }
    }, 1000);
  });
}
```

### Tipos em Promises

Existem várias bibliotecas que fornecem tipos de Promises específicos, como` Promise<void>`, `Promise<number>`, etc., para garantir que o tipo de dado esperado seja retornado.

```typescript
const minhaPromise: Promise<number> = new Promise((resolve, reject) => {
  // Lógica assíncrona aqui
  resolve(42);
});
```

### `Promise.all` e `Promise.race`

`Promise.all` é usado para esperar que todas as Promises em um array sejam resolvidas, enquanto `Promise.race` é usado para esperar que a primeira Promise em um array seja resolvida.

```typescript
const promises: Promise<any>[] = [promise1, promise2, promise3]; // lista de funções promises

Promise.all(promises)
  .then(resultados => {
    console.log("Todas as Promises foram resolvidas:", resultados);
  })
  .catch(erro => {
    console.error("Pelo menos uma Promise foi rejeitada:", erro);
  });
  
Promise.race(promises)
  .then(resultados => {
    console.log("Todas as Promises foram resolvidas:", resultados);
  })
  .catch(erro => {
    console.error("Pelo menos uma Promise foi rejeitada:", erro);
  });
```

### `.then()` e `.catch()`

`.then()` é usado para manipular o resultado de uma Promise resolvida, enquanto `.catch()` é usado para lidar com erros em Promises rejeitadas.

```typescript
minhaPromise
  .then(resultado => {
    console.log("Sucesso:", resultado);
  })
  .catch(erro => {
    console.error("Erro:", erro);
  });
```

### Async/Await

`async/await` é uma sintaxe mais limpa e legível para trabalhar com Promises. Uma função marcada com async sempre retorna uma `Promise`, permitindo o uso de `await` para aguardar o resultado da Promise.

```typescript
async function operacaoAssincrona(): Promise<string> {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const sucesso = true;
      if (sucesso) {
        resolve("Operação bem-sucedida");
      } else {
        reject("Operação falhou");
      }
    }, 1000);
  });
}

// Utilizando Async/Await
async function executarOperacao() {
  try {
    const resultado = await operacaoAssincrona();
    console.log("Sucesso:", resultado);
  } catch (erro) {
    console.error("Erro:", erro);
  }
}

executarOperacao();

```

## Usando a API `fetch()`

Neste exemplo, vamos acessar a API de produtos do [DummyJSON](https://dummyjson.com/products).

Para isso, vamos fazer uma Requisição HTTP para o servidor. Em uma requisição HTTP, enviamos uma solicitação para o servidor e ele nos envia uma resposta de volta. Neste caso, vamos enviar uma solicitação para obter um arquivo `JSON` do servidor. Entao usaremos a API nativa do Javascript `fetch()`, que é a substituição moderna baseada em `promise` para `XMLHttpRequest`.

Cole o seguinte código no seu componente:

```ts
const [products, setProducts] = useState<Product[]>([]);

const getProducts = async () => {
  const response = await fetch("https://dummyjson.com/products");
  const data = await response.json();

  return data;
};

useEffect(() => {
  getProducts()
    .then((data) => setProducts(data.products))
    .catch((error) => console.log(error));
}, []);
```

## Try-Catch: Tratando Erros 

O bloco `try..catch` é uma estrutura fundamental em programação para gerenciamento de exceções. Em `TypeScript`, assim como em outras linguagens, você pode usar `try` para envolver o código que pode gerar exceções e `catch` para lidar com essas exceções. O bloco `finally` é opcional e é usado para especificar um bloco de código que será executado, **independentemente de uma exceção ser lançada ou não**.

### 1. Tratando Erros em Requisições HTTP:

```typescript
import axios from 'axios';

async function fetchData() {
    try {
        const response = await axios.get('https://api.example.com/data');
        console.log(response.data);
    } catch (error) {
        console.error('Erro na requisição HTTP:', error);
        // Tratar o erro, exibir uma mensagem para o usuário, ou fazer outra operação adequada.
    }
}
```

> Neste exemplo, o bloco `try` envolve a chamada à API usando o `axios`. Se a requisição falhar, o controle é transferido para o bloco `catch`, onde você pode lidar com o erro de forma apropriada.

### 2. Outros Cenários Comuns:

#### a) Manipulando Erros de Parsing JSON:

```typescript
function parseJSON(data: string) {
    try {
        return JSON.parse(data);
    } catch (error) {
        console.error('Erro ao fazer parsing do JSON:', error);
        // Tratar o erro, fornecer um valor padrão, ou realizar outra lógica apropriada.
        return null;
    }
}
```

> Aqui, o bloco `try` tenta fazer o parsing do JSON. Se o JSON for inválido, o controle é transferido para o bloco `catch`.

#### b) Lidando com Operações Assíncronas:

```typescript
async function performAsyncOperation() {
    try {
        const result = await asyncFunction();
        console.log('Operação assíncrona bem-sucedida:', result);
    } catch (error) {
        console.error('Erro na operação assíncrona:', error);
    } finally {
        console.log('Este bloco sempre será executado, independente de erro ou não.');
        // Código que precisa ser executado independentemente de haver um erro ou não.
    }
}
```

> No exemplo acima, o bloco `finally` contém código que deve ser executado independentemente de ocorrer um erro ou não. Por exemplo, pode ser usado para limpar recursos ou realizar operações que precisam acontecer, não importa o que ocorra no `try` ou no `catch`.

O uso adequado de **try-catch** é crucial para lidar com exceções e erros em uma aplicação TypeScript. Isso ajuda a tornar a aplicação mais robusta, fornecendo uma maneira de gerenciar situações excepcionais de forma controlada e elegante. Além disso, o bloco **finally** é útil para garantir que certas operações sejam sempre realizadas, independentemente de ocorrer um erro ou não.

## Conclusão

A programação assíncrona no `TypeScript` resolve problemas relacionados à eficiência, melhorando a responsividade das aplicações e permitindo a execução de várias operações simultaneamente. As `Promises`, `async/await` e outras funcionalidades discutidas neste tutorial são ferramentas poderosas para lidar com operações assíncronas de forma eficaz e compreensível. Certifique-se de entender bem esses conceitos para escrever código robusto e reativo em TypeScript.

## Docs

- <https://developer.mozilla.org/pt-BR/docs/Learn/JavaScript/Asynchronous/Introducing>
- <https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/async_function>
- <https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/await>
- <https://developer.mozilla.org/pt-BR/docs/Learn/JavaScript/Asynchronous/Promises>
- <https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/try...catch>
- <https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Control_flow_and_error_handling>