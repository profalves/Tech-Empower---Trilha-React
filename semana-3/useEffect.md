# `useEffect` hook e Ciclo de Vida dos Componentes

O `useEffect` é uma poderosa ferramenta para lidar com efeitos colaterais em componentes funcionais no React e é amplamente utilizado para gerenciar o comportamento de componentes que precisam interagir com o ambiente fora do mundo do `React`.

**O ciclo de vida de um componente React** é uma parte fundamental para entender como os componentes funcionam. Antes do React 16.3, os componentes tinham vários métodos para gerenciar o seu ciclo de vida: 

- **Montagem (Mounting)**: usando os métodos `constructor()`, `render()` e o `componentDidMount()`. 
- **Atualização (Updating)**: usando os métodos `componentDidUpdate(prevProps, prevState)` e `shouldComponentUpdate(nextProps, nextState)`
- **Desmontagem (Unmounting)**: `componentWillUnmount()`

No entanto, com a introdução do **useEffect** e dos componentes funcionais, o ciclo de vida foi simplificado e veremos como é mais adiante. É importante observar que, com a introdução de componentes funcionais e hooks no React, a recomendação é usar componentes funcionais sempre que possível, pois eles oferecem uma sintaxe mais simples e legível. Os hooks, como `useState` e `useEffect`, foram projetados para simplificar o gerenciamento de estado e efeitos colaterais em componentes funcionais, eliminando a necessidade de classes e métodos de ciclo de vida. 

## `useEffect`

O ***useEffect*** é um hook fundamental no React que **permite adicionar efeitos colaterais a componentes funcionais**. Efeitos colaterais são operações que não fazem parte da renderização de um componente, como manipulação do DOM, requisições para APIs, assinaturas de eventos, entre outros. O `useEffect` é uma maneira de lidar com essas operações fora do fluxo principal de renderização do React.

### Sintaxe Básica do `useEffect`:

```jsx
import React, { useState, useEffect } from 'react';

function MeuComponente() {
  // useState é usado para gerenciar o estado do componente
  const [estado, setEstado] = useState(valorInicial);

  // useEffect é usado para efeitos colaterais
  useEffect(() => {
    // Código para o efeito colateral vai aqui
  }, [dependencias]);
  
  return (
    // JSX do componente
  );
}
```

### Parâmetros do `useEffect`:

1. **Função de Efeito**: A função passada como o primeiro argumento de `useEffect` é a função que será executada quando o efeito for ativado, ou seja, quando o ciclo de vida muda de etapa. Este é o lugar onde você coloca seu código para efeitos colaterais.

2. **Array de Dependências**: O segundo argumento é uma matriz de dependências que determina quando o efeito deve ser executado. O React compara as dependências atuais com as dependências anteriores para decidir se o efeito deve ser executado novamente.
   - **Se o array de dependências estiver vazio (`[]`)**, o efeito será executado apenas após a montagem inicial do componente.
    ```jsx
    useEffect(() => {
      // Este efeito será executado após a montagem inicial do componente
      // Coloque aqui o código para efeitos colaterais iniciais
    }, []);
    ```
   - **Se você omitir o segundo argumento**, o efeito será executado após cada renderização do componente. **Devemos ser extremamente cuidadosos em omitir as dependencias para que o componente não execute várias renderizações desnecessárias**.
    ```jsx
    useEffect(() => {
      // Este efeito será executado após cada renderização
      // Coloque aqui o código para efeitos colaterais que dependem da 'dependencia'
    });
    ```
   - **Se você fornecer um array de dependências com valores**, o efeito será executado apenas quando uma ou mais dessas dependências mudarem.
    ```jsx
    useEffect(() => {
      // Este efeito será executado quando 'dependencia' mudar
      // Coloque aqui o código para efeitos colaterais que dependem da 'dependencia'
    }, [dependencia]);
    ```

## Ciclo de vida de um componente React usando `useEffect`

Vou explicar o ciclo de vida de um componente React usando useEffect e detalhando este hook. Primeiro, vamos criar um componente funcional e depois explicar as diferentes fases do ciclo de vida.

```jsx
import React, { useState, useEffect } from 'react';

function MeuComponente() {
  // Fase de Montagem (Mounting)

  // useState é usado para gerenciar o estado do componente
  const [contador, setContador] = useState(0);

  // useEffect é usado para lidar com efeitos colaterais
  useEffect(() => {
    // Este código será executado após a renderização do componente
    console.log('Componente montado');

    // Fase de Desmontagem (Unmounting)
    return () => {
      console.log('Componente desmontado');
    };
  }, []); // A matriz de dependências está vazia, o que significa que o efeito será executado apenas uma vez na montagem

  // Fase de Renderização (Render/Re-render)
  useEffect(() => {
    // Este código será executado após cada renderização do componente
    console.log('Componente re-renderizado');
  });
  
  // Fase de Atualização (Updating)
  useEffect(() => {
    // Este código será executado após cada renderização do componente
    console.log('Componente atualizado');
  });

  return (
    <div>
      <p>Contador: {contador}</p>
      <button onClick={() => setContador(contador + 1)}>Incrementar</button>
    </div>
  );
}

export default MeuComponente;
```

Agora, vamos detalhar as diferentes fases do ciclo de vida usando `useEffect`:

1. **Montagem (Mounting):**
   - Durante a montagem, o `React` cria uma **instância** do componente e insere no **DOM**.
   - O `useState` é usado para inicializar o estado do componente, e o `useEffect` pode ser utilizado para efeitos colaterais durante a montagem. Quando a matriz de dependências (segundo argumento) estiver vazia, o efeito será executado apenas uma vez após a montagem.
É um bom lugar para fazer solicitações de rede, configurar assinaturas, etc.
2. **Atualização (Updating):**
   - A fase de atualização ocorre sempre que o `state` ou as `props` do componente mudam.
   - O segundo `useEffect` sem um array de dependências será chamado após cada renderização do componente. É um bom lugar para gerenciar efeitos colaterais que precisam ser atualizados quando o estado ou as props mudam.
3. **Desmontagem (Unmounting):**
   - A fase de desmontagem ocorre quando o componente é removido do **DOM**, como quando você navega para outra página ou remove o componente.
   - Uma função após o `return` no `useEffect` é usado para definir a lógica de desmontagem, como cancelar assinaturas ou limpar recursos.


## `useLayoutEffect`

`useLayoutEffect` é uma versão useEffectque é acionada antes que o navegador renderizar a tela. A maneira como você os chama parece a mesma:

```jsx
useEffect(() => {
  // efeitos colaterais
  return () => /* limpeza */
}, [dependency]);

useLayoutEffect(() => {
  // efeitos colaterais
  return () => /* limpeza */
}, [dependency]);
```

### A diferença entre `useEffect` e `useLayoutEffect`

No React, tanto `useEffect` quanto `useLayoutEffect` são hooks usados para executar efeitos colaterais em componentes funcionais. Ambos são usados para lidar com tarefas como assinatura de eventos, chamadas de API, manipulação do DOM e muito mais. No entanto, eles têm uma diferença importante em relação ao momento em que são executados durante o ciclo de renderização.

Então isso se parece com:

1. Você causa uma renderização de alguma forma (altera o estado ou o componente pai é renderizado novamente)
2. React renderiza seu componente (chama seu componente)
3. A tela é visualmente atualizada
4. SÓ ENTÃO `useEffect` é executado

`useLayoutEffect`, por outro lado, é executado de forma síncrona após uma renderização, mas antes da atualização da tela. Significa:

1. Você causa uma renderização de alguma forma (altera o estado ou o componente pai é renderizado novamente)
2. React renderiza seu componente (chama seu componente)
3. `useLayoutEffect` é executado e o React aguarda seu término
4. A tela é visualmente atualizada

### Devo usar o Effect ou use o LayoutEffect?
Na maioria das vezes, `useEffect` é a escolha certa. Se o seu código estiver causando oscilações na renderização, mude para `useLayoutEffect` e veja se isso ajuda.

Como `useLayoutEffect` é síncrono (bloqueia a renderização), o aplicativo não será atualizado visualmente até que o efeito termine de executar … isso pode causar problemas de desempenho se você tiver um código lento no seu efeito. Juntamente com o fato de que a maioria dos efeitos não precisam que o mundo faça uma pausa enquanto acontecem.

A escolha entre `useEffect` e `useLayoutEffect` depende das necessidades específicas do seu componente. É importante considerar o desempenho e a sincronização das atualizações do DOM ao escolher entre eles. Geralmente, useEffect é suficiente para a maioria dos casos.

## Docs

- <https://react.dev/reference/react/useEffect>
- <https://react.dev/reference/react/useLayoutEffect>
- <https://oieduardorabelo.medium.com/react-quando-usar-uselayouteffect-ao-inv%C3%A9s-de-useeffect-9f2157396aac>