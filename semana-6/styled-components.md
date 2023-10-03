# Styled Components

## O que é CSS-in-JS?

**CSS-in-JS** é uma abordagem para escrever estilos em aplicações web. Em vez de criar arquivos CSS separados, você escreve estilos diretamente no arquivo JavaScript ou TypeScript, dentro dos seus componentes. Isso tem algumas vantagens, como escopo de estilo encapsulado, melhor suporte para componentes reutilizáveis e a capacidade de usar lógica dinâmica para estilos.

`Styled Components` é uma biblioteca popular para CSS-in-JS no ecossistema React. Ele permite que você crie componentes React com estilos encapsulados e reutilizáveis, todos dentro do mesmo arquivo.

## Como criar um componente estilizado usando Styled Components e TypeScript:

### Pré-requisitos:

Certifique-se de ter o `Node.js` e o `npm` (ou `yarn`) instalados em seu sistema.

### Passo 1: Criar um novo projeto React com TypeScript

```bash
npx create-next-app styled-components-demo
cd styled-components-demo
```

### Passo 2: Instalar Styled Components

```bash
npm install styled-components
# or
yarn add styled-components
```

### Passo 3: Criar um componente estilizado

```tsx
import React from 'react';
import styled from 'styled-components';

// Definindo um componente estilizado usando Styled Components
const StyledDiv = styled.div`
  background-color: lightblue;
  padding: 20px;
  border-radius: 5px;
  text-align: center;
`;

// Criando um componente funcional que usa o Styled Component
const Component = () => {
  return <StyledDiv>Este é um componente estilizado usando Styled Components!</StyledDiv>;
};

export default StyledComponent;
```

Neste exemplo, `StyledDiv` é um componente estilizado usando **Styled Components**. Ele é uma `div` com estilos CSS definidos dentro da string de template.

### Passo 4: Usar o componente estilizado em seu aplicativo

Abra o arquivo `src/pages/index.tsx` e use o componente estilizado:

```tsx
import React from 'react';
import StyledComponent from './StyledComponent';

function Home() {
  return (
    <div >
      <StyledComponent />
    </div>
  );
}
```

### Passo 5: Executar o aplicativo

```bash
npm run dev
# or
yarn dev
```

## Conclusão:

Você aprendeu sobre CSS-in-JS e como usar Styled Components para criar componentes estilizados em um aplicativo React com TypeScript. Lembre-se de que Styled Components oferece muitos recursos avançados, como a capacidade de passar props dinamicamente para estilos e criar temas para sua aplicação. Consulte a documentação oficial do Styled Components para aprender mais sobre esses recursos e aprofundar seus conhecimentos.