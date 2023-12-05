# Projeto Final

Você teve a oportunidade de iniciar com a construção de uma jornada simples de compras. Daremos continuidade construindo um e-commerce por completo até a nossa ultima semana. Com isso, nesta semana será o desenvolvimento das páginas de PLP (Product List Page) e Minha Conta.

Com isso, precisa escolher qual loja virtual deseja construir (moda, eletrônicos, mercados, variedades, etc.). Isso será fundamental para as regras e o que será necessário incrementar nela ao longo dos desafios.

## Estrutura de navegação

Esta semana precisaremos desta estrutura de navegação criada:

```bash

|-- home
    |-- PLP
    |-- PDP
    |-- Cart
        |-- resume
        |-- checkout
        |-- confirmation
|-- Minha Conta
|-- Admin (CMS)
    |-- Cadastro de Produtos
    |-- Listagem de Clientes
|-- Login

```

## Critérios de Aceite

- [Projeto Final](#projeto-final)
  - [Estrutura de navegação](#estrutura-de-navegação)
  - [Critérios de Aceite](#critérios-de-aceite)
    - [Critérios Anteriores (ultima chance)](#critérios-anteriores-ultima-chance)
    - [PLP (Product List Page) e Busca de Produtos](#plp-product-list-page-e-busca-de-produtos)
    - [Minha Conta](#minha-conta)
    - [Login](#login)
    - [Testes](#testes)
  - [Pesos](#pesos)
  - [Modelo/Exemplo](#modeloexemplo)
- [Fake Apis para produtos](#fake-apis-para-produtos)
  - [Envio](#envio)

### Critérios Anteriores (ultima chance)

- Todos os critérios anteriores poderão ser atingidos a fim de tornar o seu ecommerce mais completo e satisfatório. Além de ser um portfolio para o seu Github. Então vamos lá e mão na massa: [Semana 2 - Critérios de Aceite](../semana-2/DESAFIO_S2.md)

### PLP (Product List Page) e Busca de Produtos

- Para se ter uma página de lista de produtos, antes precisamos ter as 3 opções no menu, como solicitado anteriormente e que cada uma seja para uma categoria de produtos. Logo os seus produtos deverão ter uma categoria associada a eles.
- Ao clicar nessa categoria no menu, deverá ser redirecionado para PLP que irá carregar somente os produtos dessa categoria.
- Precisa também permitir que na tela exista um input para pesquisar produtos, o resultado da pesquisa deverá ser independente da categoria selecionada. 
- Ao clicar no botão de busca de produtos, deverá ser redirecionado para a página de busca de produtos com os dados da categoria selecionada.

### Minha Conta

- Deverá ser protegida e somente acessar quando estiver logado.
- Caso o usuário não esteja logado, o usuário deverá ser avisado que os dados de login não estão corretos.
- No header o ícone/botão para a conta deverá mudar para o nome do usuário logado e ao clicar deverá ser redirecionado para a página de minha conta com os dados deste usuário.
- Nesta tela deverá ter também a opção de voltar pra home e fazer logout.

### Login

- A tela de login deverá ter os campos de login e senha.
- Agora é necessário existir uma lógica e será mais fácil usando um serviço http para o site [DummyJson](https://dummyjson.com/docs/auth) ou alguma *API fake* que faça algo similar.
- Após o login, redirecionar para a página principal

### Testes

É necessario que os testes cubram uma margem de 40% de linhas e funcões em seu código. Devem sintetizar com a aplicação e os testes existentes quando os componentes/recursos forem criados serão ignorados. 

## Pesos

Considerando que o desafio vale **10 pontos**:

1. Critérios Anteriores (segunda chance): **3 pontos**
2. PLP e Busca de produtos: **2 pontos**
3. Login: **1 ponto**
4. Minha Conta: **1 ponto**
5. Testes: **1 ponto**
6. Extras: **1 ponto**
7. Apresentação: **1 ponto**


## Modelo/Exemplo

Para se inspirar e ver como é uma jornada completa em um e-commerce, acesse: 
- <https://storetheme.vtex.com/>
- <https://www.figma.com/community/file/1039629108858463825>

# Fake Apis para produtos

- <https://dummyjson.com/docs>
- <https://fakestoreapi.com/docs>
- <https://fakeapi.platzi.com/en/rest/products/>

## Envio

- Para envio, suba para o repositório do seu projeto no Github
- Deverá criar um pull/request de uma branch nova para que seja comparada as mudanças somente dessa semana, se possível.
- Qualquer dificuldade, favor informar com antecedencia, estou aqui para ajudar.
- **Prazo**: Até as *23h59*, dia 13/12/2023
