# Princípios de Design de Software

## Introdução

Na engenharia de software, os princípios são diretrizes essenciais que orientam o desenvolvimento de sistemas robustos, escaláveis e de fácil manutenção. Entre eles, quatro princípios fundamentais são frequentemente destacados: ***KISS***, ***YAGNI***, ***DRY*** e ***SOLID***.

## KISS (Keep It Simple, Stupid)
O princípio **KISS** prega a simplicidade na concepção e na implementação do código, onde sugere que as soluções devem ser simples, diretas e fáceis de entender. A ideia é manter as soluções tão simples quanto possível, evitando complexidade desnecessária. Isso promove a compreensão fácil do código, facilita a manutenção e reduzir a chance de erros.

### Exemplo com React: Evite a criação de componentes complexos. Mantenha a lógica de renderização simples e fácil de compreender. Por exemplo, um componente de botão:

```tsx
import React from 'react';

type ButtonProps = {
  text: string;
  onClick: () => void;
}

const Button: React.FC<ButtonProps> = ({ text, onClick }) => {
  return <button onClick={onClick}>{text}</button>;
};

export default Button;
```

## YAGNI (You Aren't Gonna Need It)
**YAGNI** defende a ideia de não adicionar funcionalidades que não são necessárias no momento. Em vez de antecipar necessidades futuras, concentra-se na implementação do que é necessário no presente. Isso evita o excesso de engenharia e reduz o desperdício de recursos. Então adicione apenas o que é necessário para agora.

```tsx
🚫 // Exemplo com Excesso de useState
import React, { useState } from 'react';

const ExcessiveForm: React.FC = () => {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  // ... mais declarações de useState para outros campos, se necessário

  const handleNameChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setName(event.target.value);
  };

  const handleEmailChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setEmail(event.target.value);
  };

  // ... mais funções de manipulação de eventos para outros campos, se necessário

  const handleSubmit = (event: React.FormEvent) => {
    event.preventDefault();
    // Lógica de envio do formulário
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" value={name} onChange={handleNameChange} placeholder="Name" />
      <input type="email" value={email} onChange={handleEmailChange} placeholder="Email" />
      {/* Mais campos do formulário, se necessário */}
      <button type="submit">Submit</button>
    </form>
  );
};

export default ExcessiveForm;

✅ // Exemplo aplicando YAGNI 
import React from 'react';

const SimpleForm: React.FC = () => {
  const handleSubmit = (event: React.FormEvent) => {
    event.preventDefault();
    const formData = new FormData(event.target as HTMLFormElement);
    const data: Record<string, string> = {};

    formData.forEach((value, key) => {
      data[key] = value as string;
    });

    console.log(data); // Dados do formulário capturados
    // Lógica de envio do formulário
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" name="name" placeholder="Name" />
      <input type="email" name="email" placeholder="Email" />
      <textarea name="message" placeholder="Message" />
      <button type="submit">Submit</button>
    </form>
  );
};

export default SimpleForm;
```

No exemplo de formulário aplicando YAGNI, os dados do formulário são capturados diretamente do evento submit. Isso elimina a necessidade de vários useState e funções de manipulação de eventos para cada campo individual. Essa abordagem simplificada adere ao princípio de não adicionar funcionalidades que não são necessárias no momento, mantendo o código mais enxuto e direto ao ponto.

## **DRY** (Don't Repeat Yourself)
O princípio DRY prega a reutilização de código. Incentiva a reutilização de código ao evitar a duplicação. A ideia é ter uma única fonte de verdade para cada parte do sistema, minimizando a repetição de lógica ou dados. Isso simplifica a manutenção, reduz erros e promove a coesão do código. Em vez de repetir lógica, funções ou informações, coloque o código duplicado em um local centralizado e referencie-o sempre que necessário.

```tsx
import { useState, useEffect, useCallback } from 'react';

interface RequestOptions {
  method: 'GET' | 'POST' | 'PUT' | 'PATCH' | 'DELETE';
  body?: Record<string, any>;
}

function useFetch(url: string) {
  const [data, setData] = useState<any>(null);
  const [loading, setLoading] = useState<boolean>(true);

  const fetchData = useCallback(async (options: RequestOptions) => {
    setLoading(true);
    try {
      const response = await fetch(url, {
        method: options.method,
        headers: {
          'Content-Type': 'application/json', // Defina os cabeçalhos conforme necessário
        },
        body: options.body ? JSON.stringify(options.body) : undefined,
      });

      const result = await response.json();
      setData(result);
    } catch (error) {
      console.error('Error fetching data:', error);
    } finally {
      setLoading(false);
    }
  }, [url]);

  useEffect(() => {
    fetchData({ method: 'GET' });
  }, [fetchData]);

  return { data, loading, fetchData };
}

export default useFetch;
```

Neste exemplo atualizado, a função `fetchData` agora aceita um objeto de opções, permitindo especificar um método (`GET` ou `POST`, por exemplo) e opcionalmente enviar um corpo (`body`) no caso de uma solicitação `POST`. Isso possibilita a reutilização do hook `useFetch` para diferentes tipos de solicitações.

Para utilizar essa função `useFetch` em seu componente, você pode fazer o seguinte:

```tsx
import React, { useEffect } from 'react';
import useFetch from './useFetch';

const ExampleComponent: React.FC = () => {
  const { data, loading, fetchData } = useFetch('https://api.example.com/data');

  useEffect(() => {
    // Exemplo de chamada GET
    fetchData({ method: 'GET' });
  }, [fetchData]);

  const handlePostData = () => {
    // Exemplo de chamada POST
    fetchData({ method: 'POST', body: { /* Seus dados para enviar no corpo */ } });
  };

  return (
    <div>
      {loading ? <p>Loading...</p> : <p>Data: {JSON.stringify(data)}</p>}
      <button onClick={handlePostData}>Send POST Request</button>
    </div>
  );
};

export default ExampleComponent;
```

Agora, a função `fetchData` aceita opções para especificar o método e o corpo, permitindo chamadas tanto para solicitações GET quanto POST, enquanto mantém o código DRY (Don't Repeat Yourself) ao reutilizar a lógica de busca em ambas as situações.


## SOLID (Princípios de Design de Objetos)
SOLID é um acrônimo para cinco princípios de design orientado a objetos formulados por [**Robert C. Martin**](https://pt.wikipedia.org/wiki/Robert_Cecil_Martin).

**S - Single Responsibility Principle**: Uma classe deve ter apenas uma razão para mudar.
**O - Open/Closed Principle**: As entidades de software devem ser abertas para extensão, mas fechadas para modificação.
**L - Liskov Substitution Principle**: Subtipos devem ser substituíveis por seus tipos base sem afetar a integridade do sistema.
**I - Interface Segregation Principle**: Múltiplas interfaces específicas são melhores do que uma única interface geral.
**D - Dependency Inversion Principle**: Dependa de abstrações, não de implementações concretas.

Esses princípios visam criar código mais limpo, flexível e de fácil manutenção, dividindo responsabilidades, evitando dependências desnecessárias e criando componentes reutilizáveis.

```tsx
🚫 // Exemplo com Má Prática: APROPCALIPSE
import React from 'react';

interface BadModalProps {
  isOpen: boolean;
  onClose: () => void;
  headerContent: React.ReactNode;
  modalContent: React.ReactNode;
  footerContent: React.ReactNode;
  primaryButtonLabel: string;
  secondaryButtonLabel: string;
  onPrimaryButtonClick: () => void;
  onSecondaryButtonClick: () => void;
  // ... mais props, se necessário
}

const BadModal: React.FC<BadModalProps> = ({
  isOpen,
  onClose,
  headerContent,
  modalContent,
  footerContent,
  primaryButtonLabel,
  secondaryButtonLabel,
  onPrimaryButtonClick,
  onSecondaryButtonClick,
}) => {
  if (!isOpen) {
    return null;
  }

  return (
    <div className="modal">
      <div className="modal-header">{headerContent}</div>
      <div className="modal-body">{modalContent}</div>
      <div className="modal-footer">{footerContent}</div>
      <button onClick={onPrimaryButtonClick}>{primaryButtonLabel}</button>
      <button onClick={onSecondaryButtonClick}>{secondaryButtonLabel}</button>
    </div>
  );
};

export default BadModal;

✅ // Exemplo com Boa Prática (SOLID)

// Header Component

import React from 'react';

interface ModalHeaderProps {
  children: React.ReactNode;
}

const ModalHeader: React.FC<ModalHeaderProps> = ({ children }) => {
  return <div className="modal-header">{children}</div>;
};

export default ModalHeader;

// Footer Component

import React from 'react';

interface ModalFooterProps {
  children: React.ReactNode;
}

const ModalFooter: React.FC<ModalFooterProps> = ({ children }) => {
  return <div className="modal-footer">{children}</div>;
};

export default ModalFooter;

// Actions (Botões) Component

import React from 'react';

interface ModalActionsProps {
  primaryButtonLabel: string;
  secondaryButtonLabel: string;
  onPrimaryButtonClick: () => void;
  onSecondaryButtonClick: () => void;
}

const ModalActions: React.FC<ModalActionsProps> = ({
  primaryButtonLabel,
  secondaryButtonLabel,
  onPrimaryButtonClick,
  onSecondaryButtonClick,
}) => {
  return (
    <div className="modal-actions">
      <button onClick={onPrimaryButtonClick}>{primaryButtonLabel}</button>
      <button onClick={onSecondaryButtonClick}>{secondaryButtonLabel}</button>
    </div>
  );
};

export default ModalActions;

// Modal Utilizando os Componentes Separados

import React from 'react';
import ModalHeader from './ModalHeader';
import ModalFooter from './ModalFooter';
import ModalActions from './ModalActions';

interface GoodModalProps {
  isOpen: boolean;
  onClose: () => void;
}

const GoodModal: React.FC<GoodModalProps> = ({ isOpen, onClose }) => {
  if (!isOpen) {
    return null;
  }

  const handlePrimaryButtonClick = () => {
    // Lógica do botão primário
  };

  const handleSecondaryButtonClick = () => {
    // Lógica do botão secundário
  };

  return (
    <div className="modal">
      <ModalHeader>Conteúdo do Cabeçalho</ModalHeader>
      <div className="modal-body">Conteúdo Interno do Modal</div>
      <ModalFooter>Conteúdo do Rodapé</ModalFooter>
      <ModalActions
        primaryButtonLabel="Primary"
        secondaryButtonLabel="Secondary"
        onPrimaryButtonClick={handlePrimaryButtonClick}
        onSecondaryButtonClick={handleSecondaryButtonClick}
      />
    </div>
  );
};

export default GoodModal;
```

Com a abordagem do segundo exemplo, você pode reutilizar facilmente os componentes ModalHeader, ModalFooter e ModalActions em outros modais, promovendo a coesão, facilitando a manutenção e seguindo o princípio SOLID, especialmente o princípio da Responsabilidade Única (SRP).

## Conclusão
Aplicar os princípios `KISS`, `YAGNI`, `DRY` e `SOLID` no desenvolvimento de software é crucial para criar sistemas mais robustos, flexíveis e fáceis de manter. A simplicidade, a evitação do excesso e a organização do código não apenas melhoram a legibilidade, mas também reduzem bugs, facilitam a manutenção e promovem a escalabilidade do software. Incorporar esses princípios no processo de desenvolvimento ajuda a construir produtos de alta qualidade e sustentáveis ao longo do tempo.