# Introdução às Rotas no `Next.js`

O ``Next.js`` possui um sistema de roteamento integrado que permite criar aplicativos com várias páginas. Cada arquivo `JavaScript` (ou `TypeScript`) na pasta `pages` do seu projeto `Next.js` se torna uma rota automaticamente. Por exemplo, um arquivo chamado `index.tsx` em pages representa a rota raiz `/`, enquanto um arquivo chamado `about.tsx` representa a rota `/about`.

## AppRouter no `Next.js`

O ``Next.js`` não requer uma biblioteca de terceiros para roteamento básico, mas você pode usar o componente `Link` para criar links de navegação entre as páginas. Aqui está um exemplo de como você pode usar o `Link` para criar um menu de navegação:

```tsx
// components/Navbar.tsx
import Link from 'next/link';

const Navbar: React.FC = () => {
  return (
    <nav>
      <ul>
        <li>
          <Link href="/">Home</Link>
        </li>
        <li>
          <Link href="/about">About</Link>
        </li>
      </ul>
    </nav>
  );
};

export default Navbar;
```

## Usando `useRouter`

No `Next.js`, você pode usar o hook `useRouter` para acessar o objeto de roteamento. Ele fornece informações sobre a rota atual.

```tsx
// pages/meu-componente.js
import { useRouter } from 'next/router';

const MeuComponente = () => {
  const router = useRouter();

  return (
    <div>
      Rota Atual: {router.pathname}
    </div>
  );
};

export default MeuComponente;
```

## Parâmetros de Rota e Query Params:

Você pode acessar **parâmetros de rota** usando colchetes `[]` em seus arquivos de rota. Por exemplo, se você criar um arquivo chamado `[id].tsx` em `pages`, você pode acessar o parâmetro `id` usando `useRouter` do `Next.js`:

```tsx
// pages/[id].tsx
import { useRouter } from 'next/router';

const Post: React.FC = () => {
  const router = useRouter();
  const { id } = router.query;

  return <div>Post ID: {id}</div>;
};

export default Post;
```

Para acessar **query params**, você pode usá-los diretamente no objeto `query` do `useRouter`. Caso a rota seja com `/posts?id=123`:

```tsx
// pages/post.tsx
import { useRouter } from 'next/router';

const Post: React.FC = () => {
  const router = useRouter();
  const { id } = router.query;

  return <div>Post ID: {id}</div>;
};

export default Post;
```

## Navegação Programática:

Você pode usar o hook `useRouter` para navegação programática no `Next.js`. Por exemplo, você pode criar uma função para lidar com um formulário e, após o envio bem-sucedido, redirecionar o usuário para outra página:

```tsx
// components/PostForm.tsx
import { useRouter } from 'next/router';

const PostForm: React.FC = () => {
  const router = useRouter();

  const handleSubmit = async (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    // Lógica de envio do formulário

    // Redirecionar após o envio bem-sucedido
    router.push('/success');
  };

  return (
    <form onSubmit={handleSubmit}>
      {/* Campos do formulário */}
      <button type="submit">Enviar</button>
    </form>
  );
};

export default PostForm;
```

> Neste exemplo, `router.push('/success')` navegará o usuário para a rota `/success` após o envio do formulário.

Também podemos ter um botão de voltar caso usarmos o `router.back()` como função para este botão.

## Modificando a página 404

Para personalizar a página de erro 404 (página não encontrada) em uma aplicação Next.js, você pode criar um arquivo chamado `404.js` ou `404.tsx` na pasta pages do seu projeto. Quando o Next.js não encontrar uma rota correspondente, ele irá usar o conteúdo desse arquivo para renderizar a página de erro 404.

Aqui está um exemplo básico de como você pode personalizar sua página 404:

```tsx
// pages/404.tsx
import { FC } from 'react';

const Custom404: FC = () => {
  return (
    <div style={{ textAlign: 'center', padding: '100px' }}>
      <h1>Oops! Página não encontrada!</h1>
      <p>Desculpe, a página que você está procurando não foi encontrada.</p>
    </div>
  );
};

export default Custom404;
```

Neste exemplo, o componente `Custom404` é uma página React que será exibida quando um usuário acessar uma rota que não existe.

1. **Personalize conforme necessário**: Você pode personalizar o conteúdo da página 404 da maneira que desejar. Adicione estilos, imagens ou qualquer outro conteúdo que achar apropriado para sua aplicação.

2. **Teste a Página 404**: Agora, se você acessar uma rota que não existe em sua aplicação, o Next.js irá renderizar o conteúdo do arquivo `404.tsx`.

## Tratamento de erros em rotas de forma global

Além disso, se você quiser controlar o comportamento de roteamento em relação aos erros 404, você pode usar o componente ErrorBoundary do Next.js para manipular esses erros em um nível mais global. Por exemplo:

```ts
import { ErrorBoundary } from 'react-error-boundary';

function MyApp({ Component, pageProps }: AppProps) {
  return (
    <ErrorBoundary
      onReset={() => {
        // Reset the state of your app so the user can try again.
      }}
      onError={(error, stackTrace) => {
        // Handle the error
      }}
    >
      <Component {...pageProps} />
    </ErrorBoundary>
  );
}

export default MyApp;
```

Dentro do `onError` callback, você pode redirecionar o usuário para uma página de erro customizada ou fazer outras manipulações necessárias em caso de erros na aplicação.

## Conclusão

Com esses exemplos acima, você deve ter uma compreensão básica de como trabalhar com rotas no `Next.js`, incluindo parâmetros de rota, parâmetros de consulta, navegação programática e o uso geral do `useRouter`. Lembre-se de adaptar e expandir esses conceitos de acordo com os requisitos específicos do seu projeto.

## Docs

- [Páginas](https://nextjs.org/docs/pages/building-your-application/routing/pages-and-layouts)
- [Rotas Dinamicas](https://nextjs.org/docs/pages/building-your-application/routing/dynamic-routes)

## Desafio

Conforme o exemplo trabalhado na aula usando uma lista de usuários e acessar um usuário da lista em outra página passando o `id` dele pela rota, podemos fazer o mesmo com uma lista de produtos. Os produtos podem ficar em uma lista em um arquivo que terá o seguinte objeto para cada com as propriedades

```ts
[
  {
    id: '1',
    name: 'Uvas',
    image: 'https://picsum.photos/id/1/200/',
    price: 18.75
  },
  ...
]
```

Depois é só seguir conforme a utilização de rotas dinâmicas, conforme o material [acima](#parâmetros-de-rota-e-query-params)