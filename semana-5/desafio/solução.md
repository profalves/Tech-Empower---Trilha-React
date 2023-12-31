# Desafio - Criando um Tema Escuro/Luz com useContext

Vou fornecer um exemplo simplificado de como você pode criar um aplicativo ***React*** com temas **escuros** e **claros** usando `useContext`. Lembre-se de que este é um exemplo básico e você pode estender e personalizar de acordo com sua criatividade.

## Passo 1: Configuração Inicial

Certifique-se de ter um projeto React com TypeScript criado. Se você ainda não o fez, siga as etapas descritas anteriormente no tutorial.

## Passo 2: Criar o Contexto do Tema

Crie um novo arquivo chamado ThemeContext.tsx para definir o contexto do tema:

```tsx
import React, { createContext, useContext, useState } from 'react';

// Defina os tipos para o tema
type Theme = 'light' | 'dark';

type ThemeContextType = {
  theme: Theme;
  toggleTheme: () => void;
};

// Crie o contexto do tema
const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

// Crie um provedor de tema
export const ThemeProvider: React.FC = ({ children }) => {
  const [theme, setTheme] = useState<Theme>('light');

  const toggleTheme = () => {
    setTheme((prevTheme) => (prevTheme === 'light' ? 'dark' : 'light'));
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

// Crie um hook personalizado para facilitar o uso do contexto do tema
export const useTheme = () => {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within a ThemeProvider');
  }
  return context;
};
```

## Passo 3: Criar Componentes

Agora, crie os componentes `ThemeSwitch` e `Home` que usarão o contexto do tema.

```tsx
// ThemeSwitch.tsx
import React from 'react';
import { useTheme } from './ThemeContext';

const ThemeSwitch: React.FC = () => {
  const { theme, toggleTheme } = useTheme();

  return (
    <button onClick={toggleTheme}>
      {theme === 'light' ? 'Switch to Dark Theme' : 'Switch to Light Theme'}
    </button>
  );
};

export default ThemeSwitch;


// _app.tsx
import React from 'react';
// ...
import { ThemeProvider, useTheme } from './ThemeContext';
import ThemeSwitch from './ThemeSwitch';

// ...

function Home() {
  const { theme } = useTheme();
  // ...

  // adicione o componente na página principal

  return (
    ...
    <main className={`${styles.main} ${inter.className} ${styles[theme]}`}>
      <ThemeSwitch /> 
      <div className={styles.description}>
        <p>
          Get started by editing&nbsp;
          <code className={styles.code}>src/pages/index.tsx</code>
        </p>
      <div>
    </main>
  );
}

export default Home;
```

## Passo 4: Distribuindo o contexto

Como este tema irá modificar ***TODA*** a aplicação, o ideal será colocar no componente app para que o mesmo esteja disponível para toda a parte.

```tsx
import { ThemeProvider } from "@/context/ThemeContext";
import "@/styles/globals.css";
import type { AppProps } from "next/app";

export default function App({ Component, pageProps }: AppProps) {
  return (
    <ThemeProvider>
      <Component {...pageProps} />
    </ThemeProvider>
  );
}
```

## Passo 5: Estilização

Estilize o aplicativo com base no tema escolhido. Crie arquivos CSS separados para os estilos de tema claro e escuro, por exemplo, App.css, App-light.css e App-dark.css. Os estilos podem ser algo assim:

```css
/* App.css */
* {
  font-family: Arial, sans-serif;
  transition: background-color 0.3s ease-in-out;
  padding: 20px;
}

/* App-light.css */
.light {
  background-color: #ffffff;
  color: #000000;
}

/* App-dark.css */
.dark {
  background-color: #333333;
  color: #ffffff;
}
```

## Passo 6: Teste a Aplicação

Agora, quando você executa a aplicação e clica no botão **"Switch to Dark Theme"** ou **"Switch to Light Theme"**, os estilos e o tema da aplicação devem mudar dinamicamente.

Este é um exemplo básico de como usar `useContext` para alternar entre temas claros e escuros em uma aplicação React. Você pode aprimorar e personalizar os estilos e funcionalidades para atender às suas necessidades específicas.