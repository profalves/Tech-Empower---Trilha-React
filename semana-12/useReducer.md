# Gerenciamento de Estado com `useReducer` no React

Agora vamos aprender como usar o hook `useReducer` para gerenciar o estado local de um carrinho de compras em uma aplicação React. Vamos criar um componente de produto que permita adicionar, remover e alterar quantidades de itens no carrinho de compras usando o `useReducer`.

## O que é `useReducer` no React?

`useReducer` é um dos hooks introduzidos no React para gerenciar o estado de um componente. Ele é uma alternativa ao `useState` e é mais adequado para casos em que o estado do componente envolve lógica complexa ou múltiplas sub-ações. O `useReducer` segue o mesmo padrão de design que Redux, permitindo que você gerencie o estado do componente de forma mais previsível.

## Estrutura Básica do useReducer

O `useReducer` recebe dois argumentos:

- **Reducer Function**: Uma função que recebe o estado atual (`state`) e uma ação (`action`) como argumentos e retorna um novo estado. O reducer especifica como o estado deve ser atualizado com base na ação.

- **Initial State**: O estado inicial do componente. Pode ser qualquer valor, semelhante ao que você passaria para o `useState`.

```javascript
const [state, dispatch] = useReducer(reducer, initialState);
```

> `state`: Representa o estado atual do componente.

> `dispatch`: Uma função que você chama para despachar uma ação e assim alterar o estado usando o reducer.

## Como Funciona o Exemplo com Carrinho de Compras

Agora, usaremos `useReducer` para gerenciar o estado local do carrinho de compras de um componente de produto. Vamos examinar como isso funciona em detalhes:

### 1. Inicialização do `useReducer`

```typescript
const [state, dispatch] = useReducer(cartReducer, { items: [] } as CartState);
```

> `cartReducer`: O reducer que definimos para gerenciar o estado do carrinho de compras.
>
> `{ items: [] }`: O estado inicial do carrinho de compras contendo uma matriz vazia de itens.

### 2. Despachando Ações para Alterar o Estado

Quando o usuário interage com o componente (por exemplo, adicionando um item ao carrinho), chamamos a função `dispatch` com uma ação apropriada:

- Adicionar Item ao Carrinho:

```typescript
const handleAddToCart = (product: CartItem) => {
  dispatch({ type: 'ADD_TO_CART', payload: { product } });
};
```

> Aqui, dispatch envia uma ação com o tipo `'ADD_TO_CART'` e um payload contendo o produto que queremos adicionar ao carrinho.

- Remover Item do Carrinho:
  
```typescript
const handleRemoveFromCart = (id: string) => {
  dispatch({ type: 'REMOVE_FROM_CART', payload: { productId: id } });
};
```

> Similar ao exemplo anterior, mas com o tipo `'REMOVE_FROM_CART'` para indicar que queremos remover um item.

- Atualizar Quantidade do Item no Carrinho:
  
```typescript
const handleQuantityChange = (event: React.ChangeEvent<HTMLInputElement>, id: string) => {
  const quantity = parseInt(event.target.value, 10);
  dispatch({ type: 'UPDATE_QUANTITY', payload: { productId: id, quantity } });
};
```

> Neste caso, dispatch é usado com o tipo `'UPDATE_QUANTITY'` e um payload que inclui o ID do produto e a nova quantidade.

### 3. Reducer para Gerenciar as Ações
O reducer cartReducer recebe a ação despachada e atualiza o estado com base no tipo de ação:

```typescript
const cartReducer = (state: CartState, action: CartAction): CartState => {
  switch (action.type) {
    case 'ADD_TO_CART':
      // Lógica para adicionar item ao carrinho
    case 'REMOVE_FROM_CART':
      // Lógica para remover item do carrinho
    case 'UPDATE_QUANTITY':
      // Lógica para atualizar a quantidade do item no carrinho
    default:
      return state;
  }
};
```

> Dentro do `reducer`, implementamos a lógica para ***adicionar***, ***remover*** ou ***atualizar*** a quantidade do item no carrinho com base no tipo de ação.

## Mão na Massa: Carrinho de Compras

### Passo 1: Criar um Reducer para o Carrinho de Compras

Começamos criando um reducer para gerenciar o estado do carrinho de compras.

```typescript
// reducer.ts
type CartItem = {
  productId: number;
  quantity: number;
};

type CartState = {
  items: CartItem[];
};

type CartAction =
  | { type: 'ADD_TO_CART'; payload: { productId: number } }
  | { type: 'REMOVE_FROM_CART'; payload: { productId: number } }
  | { type: 'UPDATE_QUANTITY'; payload: { productId: number; quantity: number } };

const cartReducer = (state: CartState, action: CartAction): CartState => {
  switch (action.type) {
    case 'ADD_TO_CART':
      // Adicionar item ao carrinho
      // ...
    case 'REMOVE_FROM_CART':
      // Remover item do carrinho
      // ...
    case 'UPDATE_QUANTITY':
      // Atualizar a quantidade do item no carrinho
      // ...
    default:
      return state;
  }
};

export default cartReducer;
```

### Passo 2: Criar o Componente de Produto com `useReducer`

Agora, vamos criar o componente de produto que utiliza o useReducer para gerenciar o estado local do carrinho de compras.

```typescript
// components/Product.tsx
import React, { useReducer } from 'react';
import cartReducer from './reducer';

type ProductProps = {
  id: number;
  name: string;
  price: number;
};

const Product: React.FC<ProductProps> = ({ id, name, price }) => {
  const [state, dispatch] = useReducer(cartReducer, { items: [] });

  const handleAddToCart = (product: CartItem) => {
    dispatch({ type: 'ADD_TO_CART', payload: { product } });
  };

  const handleRemoveFromCart = (id: string) => {
    dispatch({ type: 'REMOVE_FROM_CART', payload: { productId: id } });
  };

  const handleQuantityChange = (event: React.ChangeEvent<HTMLInputElement>, id: string) => {
    const quantity = parseInt(event.target.value, 10);
    dispatch({ type: 'UPDATE_QUANTITY', payload: { productId: id, quantity } });
  };

  return (
    <div>
      <h2>{name}</h2>
      <p>Price: ${price}</p>
      <button onClick={handleAddToCart}>Add to Cart</button>
      <button onClick={handleRemoveFromCart}>Remove from Cart</button>
      <input type="number" value={1} onChange={handleQuantityChange} />
    </div>
  );
};

export default Product;
```

## Conclusão

Aprendemos como usar o hook `useReducer` para gerenciar o estado local de um carrinho de compras em um componente React. Utilizamos a função `useReducer` para criar um estado local para o componente de produto e definimos a lógica de ações no reducer cartReducer. Dessa forma, conseguimos adicionar, remover e alterar quantidades de itens no carrinho de compras de forma eficaz e organizada.

Espero que este tutorial seja útil para entender como usar o useReducer no React! Se você tiver mais perguntas ou precisar de mais ajuda, sinta-se à vontade para perguntar.