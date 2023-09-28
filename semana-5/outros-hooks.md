# Outros React Hooks 

## `useCallback`

O hook `useCallback` é uma ferramenta importante no desenvolvimento de aplicações React para otimizar o desempenho e evitar problemas de renderização desnecessária de componentes. Ele é usado para ***memoizar (cachear)*** funções e evitar que sejam recriadas a cada renderização de um componente. Isso é especialmente útil quando você está passando funções como props para componentes filhos ou quando essas funções são parte da dependência de um hook, como o useEffect. Quando você envolve uma função com o useCallback, o React armazena essa função em memória e a retorna sempre que o componente for renderizado novamente, a menos que suas dependências mudem. Isso evita que a função seja recriada em cada renderização, o que pode causar re-renderizações desnecessárias de componentes filhos. Vamos explorar o useCallback em detalhes e fornecer exemplos de situações em que ele é útil.

### Sintaxe do useCallback

O useCallback tem a seguinte sintaxe:

```jsx
const memoizedCallback = useCallback(callback, dependencies);
```

- `callback`: A função que você deseja memoizar.
- `dependencies`: Um array de dependências que determina quando o useCallback deve recalcular a função. Se alguma das dependências mudar entre renderizações, o useCallback recalculará a função; caso contrário, ele retornará a função memoizada anterior.

### Exemplos de Uso

Aqui estão alguns exemplos de situações em que o useCallback é útil:

#### Passando Funções para Componentes Filhos

Quando você passa funções como props para componentes filhos, é importante usar `useCallback` para evitar a recriação dessas funções a cada renderização do componente pai. Isso é especialmente relevante quando as funções passadas como `props` são usadas em componentes filhos que dependem de otimizações de memoização.

```jsx
import React, { useCallback } from 'react';
import ChildComponent from './ChildComponent';

function ParentComponent() {
  const handleClick = useCallback(() => {
    // Lógica da função
  }, []); // Nenhuma dependência, função é memoizada permanentemente

  return <ChildComponent onClick={handleClick} />;
}
```

#### Evitando Re-renders em `useEffect`

No `useEffect`, se você usar uma função como dependência e ela for recriada a cada renderização, isso pode levar a efeitos colaterais indesejados. O `useCallback` pode ser usado para memoizar essa função e garantir que o efeito seja executado apenas quando as dependências mudarem.

```jsx
import React, { useState, useEffect, useCallback } from "react";

const Button = ({ ...props }) => {
  useEffect(() => console.log("Button Re-Render"));

  useEffect(() => console.log("Click function Re-Created"), [props.onClick]);
  return <button {...props}></button>;
};

export default function ExampleComponent() {
  const [count, setCount] = useState(0);

  useEffect(() => console.log("Component Re-render"));

  const handleClick = useCallback(() => {
    setCount((prev) => prev + 1);
  }, []);

  return (
    <div>
      <div>Contador: {count}</div>
      <Button onClick={handleClick}>Incrementar</Button>
    </div>
  );
}
```

#### Evitando Re-renderizações de Componentes

Se você tiver um componente funcional que depende de várias funções internas que não mudam entre as renderizações, pode usar `useCallback` para evitar recriar essas funções a cada renderização do componente.

```jsx
import React, { useCallback } from 'react';

function MyComponent() {
  const doSomething = useCallback(() => {
    // Lógica da função
  }, []);

  const doAnotherThing = useCallback(() => {
    // Lógica da função
  }, []);

  return (
    <div>
      {/* ... */}
    </div>
  );
}
```

O uso adequado do `useCallback` é uma prática importante para melhorar o desempenho e a eficiência de componentes React. No entanto, lembre-se de que não é necessário memoizar todas as funções; use-o quando for relevante evitar a recriação de funções que podem causar re-renderizações desnecessárias ou efeitos colaterais indesejados. Use-o com sabedoria para otimizar o desempenho do seu aplicativo React.

## `useMemo`

O `useMemo` é um hook do React que permite otimizar o desempenho de um componente, memoizando valores computados. Isso é útil quando você possui cálculos ou operações custosas que não precisam ser refeitos a cada renderização do componente. 

### Exemplo de Uso Básico

Vamos criar um exemplo simples para entender como o `useMemo` funciona. Suponha que você tenha um componente que calcula a soma de dois números e exibe o resultado. No entanto, você deseja memoizar o resultado para evitar cálculos repetidos.

```tsx
import React, { useState, useMemo } from 'react';

const Calculator: React.FC = () => {
  const [number1, setNumber1] = useState(0);
  const [number2, setNumber2] = useState(0);

  // Use useMemo para memoizar o resultado da soma
  const sum = useMemo(() => {
    console.log('Calculating sum...');
    return number1 + number2;
  }, [number1, number2]);

  return (
    <>
      <input
        type="number"
        value={number1}
        onChange={(e) => setNumber1(Number(e.target.value))}
      />
      <input
        type="number"
        value={number2}
        onChange={(e) => setNumber2(Number(e.target.value))}
      />
      <p>Soma: {sum}</p>
    </>
  );
};

export default Calculator;
```

Neste exemplo, usamos `useMemo` para calcular a soma apenas quando `number1` ou `number2` mudam. Isso evita que a soma seja recalculada a cada renderização do componente, economizando recursos.

### Exemplo de Uso em um Projeto Real

Vamos agora considerar um exemplo mais prático em um projeto real. Suponha que você tenha uma lista de itens e deseja calcular a média dos valores de uma propriedade desses itens. O useMemo pode ser útil aqui para evitar cálculos desnecessários.

```tsx
import React, { useMemo } from 'react';

interface Item {
  id: number;
  name: string;
  value: number;
}

const ItemList: React.FC<{ items: Item[] }> = ({ items }) => {
  // Use useMemo para calcular a média dos valores dos itens
  const averageValue = useMemo(() => {
    const totalValue = items.reduce((sum, item) => sum + item.value, 0);
    return totalValue / items.length;
  }, [items]);

  return (
    <div>
      <h2>Lista de Itens</h2>
      <ul>
        {items.map((item) => (
          <li key={item.id}>{item.name}: R${item.value.toFixed(2)}</li>
        ))}
      </ul>
      <p>Média de Valores: R${averageValue.toFixed(2)}</p>
    </div>
  );
};

export default ItemList;
```

Neste exemplo, o `averageValue` é calculado apenas quando a lista de itens muda, economizando ciclos de CPU desnecessários em renderizações subsequentes.

Agora você pode usar esses componentes em seu aplicativo e observar o comportamento do `useMemo`. Sempre que os valores dependentes mudarem, o cálculo será refeito. Você pode verificar isso abrindo as ferramentas de desenvolvedor do seu navegador e observando as mensagens de log no console.

Lembre-se de que o `useMemo` é uma ferramenta poderosa para otimização de desempenho, mas também pode ser mal utilizada. Use-o apenas quando tiver certeza de que está otimizando cálculos custosos, pois o uso indevido pode tornar o código mais complexo sem benefícios significativos.

## Docs

- <https://react.dev/reference/react/useCallback>
- <https://react.dev/reference/react/useMemo>
- <https://www.w3schools.com/react/react_usememo.asp>
- [Como Usar o Hook `useCallback` do React](https://kinsta.com/pt/blog/usecallback-react/)
- [`React.useMemo` na prática](https://medium.com/reactbrasil/react-usememo-na-pr%C3%A1tica-692110771c01)
- <https://www.linkedin.com/pulse/usecallback-e-usememo-quando-usar-luiz-henrique?originalSubdomain=pt>
