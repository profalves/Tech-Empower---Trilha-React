# Intergação do Redux com NestJS

Agora vou guiá-lo através deste passo a passo para criar um carrinho de compras com `Next.js`, `Redux` e `TypeScript`. Antes de começarmos, assegure-se de ter o `Node.js` instalado em seu sistema. Vamos começar:

## Etapa 1: Configuração do Projeto

Certifique-se de criar um novo projeto `Next.js` com `TypeScript` e instale as dependências necessárias.

Instale as dependências necessárias (`Redux` e outras libs que vamos utilizar):

```bash
yarn add redux react-redux @reduxjs/toolkit
```

## Etapa 2: Configurando o Redux

Crie um diretório chamado `store` dentro da pasta `src` para armazenar os arquivos relacionados ao `Redux`.

Dentro do diretório `store`, crie um arquivo `types.ts` para definir os tipos da sua aplicação:

```typescript
// src/store/types.ts
export interface Product {
  id: number;
  name: string;
  price: number;
  quantity: number;
}

export interface RootState {
  cart: Product[];
}

export const ADD_TO_CART = 'ADD_TO_CART';
export const REMOVE_FROM_CART = 'REMOVE_FROM_CART';
```

Crie um arquivo `actions.ts` dentro do diretório `store` para definir as ações do `Redux`:

```typescript
// src/store/actions.ts
import { ADD_TO_CART, REMOVE_FROM_CART, Product } from './types';

export const addToCart = (product: Product) => ({
  type: ADD_TO_CART,
  payload: product,
});

export const removeFromCart = (productId: number) => ({
  type: REMOVE_FROM_CART,
  payload: productId,
});

export const incrementQuantity = (productId: number) => ({
  type: INCREMENT_QUANTITY,
  payload: productId,
});

export const decrementQuantity = (productId: number) => ({
  type: DECREMENT_QUANTITY,
  payload: productId,
});
```

Crie um arquivo `reducers.ts` dentro do diretório store para definir os `reducers`:

```typescript
// src/store/reducers.ts
import { ADD_TO_CART, REMOVE_FROM_CART, Product } from './types';

const initialState: Product[] = [];

const cartReducer = (state = initialState, action: any) => {
  switch (action.type) {
    case ADD_TO_CART:
      return [...state, action.payload];

    case REMOVE_FROM_CART:
      return state.filter((product) => product.id !== action.payload);

    case INCREMENT_QUANTITY:
      return state.map((product) =>
        product.id === action.payload
          ? { ...product, quantity: product.quantity + 1 }
          : product
      );

    case DECREMENT_QUANTITY:
      return state.map((product) =>
        product.id === action.payload && product.quantity > 1
          ? { ...product, quantity: product.quantity - 1 }
          : product
      );

    default:
      return state;
  }
};

export default cartReducer;
```

Crie um arquivo `index.ts` dentro do diretório `store` para configurar o store do `Redux`:

```typescript
// src/store/index.ts
import { configureStore } from '@reduxjs/toolkit';
import cartReducer from './reducers';

const store = configureStore({
  reducer: {
    cart: cartReducer,
  },
});

export default store;
```

Configure o `Redux Provider` no arquivo _app.tsx:

```tsx
// src/pages/_app.tsx
import { Provider } from 'react-redux';
import store from '../store/store';
import '../styles/globals.css';

function MyApp({ Component, pageProps }) {
  return (
    <Provider store={store}>
      <Component {...pageProps} />
    </Provider>
  );
}

export default MyApp;
```

## Etapa 3: Integração com Fake API

Crie um arquivo `products.ts` para fazer chamadas à **API DummyJSON**:

```typescript
// src/api/products.ts
import axios from 'axios';

const API_BASE_URL = 'URL_DA_API';

export const getProducts = async () => {
  try {
    const response = await axios.get(`${API_BASE_URL}/products`);
    return response.data;
  } catch (error) {
    console.error('Error fetching products:', error);
    return [];
  }
};
```

Substitua `'URL_DA_API'` pela **URL** real da **API**. Pode-se usar um arquivo `ENV` para manipular esta informação com mais facilidade.

## Etapa 4: Páginas e Componentes

Crie um componente `ProductList` para exibir a lista de produtos e o botão **"Adicionar ao Carrinho"**:

```tsx
// src/components/ProductList.tsx
import { FC } from 'react';
import { useDispatch } from 'react-redux';
import { Product } from '../store/types';
import { addToCart } from '../store/actions';

interface ProductListProps {
  products: Product[];
}

const ProductList: FC<ProductListProps> = ({ products }) => {
  const dispatch = useDispatch();

  const handleAddToCart = (product: Product) => {
    dispatch(addToCart(product));
  };

  return (
    <div>
      {products.map((product) => (
        <div key={product.id}>
          <h3>{product.name}</h3>
          <p>Price: ${product.price}</p>
          <button onClick={() => handleAddToCart(product)}>Add to Cart</button>
        </div>
      ))}
    </div>
  );
};

export default ProductList;
```

Crie uma página `index.tsx` para exibir a lista de produtos na tela inicial:

```tsx
// src/pages/index.tsx
import { NextPage } from 'next';
import ProductList from '../components/ProductList';
import { getProducts } from '../api/products';

const Home: NextPage = ({ products }) => {
  return (
    <div>
      <ProductList products={products} />
    </div>
  );
};

Home.getInitialProps = async () => {
  const products = await getProducts();
  return { products };
};

export default Home;
```

Crie uma página `cart.tsx` para exibir o carrinho de compras:

```tsx
// src/pages/cart.tsx
import { NextPage } from 'next';
import { useSelector, useDispatch } from 'react-redux';
import { RootState, Product } from '../store/types';
import { removeFromCart } from '../store/actions';

const Cart: NextPage = () => {
  const cartItems = useSelector<RootState, Product[]>((state) => state.cart);
  const dispatch = useDispatch();

  const handleRemoveFromCart = (productId: number) => {
    dispatch(removeFromCart(productId));
  };

  const handleIncrementQuantity = (productId: number) => {
    dispatch(incrementQuantity(productId));
  };

  const handleDecrementQuantity = (productId: number) => {
    dispatch(decrementQuantity(productId));
  };

  return (
    <div>
      <h1>Shopping Cart</h1>
      <ul>
        {cartItems.map((product) => (
          <li key={product.id}>
            {product.name} - ${product.price} - Quantity: {product.quantity}
            <button onClick={() => handleRemoveFromCart(product.id)}>Remove</button>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default Cart;
```

Agora você tem uma aplicação `Next.js` com Redux e `TypeScript` que permite aos usuários adicionar produtos ao carrinho de compras a partir de uma lista de produtos fornecida pela **API**. Lembre-se de configurar corretamente a **URL** da **API** e implementar as funcionalidades adicionais, como a capacidade de aumentar a quantidade de produtos no carrinho.