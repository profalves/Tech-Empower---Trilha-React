# Desafio

Você teve a oportunidade em trabalher no seu próprio ecommerce, iniciando pelos formulários de login e cadastro, depois criando as páginas home e de produto no site, e por fim mais layouts na avaliação passada. Daremos continuidade construindo um e-commerce por completo até a nossa ultima semana. Com isso, nesta semana será importante o desenvolvimento do **Carrinho de Compra**.

Com isso, já contamos com a escolha da loja virtual que deseja construir (moda, eletrônicos, mercados, variedades, etc.) pois será fundamental para as regras de negócios que vão permear essa parte e o que será necessário incrementar nela ao longo dos desafios.

## Estrutura de navegação

Esta semana precisaremos desta estrutura de navegação criada:

```bash
|-- *home*
    |-- PLP (Product List Page)
    |-- *PDP* (Product Detail Page)
    |-- *Cart*
        |-- *resume*
        |-- *checkout* (pagamento)
        |-- *confirmation* 
|-- Minha Conta
|-- Admin (CMS) (optional)
    |-- Cadastro de Produtos
    |-- Listagem de Clientes
|-- *Login* 
|-- *Cadastro*
```

## Critérios de Aceite

### Os critérios anteriores
- Login
- Cadastro
- Home
- PDP
- Header/Footer

### Carrinho

- O carrinho será uma lista de produtos onde terá imagem, o nome, preço e quantidade de cada produto
- Também precisa mostrar o **Total** dos valores dos produtos
- Poderá ter o **Desconto** adicionando através de um cupom (Este cupom é uma string que representa o valor/porcentagem de um desconto. ex: `DESCONTO10`, `DESCONTO50`)
- Enviar esses cupons no README do seu projeto.
- Se seu carrinho tem desconto, então precisa ter **Subtotal** *(Total sem desconto)*
- Precisa ter como excluir o produto do carrinho.
- Precisam ter uma rota para o resumo do pedido (`/cart`), que mostra a lista de produtos adicionados. Nele haverá o botão para **ir para a tela de pagamento**.
- Nesta tela de pagamentos será aplicado o cupom de desconto. Então a partir de agora é necessário termos um input para o nome do cupom e o demonstrativo de subtotal, desconto aplicado(valor e/ou porcentagem) e total (total = subtotal - desconto).
- Precisam ter uma rota para o pagamento do pedido (`/checkout`), onde deve só passar para a próxima após escolher a forma de pagamento. Ou seja, sem ter um pagamento escohido o botão de **Finalizar Compra** deverá ficar desabilitado.
- Precisam ter uma rota para a confirmação do pedido(/`confirmation`), onde o usuário será avisado que o seu pedido foi realizado com sucesso e logo abaixo terá os detalhes do mesmo:
  - Lista de itens comprados, com imagem, nome, quantidade e preço em cada
  - Subtotal, Desconto aplicado e Total.
  - Dados do Pagamento

 ***PS***: Enviar os dados de entrega, ficará para a próxima avaliação.


### Responsividade

Ainda manteremos como critério importante, então a **responsividade** será visto como obrigatório. Lembrem de trabalhar toda a estilização by `mobile-first`

## Pesos

- [**Critérios anteriores**](#home-page): 3 pontos
- [**Carrinho de Compras**](#home-page): 5 pontos
- [**Responsividade**](#responsividade): 1 ponto
- ***Extras***: 1 ponto

## Modelo/Exemplo

Para se inspirar e ver como é uma jornada completa em um e-commerce, acesse: 
- <https://storetheme.vtex.com/>
- <https://www.figma.com/community/file/1039629108858463825>

## Envio

Para envio, suba para o repositório definitivo do seu projeto e envia o link dele na resposta da atividade do classroom.
