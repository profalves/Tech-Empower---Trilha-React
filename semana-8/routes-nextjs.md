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

Para acessar **query params**, você pode usá-los diretamente no objeto `query` do `useRouter`:

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

## Conclusão

Com esses exemplos acima, você deve ter uma compreensão básica de como trabalhar com rotas no `Next.js`, incluindo parâmetros de rota, parâmetros de consulta, navegação programática e o uso geral do `useRouter`. Lembre-se de adaptar e expandir esses conceitos de acordo com os requisitos específicos do seu projeto.

