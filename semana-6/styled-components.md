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

## Uso de props 

```jsx
import styled from 'styled-components';

interface ButtonProps {
  size?: 'small' | 'medium' | 'large';
  color?: 'primary' | 'secondary' | 'danger';
}

const StyledButton = styled.button<ButtonProps>`
  padding: ${(props) => {
    switch (props.size) {
      case 'small':
        return '5px 10px';
      case 'medium':
        return '10px 20px';
      case 'large':
        return '15px 30px';
      default:
        return '10px 20px'; // Tamanho padrão
    }
  }};
  background-color: ${(props) => {
    switch (props.color) {
      case 'primary':
        return '#007bff';
      case 'secondary':
        return '#6c757d';
      case 'danger':
        return '#dc3545';
      default:
        return '#007bff'; // Cor padrão
    }
  }};
  color: #ffffff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
`;
```

## Vantagens de usar Styled-Components

Styled Components é uma biblioteca popular para estilizar componentes em aplicações React. Ela traz várias vantagens em comparação com os métodos tradicionais de estilização, como usar arquivos CSS separados ou estilos embutidos diretamente no JSX. Aqui estão algumas das vantagens de usar Styled Components no React:

### 1. Encapsulamento de Estilos:
- **Escopo Local**: Os estilos são encapsulados no escopo do componente, o que significa que não há vazamentos de estilos para outros componentes.
- **Evita Conflitos**: Reduz a probabilidade de conflitos de nomenclatura de classe, pois os estilos são específicos para cada componente.
### 2. Facilidade de Manutenção:
- **Leitura e Manutenção Simples**: Os estilos estão diretamente relacionados ao componente, facilitando a leitura e a manutenção do código.
- **Refatoração Fácil**: Renomear um componente automaticamente renomeia seus estilos, facilitando a refatoração do código.
### 3. Dinamismo e Reatividade:
- **Estilos Dinâmicos**: Permite a criação de estilos dinâmicos com base em props ou estado do componente.
- **Reatividade**: Os estilos são reavaliados e atualizados automaticamente quando as props ou o estado do componente mudam, proporcionando uma experiência reativa.
### 4. Melhor Suporte a Temas:
- **Temas Reutilizáveis**: Facilita a criação e aplicação de temas consistentes em toda a aplicação.
- **Alteração Dinâmica de Temas**: Permite a alteração dinâmica do tema da aplicação com base nas preferências do usuário.
### 5. Performance Otimizada:
- **Otimização de Renderização**: Utiliza uma técnica de cache para otimizar a renderização, garantindo que estilos sejam injetados no DOM apenas quando necessário.
- **Remoção de Estilos Não Utilizados**: Em produção, o Styled Components remove automaticamente os estilos não utilizados, melhorando o desempenho da aplicação.
### 6. Interpolamento de Strings e Pré-processadores:
- **Interpolação de Strings**: Permite o uso de variáveis e funções JavaScript diretamente nos estilos, proporcionando maior flexibilidade.
- **Pré-processadores CSS**: Oferece suporte para pré-processadores CSS, como Sass e Less, para escrever estilos de forma mais avançada.
### 7. Ecossistema Rico e Ativo:
- **Plugins e Extensões**: Possui um ecossistema ativo com plugins e extensões para integração com outras bibliotecas e ferramentas.
- **Documentação Abundante**: Há uma documentação rica e comunidade ativa, facilitando a aprendizagem e a resolução de problemas.

Em resumo, Styled Components oferece uma maneira moderna, eficiente e reativa de estilizar componentes no React. Ele ajuda os desenvolvedores a criar interfaces de usuário elegantes, dinâmicas e de fácil manutenção, enquanto proporciona um excelente desempenho e uma experiência de desenvolvimento agradável.

## Conclusão:

Você aprendeu sobre CSS-in-JS e como usar Styled Components para criar componentes estilizados em um aplicativo React com TypeScript. Lembre-se de que Styled Components oferece muitos recursos avançados, como a capacidade de passar props dinamicamente para estilos e criar temas para sua aplicação. Consulte a documentação oficial do Styled Components para aprender mais sobre esses recursos e aprofundar seus conhecimentos.

