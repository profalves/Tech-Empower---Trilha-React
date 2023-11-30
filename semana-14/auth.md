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
import NextAuth from 'next-auth';
import Providers from 'next-auth/providers';

export default NextAuth({
  providers: [
    Providers.Credentials({
      credentials: {
        // Configurações de como os usuários serão autenticados
        async authorize(credentials) {
          // Adicione sua lógica de autenticação aqui
          if (credentials.email === 'seu-email' && credentials.password === 'sua-senha') {
            // O retorno deve ser um objeto contendo informações do usuário
            return { id: 1, name: 'Nome do Usuário', email: 'seu-email' };
          }
          return null;
        },
      },
    }),
    // Outros provedores podem ser adicionados aqui (ex: Google, Facebook, etc.)
  ],
  pages: {
    signIn: '/login', // Rota para a tela de login
  },
});
```

### Passo 2: Implementar a proteção da rota `/my-account`
Para proteger a rota /my-account, você pode criar um componente de verificação de autenticação (auth.tsx) para garantir que o usuário esteja autenticado antes de acessar essa rota.

```tsx
// components/auth.tsx
import { useSession } from 'next-auth/react';
import { useRouter } from 'next/router';
import React, { useEffect } from 'react';

const Auth: React.FC = ({ children }) => {
  const { data: session, status } = useSession();
  const router = useRouter();

  useEffect(() => {
    if (status === 'loading') return; // Aguarde a verificação da sessão

    if (!session) {
      router.replace('/login'); // Redireciona para a página de login se não estiver autenticado
    }
  }, [session, status, router]);

  if (status === 'loading') {
    // Exibir um loader enquanto verifica a sessão
    return <div>Carregando...</div>;
  }

  return <>{children}</>;
};

export default Auth;
```

### Passo 3: Criar as páginas de Login e My Account

Crie as páginas de login (`pages/login.tsx`) e `my-account.tsx`.

1. `login.tsx`:

```tsx
// pages/login.tsx
import { signIn } from 'next-auth/react';
import React from 'react';

const LoginPage: React.FC = () => {
  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const formData = new FormData(e.currentTarget);
    const email = formData.get('email') as string;
    const password = formData.get('password') as string;

    // Chama a função signIn do NextAuth para autenticar o usuário
    await signIn('credentials', {
      redirect: false,
      email,
      password,
    });
  };

  return (
    <div>
      <h1>Login</h1>
      <form onSubmit={handleSubmit}>
        <input type="email" name="email" placeholder="Email" />
        <input type="password" name="password" placeholder="Senha" />
        <button type="submit">Entrar</button>
      </form>
    </div>
  );
};

export default LoginPage;
```

2. `my-account.tsx`:

```tsx
// pages/my-account.tsx
import React from 'react';
import Auth from '../components/auth';

const MyAccountPage: React.FC = () => {
  return (
    <Auth>
      <div>
        <h1>Minha Conta</h1>
        {/* Conteúdo da página de conta */}
      </div>
    </Auth>
  );
};

export default MyAccountPage;
```

### Passo 4: Implementar a funcionalidade de logout

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

No seu frontend, onde você deseja realizar o logout, você pode criar um botão que acione uma função para chamar o endpoint `/api/logout`.

Isso deve te dar uma boa base para implementar a autenticação com `NextAuth.js`, proteger a rota `/my-account` e implementar o logout em seu projeto Next.js com TypeScript. Lembre-se de ajustar as configurações de autenticação de acordo com suas necessidades de segurança!

Lembre-se de que este é um exemplo básico para entender o conceito de rotas protegidas com Next.js. Em um ambiente de produção, você deve considerar usar soluções mais seguras para armazenar e gerenciar tokens de autenticação, bem como autenticação de dois fatores para melhorar a segurança. Além disso, sempre valide e verifique os tokens do lado do servidor para garantir a segurança da sua aplicação.