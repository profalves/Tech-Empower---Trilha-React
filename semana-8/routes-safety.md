# Rotas Protegidas com Next.js, TypeScript e React Router Dom

A segurança é uma preocupação primordial ao desenvolver aplicações web modernas. Garantir que apenas usuários autenticados e autorizados tenham acesso a determinadas partes do seu aplicativo é essencial para proteger dados sensíveis e funcionalidades importantes. Então, vamos explorar como implementar rotas protegidas em uma aplicação `Next.js`. Além disso, vamos comparar a abordagem `Next.js` com uma aplicação React padrão usando `react-router-dom`.

## Parte 1: Implementando Rotas Protegidas com Next.js

Vamos começar criando rotas protegidas em uma aplicação `Next.js`. Vamos usar o `Next.js` para gerenciar o roteamento do lado do servidor e o TypeScript para garantir tipagem segura.

### 1.1 Criando uma Rota Protegida:

Primeiro, crie uma página protegida `account.tsx` na pasta pages. Esta será a página para a qual os usuários autenticados serão redirecionados.

```tsx
// pages/account.tsx
import { useRouter } from 'next/router';
import { useEffect } from 'react';

const AccountPage = () => {
  const router = useRouter();

  useEffect(() => {
    // Lógica de verificação de autenticação (pode ser um token em localStorage ou cookies)
    const isAuthenticated = true;

    if (!isAuthenticated) {
      router.push('/login'); // Redirecionar para a página de login se não estiver autenticado
    }
  }, []);

  return <div>Página da Conta Protegida</div>;
};

export default AccountPage;
```

### 1.2 Configurando as Rotas:

Configure suas rotas em pages/index.tsx:

```tsx
// pages/index.tsx
import Link from 'next/link';

const HomePage = () => {
  return (
    <div>
      <h1>Home</h1>
      <Link href="/account">Ir para a Página da Conta</Link>
    </div>
  );
};

export default HomePage;
```

## Parte 2: Comparação com React e react-router-dom

Vamos agora comparar esta abordagem com uma aplicação `React` padrão usando `react-router-dom`.

### 2.1 Implementando Rotas Protegidas com react-router-dom:

Para implementar rotas protegidas em uma aplicação `React` com `react-router-dom`, você pode usar um componente de rota personalizado.

```tsx
import { BrowserRouter as Router, Route, Redirect } from 'react-router-dom';

const PrivateRoute = ({ component: Component, isAuthenticated, ...rest }) => {
  return (
    <Route
      {...rest}
      render={(props) =>
        isAuthenticated ? <Component {...props} /> : <Redirect to="/login" />
      }
    />
  );
};
```

### 2.2 Configurando as Rotas com react-router-dom:

```tsx
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

const App = () => {
  const isAuthenticated = true; // Lógica de verificação de autenticação

  return (
    <Router>
      <Switch>
        <Route path="/login" component={LoginPage} />
        <PrivateRoute
          path="/account"
          component={AccountPage}
          isAuthenticated={isAuthenticated}
        />
        <Route path="/" component={HomePage} />
      </Switch>
    </Router>
  );
};
```

## Conclusão: Comparando Next.js e React com react-router-dom

Ambas as abordagens oferecem soluções eficazes para rotas protegidas em aplicações `React`. O `Next.js`, com sua renderização do lado do servidor (`SSR`), oferece uma abordagem mais completa e integrada, especialmente quando a complexidade da aplicação aumenta.

Por outro lado, uma aplicação `React` padrão com `react-router-dom` oferece mais flexibilidade e controle sobre o roteamento, sendo adequada para aplicativos menores ou projetos mais simples.

A escolha entre essas abordagens dependerá dos requisitos específicos do seu projeto e do nível de complexidade que você está enfrentando. Independentemente da escolha, a segurança deve sempre ser uma prioridade, garantindo que apenas usuários autenticados e autorizados tenham acesso às partes sensíveis da sua aplicação.




