# Requisições HTTP com Next.js e TypeScript

## Introdução

As **requisições HTTP** são uma parte essencial no desenvolvimento de aplicações web modernas. Elas permitem que as aplicações comuniquem-se com servidores para obter ou enviar dados dinamicamente, possibilitando experiências interativas e dinâmicas para os usuários. Em aplicações React, o Next.js é uma escolha popular devido à sua facilidade de integração com APIs e sua tipagem usando TypeScript.

Então, vamos explorar como fazer requisições HTTP em aplicações **React** usando **Next.js** e **TypeScript**. Vamos começar com uma breve explicação sobre requisições HTTP, antes de nos aprofundarmos em exemplos práticos com Next.js.

## Requisições HTTP: Uma Visão Geral

As requisições HTTP (_Hypertext Transfer Protocol_) são um conjunto de regras para a transferência de dados na web. Elas são usadas para buscar recursos (`GET`), enviar dados para serem processados (`POST`), atualizar dados (`PUT/PATCH`), ou excluir dados (`DELETE`) em um servidor. Em aplicações React, geralmente usamos requisições `GET` para buscar dados de uma API.

## Fazendo Requisições HTTP em Aplicações `Next.js` com `TypeScript`

## 1. Requisições com fetch API

Vamos começar com um exemplo simples de como fazer uma requisição `GET` usando a função `fetch`. Crie um novo arquivo chamado `api.ts`:

```typescript
// api.ts
export async function fetchData() {
  const response = await fetch('https://api.example.com/data');
  const data = await response.json();
  return data;
}
```

A função `fetchData` faz uma requisição para <https://api.example.com/data> e retorna os dados obtidos.

## 2. Criando um Hook Customizado

Agora, crie um hook customizado para gerenciar as requisições em `useApi.ts`:

```typescript
// useApi.ts
import { useState, useEffect } from 'react';

export function useApi() {
  const [data, setData] = useState<any>(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch('https://api.example.com/data');
        const result = await response.json();
        setData(result);
      } catch (error) {
        console.error('Erro ao buscar dados:', error);
      }
    };

    fetchData();
  }, []);

  return data;
}
```

## 3. Utilizando o Hook no Componente

Agora, utilize o hook no seu componente Next.js para exibir os dados:

```typescript
Copy code
// pages/index.tsx
import React from 'react';
import { useApi } from '../hooks/useApi';

const Home: React.FC = () => {
  const data = useApi();

  if (!data) {
    return <div>Carregando...</div>;
  }

  return (
    <div>
      <h1>Dados da API</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
};

export default Home;
```

Neste exemplo, o componente `Home` utiliza o hook `useApi` para fazer uma requisição à API e exibir os dados. Se os dados ainda estiverem sendo carregados, exibe _"Carregando..."_. Quando os dados estiverem disponíveis, eles serão exibidos na página.

## Conclusão

Exploramos o básico das requisições HTTP em aplicações React usando Next.js e TypeScript. A função `fetch` é uma forma padrão e poderosa de fazer requisições, e usando hooks personalizados, podemos organizar nosso código de forma eficiente.

Lembre-se sempre de tratar erros adequadamente em suas requisições, pois problemas de rede ou falhas do servidor podem ocorrer. Além disso, ajuste as URLs e a estrutura do código conforme necessário para se adequar ao seu projeto específico.

## Pratica

```tsx
import { useState, useEffect } from "react";
import { useRouter } from "next/router";
import { Product } from "@/models/products";

export default function useProducts() {
  const [products, setProducts] = useState<Product[]>([]);
  const [selectedProducts, setSelectedProducts] = useState<Product | null>(
    null
  );
  const { query } = useRouter();

  const getProducts = async () => {
    try {
      const response = await fetch(
        `https://dummyjson.com/products?limit=8&skip=2`
      );
      const data = await response.json();

      setProducts(data.products);
    } catch (error) {
      throw new Error(JSON.stringify(error));
    }
  };

  const getProductById = async (id: number) => {
    try {
      const response = await fetch(`https://dummyjson.com/products/${id}`);
      const data = await response.json();

      setSelectedProducts(data);
    } catch (error) {
      throw new Error(JSON.stringify(error));
    }
  };

  useEffect(() => {
    getProducts();

    if (query.id) {
      const productId = Number(query.id);
      getProductById(productId);
    }
  }, [query.id]);

  return { products, selectedProducts };
}

```
E para o uso de imagens no Next.js:

1. Configuração
![image](https://github.com/profalves/Tech-Empower---Trilha-React/assets/2893710/c119c1e5-4142-4377-abb5-d9adfb664ac7)

2. Uso
```tsx
<Image
  src={product?.images[0]}
  alt={product?.description}
  width={400}
  height={400}
  className="img-main"
/>
```

