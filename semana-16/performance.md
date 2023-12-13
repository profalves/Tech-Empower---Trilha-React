# Melhorando a performance 

## useCallback X useEffect

```tsx
// PLP produtos por categoria
// pages/categories/[category].tsx
import { useRouter } from "next/router";
import React, { useCallback, useEffect, useState } from "react";
import "./styles.css";
import { Product } from "@/models/products";
import ProductList from "@/components/ProductList";

export default function Page() {
  const { query } = useRouter();
  const [products, setProducts] = useState<Product[]>([]);

  const getProducts = useCallback( // O uso de useCallback evita que a função seja recriada a cada renderização
    async (controller: AbortController) => {
      const url = `https://dummyjson.com/products/category/${query.category}`;

      try {
        const response = await fetch(url, { signal: controller.signal });
        const data = await response.json();

        setProducts(data.products);
      } catch {
        throw new Error("Error: Products not found, try again");
      }
    },
    []
  );

  useEffect(() => {
    const controller = new AbortController(); // Classe nativa que invoca o desligamento de requisições e não pesando na performace

    getProducts(controller); // Apesar de ser chamada no aqui no useEffect, ela não será recriada a cada renderização

    return () => {
      controller.abort; // assim que saimos do componente as requests serão finalizadas mesmo que já tenham reotnado os dados
    };
  }, [query.category]);

  return (
    <div className="container">
      <h1>{query.category}</h1>;
      <ProductList products={products} />
    </div>
  );
}
```

## Lazy Loading de Componentes

O lazy loading permite carregar componentes somente quando são necessários, reduzindo o tempo de carregamento inicial da página. Com o Next.js, você pode usar a função `dynamic` para implementar o lazy loading.

```tsx
import dynamic from 'next/dynamic';

const LazyComponent = dynamic(() => import('../components/SeuComponente'), {
  loading: () => <p>Carregando...</p>,
  ssr: false, // Se você não precisar do SSR para este componente
});

const SuaPagina = () => {
  return (
    <div>
      <h1>Sua Página</h1>
      <LazyComponent />
    </div>
  );
};

export default SuaPagina;
```

## Dynamic Imports

O dynamic import é usado para carregar módulos JavaScript sob demanda. Isso é especialmente útil para dividir seu código em pedaços menores que são carregados apenas quando necessários.

```tsx
import dynamic from 'next/dynamic';

const DynamicComponent = dynamic(() => import('../components/SeuComponente'));

const SuaPagina = () => {
  return (
    <div>
      <h1>Sua Página</h1>
      <DynamicComponent />
    </div>
  );
};

export default SuaPagina;
```

## Prefetching

O Next.js também oferece `prefetching`, que permite carregar recursos de forma preemptiva, melhorando a experiência do usuário ao antecipar o carregamento de recursos que podem ser necessários. Em geral, é uma técnica que permite ao navegador carregar recursos (como scripts, estilos ou páginas) antes mesmo de serem necessários.

### Prefetching Links

Você pode adicionar `prefetch` em links usando o componente Link do Next.js:

```tsx
import Link from 'next/link';

const SuaPagina = () => {
  return (
    <div>
      <h1>Sua Página</h1>
      <Link href="/outra-pagina" prefetch>
        Ir para Outra Página
      </Link>
    </div>
  );
};

export default SuaPagina;
```

> O uso do atributo `prefetch` no componente Link instrui o Next.js a realizar o carregamento de recursos na página vinculada quando a página atual for carregada.

### Prefetching de Rotas Programaticamente

Você também pode realizar prefetching de rotas programaticamente usando `router.prefetch`, para prefetch os dados necessários para uma página antes mesmo de ela ser acessada.

```tsx
import { useRouter } from 'next/router';

const SuaPagina = () => {
  const router = useRouter();

  const handleClick = () => {
    router.prefetch('/outra-pagina');
  };

  return (
    <div>
      <h1>Sua Página</h1>
      <button onClick={handleClick}>Prefetch Outra Página</button>
    </div>
  );
};

export default SuaPagina;
```

Essas técnicas ajudarão a otimizar o carregamento de sua aplicação Next.js, reduzindo o tempo de carregamento inicial e melhorando a experiência do usuário. Experimente essas abordagens em sua aplicação e acompanhe as métricas de desempenho para ver os benefícios!

Isso deve ajudar a implementar o carregamento rápido usando técnicas como lazy loading e prefetching no seu aplicativo Next.js. Experimente e ajuste de acordo com as necessidades específicas do seu projeto!
