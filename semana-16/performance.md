# Melhorando a performance: useCallback X useEffect

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

  const getProducts = useCallback(
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
    [query.category]
  );

  useEffect(() => {
    const controller = new AbortController();

    getProducts(controller);

    return () => {
      controller.abort;
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