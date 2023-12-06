# Teste Unitários

**Testes unitários** são fundamentais para garantir a estabilidade e a qualidade do código em qualquer aplicação. Vamos começar com um exemplo básico de um componente em Next.js com TypeScript e Jest.

## Mão na Massa: Testes Unitários com Jest e React Testing Library

### Passo 1: Configuração do Ambiente

Certifique-se de ter o ambiente de desenvolvimento configurado com `Next.js`, `TypeScript` e `Jest`. Você pode usar o comando npx create-next-app para criar um novo aplicativo `Next.js` com `TypeScript`.

Instale o `Jest` e as ferramentas necessárias para testes:

```bash
yarn add -D jest ts-jest jest-environment-jsdom @types/jest @testing-library/react @testing-library/react-hooks @testing-library/jest-dom @testing-library/user-event
```

### Passo 2: Criando um Componente

Vamos criar um componente simples para testar. Suponha que temos um componente Button em um arquivo `Button.tsx`.

```tsx
// components/Button/index.tsx
import React, { ReactNode } from "react";

type ButtonProps = {
  children: ReactNode;
  onClick: () => void;
  loading?: boolean;
  backgroundColor?: string;
};

export default function Button({
  children,
  onClick,
  loading = false,
  backgroundColor = "green",
}: ButtonProps) {
  return (
    <button
      data-testid="custom-button"
      onClick={onClick}
      style={{ backgroundColor, color: "white" }}
    >
      {loading ? "carregando..." : children}
    </button>
  );
}
```

### Passo 3: Escrevendo Testes Unitários

Vamos criar um arquivo de teste `Button.test.tsx` para testar o componente Button.

```tsx
// components/Button/Button.test.tsx
import React from "react";
import Button from "./Button";
import { fireEvent, render } from "@testing-library/react";

describe("Button suite tests", () => {
  it("renders button correctly", () => {
    const handleClick = jest.fn();
    const { getByRole } = render(
      <Button onClick={handleClick}>Click Me</Button>
    );

    const buttonTag = getByRole("button");

    expect(buttonTag).toBeInTheDocument();
  });

  it("should click function called", () => {
    const handleClick = jest.fn();
    const { getByTestId } = render(
      <Button onClick={handleClick}>Click Me</Button>
    );

    const buttonElement = getByTestId("custom-button");

    fireEvent.click(buttonElement);

    expect(handleClick).toHaveBeenCalled();
  });

  it("should render loading message", () => {
    const handleClick = jest.fn();
    const { getByText } = render(
      <Button onClick={handleClick} loading>
        Click Me
      </Button>
    );

    const buttonText = getByText("carregando");

    expect(buttonText).toBeVisible();
  });

  it("should render background color button", () => {
    const handleClick = jest.fn();
    const { getByTestId } = render(
      <Button onClick={handleClick} backgroundColor="red">
        Click Me
      </Button>
    );

    const buttonElement = getByTestId("custom-button");

    expect(buttonElement).toHaveStyle({
      backgroundColor: "red",
    });
  });

  it("should render background color green by default", () => {
    const handleClick = jest.fn();
    const { getByTestId } = render(
      <Button onClick={handleClick}>Click Me</Button>
    );

    const buttonElement = getByTestId("custom-button");

    expect(buttonElement).toHaveStyle({
      backgroundColor: "green",
    });
  });
});
```

### Passo 4: Rodando os Testes

Para executar os testes, use o comando:

```bash
yarn test
```

## Cobertura de Código (Code Coverage)

A cobertura de código indica a porcentagem do código que está sendo executada pelos testes. Isso pode ser alcançado usando a funcionalidade de cobertura de código do Jest.

### Configuração do Jest para Cobertura de Código

No arquivo `jest.config.js` (se não existir, crie um na raiz do projeto), adicione:

```javascript
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'jsdom',
  coverageReporters: ['text', 'lcov'],
  collectCoverageFrom: ['**/*.{ts,tsx}', '!**/*.test.{ts,tsx}', '!**/node_modules/**'],
};
```

### Executando Testes com Cobertura

Para executar os testes e obter a cobertura de código, utilize o comando:

```bash
npx jest --coverage
```

Isso mostrará um resumo da cobertura no terminal e também gerará um diretório coverage com detalhes sobre a cobertura de cada arquivo.

Com isso, você tem um ambiente configurado para escrever testes unitários em componentes `Next.js` com `TypeScript` e `Jest`, além de obter a cobertura de código para garantir que seus testes estejam abrangendo a maior parte do código possível.

## Docs

- <https://testing-library.com/docs/react-testing-library/intro/>
- <https://nextjs.org/docs/pages/building-your-application/optimizing/testing#jest-and-react-testing-library>
- <https://bobbyhadz.com/blog/support-for-experimental-syntax-jsx-isnt-currently-enabled>