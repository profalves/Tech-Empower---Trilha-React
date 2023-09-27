# Comunicação entre componentes React

## Props

Props (propriedades) são uma maneira fundamental de passar dados de um componente pai para um componente filho no React. Além disso, também é importante saber como passar dados do componente filho de volta para o componente pai, o que é feito por meio de funções de retorno (callbacks) passadas como props. Vou explicar como fazer isso usando TypeScript.

### Passando Props de um Componente Pai para um Componente Filho

Vamos criar um exemplo simples com dois componentes: um componente pai e um componente filho. O componente pai passará algumas informações para o componente filho por meio de props.

#### Componente Pai

Primeiro, vamos criar um componente pai chamado `Pai.tsx`:

```tsx
import React from "react";
import Filho from "./Filho";

interface PaiProps {
  mensagem: string;
}

const Pai: React.FC<PaiProps> = ({ mensagem }) => {
  return (
    <div>
      <h1>Componente Pai</h1>
      <p>Mensagem para o filho: {mensagem}</p>
      <Filho mensagemDoPai={mensagem} />
    </div>
  );
};

export default Pai;
```

Neste exemplo, o componente **Pai** passa uma *propriedade* chamada `mensagem` para o componente **Filho**.

#### Componente Filho

Agora, o componente filho chamado `Filho.tsx`:

```tsx
import React from "react";

interface FilhoProps {
  mensagemDoPai: string;
}

const Filho: React.FC<FilhoProps> = ({ mensagemDoPai }) => {
  return (
    <div>
      <h2>Componente Filho</h2>
      <p>Recebido do pai: {mensagemDoPai}</p>
    </div>
  );
};

export default Filho;
```

Aqui, o componente **Filho** recebe a prop `mensagemDoPai` do componente **Pai** e a renderiza.

### Passando Dados do Componente Filho para o Componente Pai

Para passar dados do componente filho de volta para o componente pai, você pode usar funções de retorno (callbacks). Vamos estender o exemplo anterior para fazer isso.

#### Componente Pai

Modifique o componente pai `Pai.tsx` da seguinte forma:

```tsx
import React, { useState } from "react";
import Filho from "./Filho";

interface PaiProps {
  mensagem: string;
}

const Pai: React.FC<PaiProps> = ({ mensagem }) => {
  const [mensagemDoFilho, setMensagemDoFilho] = useState<string>("");

  const receberMensagemDoFilho = (mensagem: string) => {
    setMensagemDoFilho(mensagem);
  };

  return (
    <div>
      <h1>Componente Pai</h1>
      <p>Mensagem para o filho: {mensagem}</p>
      <p>Mensagem do filho: {mensagemDoFilho}</p>
      <Filho mensagemDoPai={mensagem} enviarMensagemParaPai={receberMensagemDoFilho} />
    </div>
  );
};

export default Pai;
```

Neste exemplo, o componente **Pai** agora possui um estado (`mensagemDoFilho`) para armazenar a mensagem recebida do componente **filho**. Além disso, ele passa uma função chamada `enviarMensagemParaPai` como *prop* para o componente **filho**.

### Componente Filho

Modifique o componente filho `Filho.tsx` para permitir o envio de mensagens de volta para o pai:

```tsx
import React, { useState } from "react";

interface FilhoProps {
  mensagemDoPai: string;
  enviarMensagemParaPai: (mensagem: string) => void;
}

const Filho: React.FC<FilhoProps> = ({ mensagemDoPai, enviarMensagemParaPai }) => {
  const [mensagemLocal, setMensagemLocal] = useState<string>("");

  const enviarMensagemParaPaiEAtualizar = () => {
    enviarMensagemParaPai(mensagemLocal);
    setMensagemLocal("");
  };

  return (
    <div>
      <h2>Componente Filho</h2>
      <p>Recebido do pai: {mensagemDoPai}</p>
      <input
        type="text"
        value={mensagemLocal}
        onChange={(e) => setMensagemLocal(e.target.value)}
        placeholder="Digite uma mensagem para o pai"
      />
      <button onClick={enviarMensagemParaPaiEAtualizar}>Enviar para o pai</button>
    </div>
  );
};

export default Filho;
```

Aqui, o componente Filho recebe a função enviarMensagemParaPai do componente Pai como prop. Quando o botão é clicado, ele chama essa função para enviar a mensagem de volta para o componente Pai.

Agora, você tem um exemplo completo de como passar props de um componente pai para um componente filho e como passar dados do componente filho de volta para o componente pai no React usando TypeScript.

## Mão na Massa: To-Do-List

Vamos criar um caso de uso comum onde podemos aplicar os conceitos que explicamos anteriormente. Vamos criar um aplicativo simples de lista de tarefas (To-Do List) com React e TypeScript. O aplicativo terá um componente pai que renderiza uma lista de tarefas e um componente filho para adicionar novas tarefas. Também vamos demonstrar como passar dados do componente filho de volta para o componente pai quando uma nova tarefa for adicionada.

### 1. Criando os Componentes

Crie dois componentes: `TodoList.tsx` (o componente pai) e `AddTodo.tsx` (o componente filho).

#### `TodoList.tsx` (Componente Pai)

```tsx
import React, { useState } from "react";
import Task from "./ToDo";
import AddToDo from "./AddToDo";

export default function ToDoList() {
  const [tasks, setTasks] = useState<Array<string>>([]);

  const addTodo = (newTask: string) => {
    setTasks([...tasks, newTask]);
  };

  return (
    <div>
      <h1>Lista de Tarefas</h1>
      <AddToDo addTodo={addTodo} />
      <ul>
        {tasks.map((task: string, index: number) => {
          return <Task key={index}>{task}</Task>;
        })}
      </ul>
    </div>
  );
}

```

#### `AddTodo.tsx` (Componente Filho)

```tsx
import React, { useState } from "react";

interface AddTodoProps {
  addTodo: (task: string) => void;
}

export default function AddToDo({ addTodo }: AddTodoProps) {
  const [newTask, setNewTarefa] = useState<string>("");

  const addTodoHandler = () => {
    if (newTask === "") return;
    addTodo(newTask);
  };

  return (
    <>
      <input
        type="text"
        value={newTask}
        onChange={(e) => setNewTarefa(e.target.value)}
        placeholder={"Digite a tarefa"}
      />
      <button onClick={addTodoHandler}>Adicionar Tarefa</button>
    </>
  );
}

```

#### `Todo.tsx` (Componente de Tarefa)

Crie um componente `Todo.tsx` para exibir cada tarefa individualmente. Este componente é apenas para fins de ilustração e não requer interação com os outros componentes. Aqui, estamos apenas recebendo a `text` *prop* e renderizando-a.

```tsx
import React from "react";

interface ToDoProps {
  children: React.ReactNode;
}

export default function ToDo({ children }: ToDoProps) {
  return <li>{children}</li>;
}
```

### 2. Renderizando os Componentes no `Index.tsx`

Agora, renderize os componentes da `TodoList` no arquivo `src/pages/Index.tsx`:

```tsx
import Head from "next/head";
// ...
import ToDoList from "@/components/ToDoList";

// ...

export default function Home() {
  return (
    <>
      <Head>
        ...
      </Head>
      <main className={`${styles.main} ${inter.className}`}>
        <ToDoList />
      </main>
    </>
  );
}

```

### 3. Estilos CSS (Opcional)

Você pode adicionar estilos CSS para tornar seu app mais atraente. Este exemplo é apenas o básico:

```css
.main {
  text-align: center;
}

h1 {
  font-size: 2rem;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin: 5px 0;
  padding: 5px;
  background-color: #f0f0f0;
  border-radius: 5px;
}
```

As tarefas serão exibidas na lista de tarefas e você terá implementado com sucesso a passagem de dados de um componente filho (`AddTodo`) para um componente pai (`TodoList`) e também a renderização de props entre componentes em React usando TypeScript.

## useContext Hook

O useContext é um hook do React que oferece uma forma de acessar o contexto (context) de um componente sem precisar passar dados por meio das props de componente em componente. Ele é amplamente usado para gerenciar e compartilhar informações globais em uma aplicação React, como configurações, autenticação, temas, entre outros.

Aqui estão os principais conceitos e funcionalidades do useContext:

1. **Contexto (Context)**: No React, um contexto é um objeto que armazena dados globais que podem ser compartilhados entre componentes sem a necessidade de passar esses dados manualmente por meio das props. É uma maneira de evitar a chamada "prop drilling", onde você precisa passar dados por vários níveis de componentes apenas para chegar ao componente que realmente precisa desses dados.

2. **useContext Hook**: O useContext é um hook que permite que você acesse o valor atual de um contexto. Você precisa fornecer o contexto que deseja acessar como argumento para o useContext. O hook retornará o valor atual do contexto.

3. **Provedor de Contexto (Context Provider)**: Antes de usar o useContext, você precisa criar um provedor de contexto usando o componente Context.Provider. Este provedor envolve a parte da sua árvore de componentes onde os dados globais devem estar disponíveis. Ele aceita um valor (geralmente um objeto ou qualquer outro tipo de dado) que será acessível aos componentes que usam o contexto.

4. **Consumo de Contexto**: Componentes que precisam acessar os dados do contexto podem usar o useContext para obter esses dados. Quando você usa o useContext, ele procura o contexto mais próximo na árvore de componentes e retorna o valor do contexto. Se não encontrar um provedor de contexto correspondente, ele usará o valor padrão definido no contexto.

Aqui está um exemplo simples de como usar o useContext:

```tsx
import React, { useContext } from 'react';

// Crie um contexto
const MyContext = React.createContext();

// Crie um componente que fornece o contexto
function MyContextProvider({ children }) {
  const sharedData = 'Dados Globais';

  return <MyContext.Provider value={sharedData}>{children}</MyContext.Provider>;
}

// Use o contexto em um componente filho
function ChildComponent() {
  const contextData = useContext(MyContext);

  return <div>{contextData}</div>;
}

// Use o provedor de contexto em seu aplicativo
function App() {
  return (
    <MyContextProvider>
      <ChildComponent />
    </MyContextProvider>
  );
}

export default App;
```

> Neste exemplo, `ChildComponent` usa o `useContext(MyContext)` para acessar o valor definido no contexto `MyContext`. O valor "Dados Globais" será renderizado dentro de `ChildComponent`, mesmo que não seja passado explicitamente através das *props*.

O `useContext` é especialmente útil para gerenciar estados globais, como informações de autenticação, temas da aplicação, preferências do usuário e muito mais, tornando o compartilhamento de dados entre componentes mais eficiente e legível.

Agora vamos fazer um passo a passo de como usar o `useContext` no **React** com **TypeScript** em um projeto de ***To-Do List***. O `useContext` é uma ferramenta poderosa para gerenciar o estado global em aplicações React.

### Passo 1: Criar o Contexto

Crie o contexto que irá conter o estado global do **To-Do List**. No arquivo `TodoContext.tsx`, defina o contexto da seguinte forma:

```tsx
import React, { createContext, useContext, useState } from 'react';

// Defina o tipo do estado
type Todo = {
  id: number;
  text: string;
  completed: boolean;
};

type State = {
  todos: Todo[];
};

// Crie o contexto
const TodoContext = createContext<State | undefined>(undefined);

// Crie o provedor do contexto
const TodoProvider: React.FC = ({ children }) => {
  const [todos, setTodos] = useState<Todo[]>([]);

  const addTodo = (text: string) => {
    setTodos([...todos, { id: todos.length + 1, text, completed: false }]);
  };

  const toggleTodo = (id: number) => {
    setTodos(
      todos.map((todo) =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
      )
    );
  };

  const removeTodo = (id: number) => {
    setTodos(todos.filter((todo) => todo.id !== id));
  };

  return (
    <TodoContext.Provider value={{ todos, addTodo, toggleTodo, removeTodo }}>
      {children}
    </TodoContext.Provider>
  );
};

export { TodoProvider, TodoContext };
```

### Passo 2: Criar o Componente de Lista de Tarefas (TodoList)

Agora, crie o componente que usará o contexto para acessar e atualizar a lista de tarefas. No arquivo `TodoList.tsx`, você pode fazer algo como:

```tsx
import React, { useContext, useState } from 'react';
import { TodoContext } from '../context/TodoContext';

const TodoList: React.FC = () => {
  const { todos, addTodo, toggleTodo, removeTodo } = useContext(TodoContext);
  const [newTodo, setNewTodo] = useState<string>('');

  const handleAddTodo = () => {
    if (newTodo.trim() === '') return;
    addTodo(newTodo);
    setNewTodo('');
  };

  return (
    <div>
      <h1>To-Do List</h1>
      <input
        type="text"
        placeholder="Adicionar nova tarefa"
        value={newTodo}
        onChange={(e) => setNewTodo(e.target.value)}
      />
      <button onClick={handleAddTodo}>Adicionar</button>
      <ul>
        {todos.map((todo) => (
          <li key={todo.id}>
            <span
              style={{
                textDecoration: todo.completed ? 'line-through' : 'none',
              }}
              onClick={() => toggleTodo(todo.id)}
            >
              {todo.text}
            </span>
            <button onClick={() => removeTodo(todo.id)}>Remover</button>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default TodoList;
```

### Passo 3: Integre o Contexto na Aplicação

Em `App.tsx`, envolva sua aplicação com o `TodoProvider` para que o contexto esteja disponível em toda a árvore de componentes:

```tsx
import React from 'react';
import './App.css';
import TodoList from './components/TodoList';
import { TodoProvider } from './context/TodoContext';

function App() {
  return (
    <div className="App">
      <TodoProvider>
        <TodoList />
      </TodoProvider>
    </div>
  );
}

export default App;
```

No tutorial acima, você aprendeu como usar o `useContext` no React com TypeScript para criar um sistema de To-Do List de forma simplificada. O useContext é uma ferramenta poderosa que permite compartilhar informações globais entre componentes sem a necessidade de passar dados por meio das props, tornando o código mais limpo e eficiente.

Ao criar um contexto e um provedor de contexto, você pode compartilhar dados em toda a árvore de componentes, tornando mais fácil o gerenciamento do estado global da sua aplicação. Isso é particularmente útil em cenários onde vários componentes precisam acessar e atualizar os mesmos dados.

## Docs

- <https://react.dev/learn/passing-props-to-a-component>
- <https://react.dev/learn/passing-data-deeply-with-context#context-an-alternative-to-passing-props>

## Desafio

Agora, vamos para um desafio para aplicar o conhecimento sobre o `useContext`:

> Objetivo: Criar um aplicativo React que permita alternar entre temas escuros e claros usando o useContext.

### Instruções:

* Crie um novo projeto React com TypeScript. 

* Implemente um sistema de temas com dois temas: **escuro** e **claro**. Crie um contexto para gerenciar o tema.

* Crie um componente `ThemeSwitch` que permita alternar entre os temas. Esse componente deve usar o `useContext` para acessar o contexto do tema e alternar entre os temas escuro e claro quando clicado.

* Crie um componente principal que mude seu estilo com base no tema selecionado. Por exemplo, se o tema for escuro, o aplicativo deve usar cores escuras; se o tema for claro, use cores claras.

* Certifique-se de que a troca de tema seja imediatamente refletida em toda a aplicação devido ao uso do useContext.

Este desafio permitirá que você pratique a criação e o uso de contextos com `useContext` para gerenciar temas em uma aplicação React. Além disso, você ganhará experiência em lidar com lógica de alternância de temas e estilização com base no tema selecionado. Boa sorte!

### Dicas

Vou fornecer um exemplo simplificado de como você pode criar um aplicativo com os temas **escuros** e **claros** usando `useContext`. Lembre-se de que este é um exemplo básico e você pode estender e personalizar de acordo com sua criatividade.

#### Passo 1: Criar o Contexto do Tema

Crie um novo arquivo chamado `ThemeContext.tsx` para definir o contexto do tema:

```tsx
import React, { ReactNode, createContext, useContext, useState } from "react";

type Theme = "light" | "dark"; // tipo string já forncendo valores padrão

type ThemeContextType = {
  theme: Theme; // deve conter o state do tema
  toggleTheme: () => void; // a função que irá modificar o tema
};

type ThemeProviderProps = {
  children: ReactNode;
};

const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

export const ThemeProvider = ({ children }: ThemeProviderProps) => {
  // lógica para usar o state e modifica-lo ... 

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

// sugestão:  poderá usar este hook customizado para que não seja 
// preciso chamar o useContext, e poder pegar o valor diretamente
export const useTheme = () => {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error("useTheme must be used within a ThemeProvider");
  }
  return context;
};
```

#### Passo 2: Criar Componente de Botão de Troca de Tema

Agora, crie o componente `ThemeSwitch.tsx` que usará o contexto do tema.

```tsx
import React from "react";
import { useTheme } from "@/context/ThemeContext";

const ThemeSwitch: React.FC = () => {
  // use o contexto...

  return (
    <button onClick={toggleTheme}>
      {theme === "light" ? "Switch to Dark Theme" : "Switch to Light Theme"}
    </button>
  );
};

export default ThemeSwitch;
```

### Passo 3: Estilização

Os estilos globais podem ser adicionados diretamente ao arquivo `_app.tsx`. No `Next.js`, você deve usar o arquivo chamado `_app.tsx` para importar estilos globais e envolver a aplicação de modo a realizar a mudança de cores para TODOS os componentes e componentes futuros.

```tsx
import { ThemeProvider } from "@/context/ThemeContext";
import "@/styles/globals.css";
import type { AppProps } from "next/app";

export default function App({ Component, pageProps }: AppProps) {
  return (
    <ThemeProvider> // tema adicionado
      <Component {...pageProps} />
    </ThemeProvider>
  );
}
```

Ainda precisa adicionar o css dos temas claro e escuro

```css
/* light */
.light {
  background-color: #fff;
  color: #000;
}

/* dark */
.dark {
  background-color: #000;
  color: #fff;
}

```

### Passo 4: Teste a Teoca de Tema

Certifique-se de importar o `useTheme` em `index.tsx` para que o contexto do tema seja utilizado em toda a aplicação.

```tsx
import Head from "next/head";
...
import { useTheme } from "@/context/ThemeContext";
import ThemeSwitch from "@/components/ThemeSwitch";

...

export default function Home() {
  // chama o hook customizado para o tema

  // usando styles[theme] a classe css será alterada de acordo com o que o tema indicou

  return (
    <>
      <Head>
        ...
      </Head>
      <main className={`${styles.main} ${inter.className} ${styles[theme]}`}>  
        <ThemeSwitch />
        <div>
          ...
        </div>
      </main>
    </>
  );
}
```