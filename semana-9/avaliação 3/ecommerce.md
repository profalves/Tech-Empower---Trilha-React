# Desafio

Você teve a oportunidade de iniciar com a construção de ecommerce, iniciando pelos formulários de login e cadastro na avaliação passada. Daremos continuidade construindo um e-commerce por completo até a nossa ultima semana. Com isso, nesta semana será importante o desenvolvimento da navegação na **Jornada Simples de Compra**.

Com isso, a escolha da loja virtual que deseja construir (moda, eletrônicos, mercados, variedades, etc.) será fundamental para as regras e o que será necessário incrementar nela ao longo dos desafios.

## Estrutura de navegação

Esta semana precisaremos desta estrutura de navegação criada:

```bash
|-- *home*
    |-- PLP (Product List Page)
    |-- *PDP* (Product Detail Page)
    |-- Cart
        |-- resume
        |-- checkout
        |-- confirmation
|-- Minha Conta
|-- Admin (CMS) (optional)
    |-- Cadastro de Produtos
    |-- Listagem de Clientes
|-- *Login* 
|-- *Cadastro*
```

## Critérios de Aceite

### Home page

- Deve Apresentar um banner principal onde pode ser uma única imagem ou um carrossel de imagens.
- Uma lista de no mínimo 8 produtos. Podem ser apresentados em uma lista na tela ou em um carrossel de produtos.
- Caso escolha optar por um carrosel de produtos, a escolha da biblioteca que fará tal funcionalidade é a seu critério e contará como extra da sua nota.
- Cada produto será um card onde terá:
  - **imagem do produto** 
  - **nome** 
  - **preço** 
  - **botão** de *"adicionar no carrinho"*. 
- Pode alterar a quantidade de produtos ao ser enviada no carrinho através dele, mas não é obrigatório.
- Fique a vontade para deixar o site mais próximo do segmento que você escolheu, e pode apresentar mais de uma lista de produtos (novidades, mais vendidos, etc.). Flexivel a no mínimo 4 produtos por seção.

### PDP (Product Details Page)

- Deverá abrir o produto com a imagem mais ampliada.
- Nome do produto.
- Preço.
- Quantidade a ser lançada no carrinho.
- Botão para adicionar este produto ao carrinho.

### Header/Footer

Para toda a navegação pode apresentar um `header` e/ou um `footer`, e estes também devem condizer com o segmento escolhido para o seu site.

#### Header

- Logo (Deve ter o link para a home do site)
- Menu categorias do site (no mínimo de 3 sessões)
- Ícone para a conta do usuário (cliente). Caso ele ainda não esteja logado, deverá aparecer um botão de Login. Caso esteja logado, ao lado do Icone deve aparececer o nome do usuário. Para isso pode aplicar Contexto (`useContext`) para salvar os dados do usuário.
- Ícone para ir para o carrinho

#### Footer

- Deve estar dividido em 3 colunas
   1. Deve conter Logo da loja, um slogan e informações sobre a loja, endereço, etc.
   2. Os links para diferentes partes do site e/ou links externos.
   3. Contatos. Pode ser icones das redes sociais.

### Responsividade

A partir de agora já teremos um site com mais cara de **E-Commerce**, por isso a **responsividade** será visto como algo obrigatório.

## Pesos

- [**Home**](#home-page): 3 pontos
- [**PDP**](#pdp-product-details-page): 3 pontos
- [**Header**](#header): 1 ponto
- [**Footer**](#footer): 1 ponto 
- [**Responsividade**](#responsividade): 1 ponto
- ***Extras***: 1 ponto

## Modelo/Exemplo

Para se inspirar e ver como é uma jornada completa em um e-commerce, acesse: 
- <https://storetheme.vtex.com/>
- <https://www.figma.com/community/file/1039629108858463825>

## Envio

Para envio, suba para o repositório definitivo do seu projeto e envia na resposta da atividade do classroom.
