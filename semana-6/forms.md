# Fomulários e validações de campos no React

## `useRef` hook

O `useRef` cria um objeto de referência que persiste entre as renderizações, evitando a renderização dos componentes do formulário de forma desnecessária quando atualiza o `state`. Aqui está um exemplo de como usá-lo:

```tsx
import React, { useRef, useEffect } from 'react';

const MyComponent = () => {
  const inputRef = useRef(null);

  useEffect(() => {
    // Após o componente ser montado, o input será focado
    inputRef.current.focus();
  }, []); // O array vazio [] assegura que o useEffect será chamado apenas uma vez, após o montar do componente

  return <input type="text" ref={inputRef} />;
};
```

> Neste exemplo, `inputRef` é uma referência para o elemento input do DOM. Você pode acessar suas propriedades e métodos diretamente usando `inputRef.current`.

O `useRef` é um hook no React que retorna um objeto `ref` mutável cuja propriedade `current` é inicializada com o argumento passado (`initialValue`). O objeto retornado persiste durante todo o ciclo de vida do componente.

### Para que serve o `useRef`?

O `useRef` é utilizado principalmente para:

- **Acesso direto ao `DOM`:** Você pode usar `ref` para acessar diretamente elementos do **DOM** e interagir com eles.

- **Persistência de Valores:** O valor de `ref` não será recriado em cada renderização, tornando-o útil para manter valores que não devem causar re-render quando mudam.

- **Manter referências para valores mutáveis:** Quando você deseja manter uma variável cujo valor pode mudar sem forçar uma nova renderização.

### Outras funcionalidades

Além de focar em um elemento, existem várias outras maneiras pelas quais você pode utilizar a propriedade current de um objeto de referência (`ref`). Aqui estão alguns exemplos:

#### 1. Manter Referência a Elementos de DOM:

```tsx
import React, { useRef, useEffect } from 'react';

const MyComponent: React.FC = () => {
  const myDivRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (myDivRef.current) {
      console.log(myDivRef.current.offsetWidth); // Largura do elemento
      console.log(myDivRef.current.getBoundingClientRect()); // Informações sobre posição e tamanho
    }
  }, []);

  return <div ref={myDivRef}>Meu Elemento Div</div>;
};
```

#### 2. Executar Animações com requestAnimationFrame:

```tsx
import React, { useRef } from 'react';

const AnimatedComponent: React.FC = () => {
  const elementRef = useRef<HTMLDivElement>(null);

  const animate = () => {
    // Lógica para animação
    if (elementRef.current) {
      // Atualize a posição do elemento aqui com base na animação
    }

    requestAnimationFrame(animate);
  };

  // Iniciar a animação quando o componente for montado
  React.useEffect(() => {
    animate();
  }, []);

  return <div ref={elementRef}>Elemento Animado</div>;
};
```

#### 3. Integração com Bibliotecas de Terceiros (como `D3.js` ou `Three.js`):

```tsx
import React, { useRef, useEffect } from 'react';
import * as D3 from 'd3'; // Exemplo usando D3.js (biblioteca de visualização de dados)

const D3Chart: React.FC = () => {
  const chartContainer = useRef(null);

  useEffect(() => {
    if (chartContainer.current) {
      // Lógica para criar um gráfico usando D3.js
      const svg = D3.select(chartContainer.current)
        .append('svg')
        .attr('width', 400)
        .attr('height', 300);

      svg.append('circle').attr('cx', 50).attr('cy', 50).attr('r', 20).style('fill', 'blue');
    }
  }, []);

  return <div ref={chartContainer}></div>;
};
```

#### 4. Integração com APIs de Terceiros (Google Maps, Mapbox, etc.):
```tsx
import React, { useRef, useEffect } from 'react';

const MapComponent: React.FC = () => {
  const mapContainer = useRef(null);

  useEffect(() => {
    if (mapContainer.current) {
      // Inicialize e interaja com o mapa usando a API do Google Maps, Mapbox, etc.
    }
  }, []);

  return <div ref={mapContainer} style={{ width: '100%', height: '400px' }}></div>;
};
```

Lembre-se de que usar o `useRef` dessa maneira é útil quando você precisa manter uma referência a um elemento DOM ou a um objeto mutável que não afeta a renderização. Sempre use o `useRef` para interações com o DOM ou para manter referências a objetos que não devem causar re-renderizações desnecessárias.

Para aprender mais sobre este hook: <https://react.dev/reference/react/useRef>

## `useForm` com a biblioteca `react-hook-form`

`react-hook-form` é uma biblioteca popular para gerenciar formulários no React. Ele simplifica a validação e manipulação de dados de formulários. Primeiro, você precisa instalar a biblioteca usando `npm` ou `yarn`:

```bash
npm install react-hook-form
# ou
yarn add react-hook-form
```

Aqui está um exemplo de como usar o `useForm` com `react-hook-form`:

```tsx
import React from 'react';
import { useForm } from 'react-hook-form';

const MyForm = () => {
  const { register, handleSubmit, errors } = useForm();

  const onSubmit = (data: any) => {
    console.log(data); // Aqui você pode fazer algo com os dados do formulário
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        type="text"
        name="username"
        placeholder="Username"
        ref={register({ required: 'Username is required' })}
      />
      {errors.username && <p>{errors.username.message}</p>}

      <input
        type="password"
        name="password"
        placeholder="Password"
        ref={register({ required: 'Password is required', minLength: 6 })}
      />
      {errors.password && <p>{errors.password.message}</p>}

      <button type="submit">Submit</button>
    </form>
  );
};
```

Neste exemplo, register é uma função que cria uma referência interna para o campo do formulário. Quando o formulário é submetido, handleSubmit chama a função onSubmit, passando os dados do formulário. Se houver erros de validação, eles serão exibidos abaixo de cada campo.

### Validando outros cenários

#### Validando um Campo de E-mail:

```tsx
import React from 'react';
import { useForm } from 'react-hook-form';

const EmailForm = () => {
  const { register, handleSubmit, errors } = useForm();

  const onSubmit = (data) => {
    console.log(data.email);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        type="email"
        name="email"
        placeholder="E-mail"
        ref={register({ 
          required: 'E-mail é obrigatório',
          pattern: {
            value: /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i,
            message: 'Endereço de e-mail inválido'
          }
        })}
      />
      {errors.email && <p>{errors.email.message}</p>}
      <button type="submit">Submit</button>
    </form>
  );
};
```

#### Validando um Campo de Número de Telefone:

```tsx
import React from 'react';
import { useForm } from 'react-hook-form';

const PhoneForm = () => {
  const { register, handleSubmit, errors } = useForm();

  const onSubmit = (data) => {
    console.log(data.phone);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        type="tel"
        name="phone"
        placeholder="Número de Telefone"
        ref={register({ 
          required: 'Número de telefone é obrigatório',
          pattern: {
            value: /^\d{10}$/i,
            message: 'Número de telefone inválido (ex: 1234567890)'
          }
        })}
      />
      {errors.phone && <p>{errors.phone.message}</p>}
      <button type="submit">Submit</button>
    </form>
  );
};
```

#### Validando uma Senha Complexa:

Para validar uma senha complexa (que inclua pelo menos 1 caractere maiúsculo, 1 número e 1 caractere especial), você pode usar expressões regulares para verificar cada uma dessas condições:

```tsx
import React from 'react';
import { useForm } from 'react-hook-form';

const PasswordForm = () => {
  const { register, handleSubmit, errors } = useForm();

  const onSubmit = (data) => {
    console.log(data.password);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        type="password"
        name="password"
        placeholder="Senha"
        ref={register({ 
          required: 'Senha é obrigatória',
          pattern: {
            value: /^(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/,
            message: 'Senha deve conter pelo menos 1 maiúsculo, 1 número, 1 caractere especial e ter no mínimo 8 caracteres'
          }
        })}
      />
      {errors.password && <p>{errors.password.message}</p>}
      <button type="submit">Submit</button>
    </form>
  );
};
```

Certifique-se de consultar a documentação do [react-hook-form](https://react-hook-form.com/get-started) para obter informações detalhadas sobre como usar a biblioteca em diferentes cenários.

### `yup`

É uma biblioteca de validação usando `schema`. Primeiro, você precisará instalá-la usando `npm install yup` ou `yarn add yup`.

```tsx
import * as yup from 'yup';


const schema = yup.object().shape({
  name: yup.string().required('Nome é obrigatório'),
  email: yup.string().email('Endereço de e-mail inválido').required('E-mail é obrigatório'),
  phone: yup.string().min(11, 'Número de telefone inválido (ex: 11999998888)').required('Número de telefone é obrigatório'),
  password: yup.string().required('Senha é obrigatória').min(8, 'Senha deve ter no mínimo 8 caracteres'),
  confirmPassword: yup.string().oneOf([yup.ref('password'), null], 'Senhas devem coincidir').required('Confirmação de senha é obrigatória'),
});
```


## Docs

- <https://medium.com/@guigaoliveira_/conhecendo-o-useref-do-react-9d67e66>
- <https://ateliware.com/blog/react-hook-form>
- <https://www.devmedia.com.br/validando-formularios-com-react-hook-forms/42903>