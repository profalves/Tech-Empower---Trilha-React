# Funções Assíncronas no TypeScript: Promises, Async/Await e Mais

## Introdução

Programação assíncrona é fundamental em `JavaScript` e `TypeScript` para lidar com *operações não bloqueantes*, como solicitações de rede, leitura de arquivos e outras tarefas que podem levar tempo para serem concluídas. Isso não é exclusivo da linguagem, muitas linguagens dependem deste recurso. 

## Funções Síncronas vs. Funções Assíncronas

Funções síncronas bloqueiam a execução do código até que a operação seja concluída, o que pode causar atrasos em operações demoradas. Funções assíncronas, por outro lado, permitem que o programa continue executando outras operações enquanto aguarda a conclusão da operação assíncrona.

## Promises

As `Promise`s são objetos que representam o resultado de uma operação assíncrona. Elas podem estar em um dos três estados: pendente(`<pending>`), resolvida (`<fulfilled>`) ou rejeitada (`<rejected>`). As Promises podem ser criadas usando o construtor Promise.

```typescript
const minhaPromise = new Promise((resolve, reject) => {
  // Lógica assíncrona aqui
  if (sucesso) {
    resolve("Operação bem-sucedida");
  } else {
    reject("Operação falhou");
  }
});
```

### Tipos de Promises

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
const promises: Promise<any>[] = [promise1, promise2, promise3];
Promise.all(promises)
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
async function minhaFuncaoAssincrona() {
  try {
    const resultado = await minhaPromise;
    console.log("Sucesso:", resultado);
  } catch (erro) {
    console.error("Erro:", erro);
  }
}
```

## Usando a API fetch()

Neste exemplo, vamos acessar a API de produtos do [DummyJSON](https://dummyjson.com/products).

Para isso, vamos fazer uma Requisição HTTP para o servidor. Em uma requisição HTTP, enviamos uma solicitação para o servidor e ele nos envia uma resposta de volta. Neste caso, vamos enviar uma solicitação para obter um arquivo `JSON` do servidor. Entao usaremos a API `fetch()`, que é a substituição moderna baseada em promise para `XMLHttpRequest`.

Cole o seguinte código no console do seu navegador:

```ts
fetch('https://dummyjson.com/products')
.then(res => res.json())
.then(console.log);
```

## Conclusão

A programação assíncrona no `TypeScript` resolve problemas relacionados à eficiência, melhorando a responsividade das aplicações e permitindo a execução de várias operações simultaneamente. As `Promises`, `async/await` e outras funcionalidades discutidas neste tutorial são ferramentas poderosas para lidar com operações assíncronas de forma eficaz e compreensível. Certifique-se de entender bem esses conceitos para escrever código robusto e reativo em TypeScript.

## Docs

- <https://developer.mozilla.org/pt-BR/docs/Learn/JavaScript/Asynchronous/Introducing>
- <https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/async_function>
- <https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/await>
- <https://developer.mozilla.org/pt-BR/docs/Learn/JavaScript/Asynchronous/Promises>