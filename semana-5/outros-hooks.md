# Outros React Hooks 

## `useReducer`

O hook `useReducer` é uma alternativa ao `useState` que é frequentemente usado em aplicações React para gerenciar o estado. Enquanto o `useState` **é adequado para gerenciar estados simples**, o `useReducer` **é mais apropriado quando o estado é complexo e requer lógica mais avançada para atualizá-lo**. Ele é especialmente útil quando você tem um estado que depende de ações e transições de estados específicas.

O `useReducer` é um hook do React que permite gerenciar estados complexos em componentes de forma mais organizada e previsível. Ele segue o padrão de design conhecido como ***"Reducer"*** que é amplamente utilizado em gerenciamento de estado em aplicações JavaScript, especialmente em aplicações que seguem o padrão **Flux/Redux**.

O `useReducer` recebe dois argumentos principais:

1. **Reducer Function**: Esta é uma função pura que define como o estado deve ser atualizado em resposta a uma ação. Ela recebe dois argumentos, o estado atual e uma ação, e retorna o novo estado. A ação geralmente é um objeto com um tipo (uma string que descreve a ação) e opcionalmente outros dados relevantes para a ação.

2. **Initial State**: Este é o estado inicial que você deseja para o seu componente.

O `useReducer` retorna um par de valores:

* O primeiro valor é o `state` atual.
* O segundo valor é uma função chamada de `dispatch` que permite enviar ações para o Reducer.

O Reducer é responsável por decidir como atualizar o estado com base nas ações recebidas. É importante notar que o Reducer deve ser uma função pura, ou seja, ele não deve realizar efeitos colaterais, como chamadas de API ou manipulações diretas no DOM.

Aqui está um exemplo do código da prática da aula anterior, de como você pode substituir o `useState` pelo `useReducer`:

```tsx
import React, { createContext, useContext, useReducer, ReactNode } from "react";

// Defina o tipo do estado
type Todo = {
  id: number;
  text: string;
  completed: boolean;
};

type State = {
  todos: Todo[];
};

// Defina as ações
type Action =
  | { type: "ADD_TODO"; text: string }
  | { type: "TOGGLE_TODO"; id: number }
  | { type: "REMOVE_TODO"; id: number };

// Função Reducer
const todoReducer = (state: State, action: Action): State => {
  switch (action.type) {
    case "ADD_TODO":
      return {
        ...state,
        todos: [
          ...state.todos,
          { id: state.todos.length + 1, text: action.text, completed: false },
        ],
      };
    case "TOGGLE_TODO":
      return {
        ...state,
        todos: state.todos.map((todo) =>
          todo.id === action.id ? { ...todo, completed: !todo.completed } : todo
        ),
      };
    case "REMOVE_TODO":
      return {
        ...state,
        todos: state.todos.filter((todo) => todo.id !== action.id),
      };
    default:
      return state;
  }
};

// Crie uma interface para representar o objeto de contexto
export interface TodoContextType {
  state: State;
  dispatch: React.Dispatch<Action>;
}

// Crie o contexto
const TodoContext = createContext<TodoContextType | undefined>(undefined);

// Crie o provedor do contexto
const TodoProvider = ({ children }: { children: ReactNode }) => {
  const [state, dispatch] = useReducer(todoReducer, { todos: [] });

  return (
    <TodoContext.Provider value={{ state, dispatch }}>
      {children}
    </TodoContext.Provider>
  );
};

export { TodoProvider, TodoContext };
```

Neste exemplo, criamos um Reducer `todoReducer` que lida com três tipos de ações: `ADD_TODO`, `TOGGLE_TODO`, e `REMOVE_TODO`. Em vez de usar `setState` diretamente, usamos `useReducer` para gerenciar o estado do componente. O estado inicial é definido como `{ todos: [] }`, e as ações são despachadas usando a função `dispatch`. O Reducer cuida de atualizar o estado de acordo com as ações especificadas.

Agora, você pode acessar o `state` e a função `dispatch` do contexto `TodoContext` para adicionar, alternar entre concluída/não-concluída e remover tarefas em seus componentes React.

Para uma explicação mais detalhada sobre o padrão Reducer e a Arquitetura Flux/Redux, leia [aqui](./complementos/reducer-flux-redux.md)

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
import React, { useState, useEffect, useCallback } from 'react';

function ExampleComponent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount(count + 1);
  }, [count]);

  useEffect(() => {
    // Este efeito só será reexecutado quando 'count' mudar
    console.log('Efeito executado');
  }, [count]);

  return (
    <div>
      <button onClick={handleClick}>Incrementar</button>
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