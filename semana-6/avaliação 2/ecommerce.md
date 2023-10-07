# Desafio

Você a partir de agora terá a oportunidade de construir um e-commerce por completo até a nossa ultima semana. Com isso, nesta semana será importante o desenvolvimento do cadastro de clientes e página de login.

Com isso, é importante antes de começar, já escolher qual loja virtual deseja construir (moda, eletrônicos, mercados, variedades, etc.). Isso será fundamental para as regras de negócios que irá precisar e o planejamento ao longo dos desafios.

## Estrutura de navegação

Esta são as possibilidade de páginas que o seu e-commerce pode ter, **mas por agora apenas precisará criar a tela de cadastro e login**:

```bash

|-- home
    |-- PLP (Product List Page)
    |-- PDP (Product Detail Page)
    |-- Cart
        |-- resume
        |-- checkout
        |-- confirmation
|-- Minha Conta
|-- Admin (CMS)
    |-- Cadastro de Produtos
    |-- Listagem de Clientes
|-- **Login**
|-- **Cadastro**

```

## Critérios de Aceite

### Criação do usuário

- A tela poderá ser acessada pela tela inicial, pode ser os dois links para cada página ou simplesmente iniciar já com a tela de login, com a mensagem abaixo _"Se ainda não é cadastrado, `clique aqui`"_ e o link direciona para o cadastro e na tela de cadastro abaixo dela terá a mensagem _"Se você já tem cadastro, `faça login`"_ e o link será direcionado para a tela de login.
- Os campos do formulário de cadastro, devem ser:
  - **Nome completo**
  - **Email**
  - **Telefone**
  - **Endereço**:
    - **CEP** (Futuramente podemos ter uma consulta a uma API de sua escolha para preencher os dados do endereço do cliente, conforme o CEP consultado)
    - Rua, Avenida, travessa, etc. (**Logradouro**)
    - **Número**. Precisa ter alguma indicação do que preencher quando não tiver número
    - **Complemento**
    - **Bairro**
    - **Cidade**
    - **Estado**
  - **Campo para senha**
  - **Campo para repetir e confirmar a senha**
  - **Checkbox** aceitando as **politicas de privacidade**. Não precisa existir a página de politica de privacidade, mas pode apresentar como link
  - **Botão de salvar o cadastro**, este botão deverá disparar um `console.log` mostrando os dados todos validados

***OBS***. 
- A ação de enviar os dados será feita em futuros conteúdos
- Caso queira adicionar uma página para cadastro de produtos pode fazer também que contará como pontos extras e você mesmo pode fazer do seu jeito, contanto que faça sentido com os futuros produtos da loja que você escolheu.

### Tela de Login

- Os campos do Tela de Login, devem ser:
  - **Email**
  - **Senha** (Lembrando que para inputs de senhas deverá ter `type=password`)
  - **Botão de realizar login**. Não precisa fazer nenhuma ação além de disparar um `console.log` mostrando os que o email e senha são campos válidos.
- Então sendo assim, deve ter as mesmas validações para **email** e **senha** da tela de cadastro

## Pesos

Considerando que o desafio vale **10 pontos**:

1. Página de cadastro de usuários _-> 3 pontos_
2. Validação de campos: _-> 3 pontos_
     - Nome é obrigatório
     - Email é obrigatório e tem que ser válido
     - Numero de celular tem que ter 11 digitos DDD + numero
     - No Endereço o CEP é obrigatório
     - As senhas devem coincindir, ter no mínimo 8 caracteres
     - Deve exigir que a caixa de aceite das politicas de privacidade esteja preenchida
3. Tela de Login (As mesmas validações para email e senha da tela de cadastro) _-> 3 pontos_
4. Extra (_escolha por um ou mais_): _-> 1 ponto_
     - Tela de cadastro de produtos 
     - Valiações extras no cadastro de clientes, por exemplo, validar o formato de email para que tenha `@` e `.`
     - Adicionar um ***Contexto*** e realizar a ***troca de tema claro e escuro*** usando `useContext` e `styled-components`

Para se inspirar e ter um exemplo de loja completa para criar a sua, acesse: <https://storetheme.vtex.com/>

## Envio

- Para envio, suba para um repositório criado para este projeto no Github
- Qualquer dificuldade, favor informar com antecedencia, estou aqui para ajudar.
- **Prazo**: Até as *23h59* do domingo, dia 08/10/2023
