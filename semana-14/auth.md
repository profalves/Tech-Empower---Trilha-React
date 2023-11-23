# Autenticação e autorização em APIs

A segurança é uma das preocupações fundamentais ao desenvolver aplicativos da web modernos. Uma parte crucial desse processo é garantir que apenas usuários autenticados e autorizados tenham acesso a determinadas funcionalidades e dados sensíveis. Neste tutorial, vamos explorar como implementar um sistema robusto de autenticação e autorização em APIs usando Next.js, TypeScript e React Hook Form.

## O que Vamos Cobrir:

- **Autenticação do Usuário**: Vamos começar criando um sistema de autenticação seguro. Para gerenciar nossos formulários de login, aproveitaremos o poder do React Hook Form para uma validação fácil e reativa.

- **Segurança com `Bcrypt` e `JWT`**: Exploraremos técnicas essenciais de segurança, incluindo a `hash` de senhas usando o algoritmo `Bcrypt` e a geração de tokens de autenticação usando `JSON` Web Tokens (`JWT`). Isso garantirá que as informações confidenciais dos usuários estejam protegidas durante a transmissão e armazenamento.

- **Autorização e Rotas Protegidas**: Vamos aprender como proteger rotas específicas da nossa aplicação para garantir que apenas usuários autenticados possam acessá-las. Implementaremos rotas protegidas, permitindo o acesso apenas a usuários autorizados, e examinaremos estratégias para garantir que somente aqueles com as permissões adequadas possam visualizar conteúdo específico.

- **Exemplo Prático com um Backend Simulado**: Para ilustrar esses conceitos, criaremos um exemplo prático usando um backend simulado. Isso nos permitirá focar nos aspectos de autenticação e autorização, sem se preocupar com a complexidade de um servidor real.

Vamos criar rotas protegidas com `Next.js` usando `TypeScript` e formulários com `react-hook-form`. 

## Mão na massa

### 1. Configuração Inicial:
Primeiro, crie um novo projeto Next.js com TypeScript:

```bash
npx create-next-app nome-do-seu-projeto --typescript
cd nome-do-seu-projeto
```

### 2. Instalação de Dependências:
Instale as dependências necessárias, incluindo o `react-hook-form`:

```bash
npm install react-hook-form axios jsonwebtoken bcryptjs cookie
# or
yarn add react-hook-form axios jsonwebtoken bcryptjs cookie
```

### 3. Implementação do Login com react-hook-form:
Crie uma página de login (`pages/login.tsx`) usando `react-hook-form` para gerenciar o formulário e fazer a validação:

```tsx
// pages/login.tsx
import { FC } from 'react';
import { useForm, SubmitHandler } from 'react-hook-form';
import axios from 'axios';
import { useRouter } from 'next/router';

interface LoginFormInputs {
  email: string;
  password: string;
}

const LoginPage: FC = () => {
  const { register, handleSubmit, setError, formState: { errors } } = useForm<LoginFormInputs>();
  const router = useRouter();

  const onSubmit: SubmitHandler<LoginFormInputs> = async (data) => {
    try {
      const response = await axios.post('/api/login', data);
      if (response.data.token) {
        // Armazene o token de autenticação em um cookie ou no estado global da aplicação
        // Redirecione para a página da conta do usuário
        router.push('/account');
      }
    } catch (error) {
      setError('password', { type: 'manual', message: 'Credenciais inválidas' });
    }
  };

  return (
    <div>
      <h1>Login</h1>
      <form onSubmit={handleSubmit(onSubmit)}>
        <input type="email" placeholder="Email" {...register('email', { required: 'Este campo é obrigatório' })} />
        {errors.email && <span>{errors.email.message}</span>}

        <input type="password" placeholder="Senha" {...register('password', { required: 'Este campo é obrigatório' })} />
        {errors.password && <span>{errors.password.message}</span>}

        <button type="submit">Entrar</button>
      </form>
    </div>
  );
};

export default LoginPage;
```

### 4. Implementação da Página de Conta Protegida:
Crie uma página de conta protegida (`pages/account.tsx`) onde os usuários autenticados podem acessar:

```tsx
// pages/account.tsx
import { FC, useEffect } from 'react';
import { useRouter } from 'next/router';
import axios from 'axios';

const AccountPage: FC = () => {
  const router = useRouter();

  useEffect(() => {
    const checkAuth = async () => {
      try {
        const response = await axios.get('/api/user');
        // Se o usuário não estiver autenticado, redirecione para a página de login
        if (!response.data.userId) {
          router.push('/login');
        }
      } catch (error) {
        // Se houver um erro ao verificar a autenticação, redirecione para a página de login
        router.push('/login');
      }
    };

    checkAuth();
  }, []);

  return (
    <div>
      <h1>Bem-vindo à sua conta!</h1>
      {/* Conteúdo da página da conta do usuário */}
    </div>
  );
};

export default AccountPage;
```

### 5. Implementação da API com react-hook-form:
Para a validação do formulário usando `react-hook-form`, você pode criar um arquivo api/login.ts para lidar com a autenticação do usuário:

```tsx
// pages/api/login.ts
import { NextApiRequest, NextApiResponse } from 'next';
import bcrypt from 'bcryptjs';
import jwt from 'jsonwebtoken';

const secret = 'seu_segredo_secreto';

export default async function handler(req: NextApiRequest, res: NextApiResponse) {
  if (req.method === 'POST') {
    const { email, password } = req.body;

    // Simulando a verificação do usuário no banco de dados
    // (realize a verificação adequada em um ambiente de produção)
    const user = { id: 1, email: 'user@example.com', password: '$2a$10$kkv3J9ggX1WkpcxyNVl6x.EJHApOG0HbbkScUkmF8sJAgfkX.3aO2' };

    if (!user || !bcrypt.compareSync(password, user.password)) {
      return res.status(401).json({ error: 'Credenciais inválidas' });
    }

    // Gere um token de autenticação
    const token = jwt.sign({ userId: user.id, userEmail: user.email }, secret, { expiresIn: '1h' });
    res.json({ token });
  } else {
    res.status(405).end(); // Método não permitido
  }
}
```

Lembre-se de que este é um exemplo básico para entender o conceito de rotas protegidas com Next.js, TypeScript e react-hook-form. Em um ambiente de produção, você deve considerar usar soluções mais seguras para armazenar e gerenciar tokens de autenticação, bem como autenticação de dois fatores para melhorar a segurança. Além disso, sempre valide e verifique os tokens do lado do servidor para garantir a segurança da sua aplicação.