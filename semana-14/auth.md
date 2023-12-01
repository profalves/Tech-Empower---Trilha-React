# Autenticação e autorização em APIs

A segurança é uma das preocupações fundamentais ao desenvolver aplicativos da web modernos. Uma parte crucial desse processo é garantir que apenas usuários autenticados e autorizados tenham acesso a determinadas funcionalidades e dados sensíveis. Neste tutorial, vamos explorar como implementar um sistema robusto de autenticação e autorização em APIs usando Next.js, TypeScript e React Hook Form.

## O que Vamos Cobrir:

- **Autenticação do Usuário**: Vamos começar criando um sistema de autenticação seguro. Para gerenciar nossos formulários de login, aproveitaremos o poder do React Hook Form para uma validação fácil e reativa.

- **Autorização e Rotas Protegidas**: Vamos aprender como proteger rotas específicas da nossa aplicação para garantir que apenas usuários autenticados possam acessá-las. Implementaremos rotas protegidas, permitindo o acesso apenas a usuários autorizados, e examinaremos estratégias para garantir que somente aqueles com as permissões adequadas possam visualizar conteúdo específico.

- **Exemplo Prático com um Backend Simulado**: Para ilustrar esses conceitos, criaremos um exemplo prático usando um backend simulado. Isso nos permitirá focar nos aspectos de autenticação e autorização, sem se preocupar com a complexidade de um servidor real.


## Mão na massa

### Passo 1: Instalar e configurar o `NextAuth.js`

Instale as dependências do `NextAuth` e também dos provedores que você deseja utilizar (neste caso, vamos usar o provedor de email/senha):

```bash
yarn add next-auth
```

Crie um arquivo `pages/api/auth/[...nextauth].ts` para lidar com as rotas de autenticação. Este será o endpoint usado pelo NextAuth:

```typescript
import NextAuth, { NextAuthOptions } from "next-auth";
import CredentialsProvider from "next-auth/providers/credentials";

const nextAuthOption: NextAuthOptions = {
  providers: [
    CredentialsProvider({
      name: "credentials",
      credentials: {
        username: { label: "username", type: "text" },
        password: { label: "password", type: "password" },
      },
      async authorize(credentials, req) {
        const response = await fetch("https://dummyjson.com/auth/login", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({
            username: credentials?.username,
            password: credentials?.password,
          }),
        });

        const user = await response.json();

        if (user && response.ok) {
          return user;
        }

        return null;
      },
    }),
  ],
  pages: {
    signIn: "/",
  },
  callbacks: {
    async jwt({ token, user }) {
      user && (token.user = user);
      return token;
    },
    async session({ session, token }) {
      session = token.user as any;
      return session;
    },
  },
};

const handler = NextAuth(nextAuthOption);

export { handler as GET, handler as POST, nextAuthOption };
```

### Passo 2: Configuração do Next-Auth Provider

Dentro do arquivo `_app.tsx` adicione o provider para configurar o Next-Auth Provider e pegar os dados da sessão:

```tsx
import { Provider } from 'next-auth/react'
import { AppProps } from 'next/app'

function MyApp({ Component, pageProps }: AppProps) {
  return (
    <Provider session={pageProps.session}>
      <Component {...pageProps} />
    </Provider>
  )
}

export default MyApp
```

### Passo 3: Implementar a proteção da rota `/my-account`

Para proteger a rota `/my-account`, e caso você usa a pasta `app`, pode criar um arquivo de layout verificação de autenticação para garantir que o usuário esteja autenticado antes de acessar essa rota.

```tsx
// app/(private-routes)/my-account/layout.tsx
import React from "react";
import { getServerSession } from "next-auth";
import { nextAuthOption } from "../../api/auth/[...nextauth]/route";
import ButtonLogoff from "@/components/ButtonLougout";

export default async function page() {
  const session = await getServerSession(nextAuthOption);

  return (
    <div className="w-full h-screen flex flex-col items-center justify-center">
      <img src={session?.image} alt={session?.email} />
      <p>
        Olá, {session?.firstName} {session?.lastName}, seja Bem-vindo(a)!
      </p>
      <ButtonLogoff />
    </div>
  );
}
```

Caso o seu projeto next não use a pasta `app`, crie um arquivo `my-account.ts` na pasta api para proteger a rota:

```tsx
// api/my-account.ts

import { getSession } from 'next-auth/react'
import { NextApiRequest, NextApiResponse } from 'next'

export default async (req: NextApiRequest, res: NextApiResponse) => {
  const session = await getSession({ req })

  if (!session) {
    res.status(401).json({ error: 'Unauthorized' })
    return
  }

  // Lógica para lidar com a rota '/my-account'
  res.status(200).json({ message: 'Authorized access to my-account' })
}
```

### Passo 4: Criar as páginas de Login e My Account

Crie as páginas de login (`pages/login.tsx`) e `pages/my-account.tsx`.

1. `login.tsx`:

```tsx
// pages/login.tsx
"use client";

import React from "react";
import { signIn } from "next-auth/react";
import { useRouter } from "next/navigation";

type FormData = {
  username: { value: string };
  password: { value: string };
};

export default function page() {
  const { replace } = useRouter();

  const submitHandler = async (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();

    const target = event.target as typeof event.target & FormData;

    const username = target.username.value;
    const password = target.password.value;

    if (!username || !password) return;

    const result = await signIn("credentials", {
      username,
      password,
      redirect: false,
    });

    if (result?.error) {
      alert(result.error);
      return;
    }

    replace("/my-account");
  };

  return (
    <div className="flex flex-col items-center justify-center w-full h-screen">
      <h1 className="text-3xl mb-6">Login</h1>

      <form className="w-[400px] flex flex-col gap-6" onSubmit={submitHandler}>
        <input
          className="h-12 rounded-md p-2 bg-transparent border border-gray-300"
          type="text"
          name="username"
          placeholder="Digite seu e-mail"
        />

        <input
          className="h-12 rounded-md p-2 bg-transparent border border-gray-300"
          type="password"
          name="password"
          placeholder="Digite sua senha"
        />

        <button
          type="submit"
          className="h-12 rounded-md bg-gray-300 text-gray-800 hover:bg-gray-400"
        >
          Entrar
        </button>
      </form>
    </div>
  );
}

```

2. `my-account.tsx`:

```tsx
// pages/my-account.tsx
import React from "react";
import { getServerSession } from "next-auth";
import { nextAuthOption } from "../../api/auth/[...nextauth]/route";
import ButtonLogoff from "@/components/ButtonLougout";

export default async function MyAccountPage() {
  const session = await getServerSession(nextAuthOption);

  return (
    <div className="w-full h-screen flex flex-col items-center justify-center">
      <img src={session?.image} alt={session?.email} />
      <p>Olá, {session?.firstName}, seja Bem-vindo(a)!</p>
      <ButtonLogoff />
    </div>
  );
}

```



### Passo 5: Implementar a funcionalidade de logout

Para o logout, você pode criar um endpoint em `pages/api/logout.ts`:

```ts
// pages/api/logout.ts
import { signOut } from 'next-auth/react';

export default async function handler(req, res) {
  const result = await signOut({ redirect: false, req });
  res.clearPreviewData();
  res.status(200).json({ message: 'Deslogado com sucesso' });
}
```

Ou apenas o botão que fará o logout para ficar mais prático e poder utilizar em qualquer lugar da aplicação:

```tsx
"use client"; // como ele utiliza hooks, deverá usar essa diretiva para ser renderizado 
// no lado do browser mas o hydratation do next se encarrega de rendeliza-lo no lado do servidor 
// caso seja usado em um server component

import { signOut } from "next-auth/react";
import { useRouter } from "next/navigation";

export default function ButtonLogoff() {
  const { replace } = useRouter();

  const logout = async () => {
    await signOut({
      redirect: false,
    });

    replace("/");
  };

  return (
    <button
      onClick={logout}
      className="p-2 m-2 w-40 border border-gray-600 rounded-md"
    >
      Sair
    </button>
  );
}
```


No seu frontend, onde você deseja realizar o logout, você pode criar um botão que acione uma função para chamar o endpoint `/api/logout`.

> Para seguir o mesmo tutorial da aula, pode ver o repositório do projeto criado aqui: https://github.com/profalves/tutorial-next-auth

Isso deve te dar uma boa base para implementar a autenticação com **NextAuth.js**, proteger a rota `/my-account` e implementar o logout em seu projeto **Next.js** com **TypeScript**. Lembre-se de ajustar as configurações de autenticação de acordo com suas necessidades de segurança!

Lembre-se de que este é um exemplo básico para entender o conceito de rotas protegidas com Next.js. Em um ambiente de produção, você deve considerar usar soluções mais seguras para armazenar e gerenciar tokens de autenticação, bem como autenticação de dois fatores para melhorar a segurança. Além disso, ***sempre valide e verifique os tokens do lado do servidor para garantir a segurança da sua aplicação***.

Isso é um guia básico para implementar autenticação usando `next-auth` e a estrutura de páginas do Next.js. Lembre-se de adaptar esses passos de acordo com as necessidades específicas do seu projeto e realizar tratamento de erros, validações e outras funcionalidades de acordo com as melhores práticas.

## Docs

- <https://next-auth.js.org/getting-started/introduction>
- <https://tailwindui.com/documentation>