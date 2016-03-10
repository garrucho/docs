# Configurações de Marketplace

No Vtex Bridge existe a opção de Configurações de Marketplaces, onde nela é possivel acessar todas as configurações dos Marketplaces, ligar, editar ou desativar novas integrações.Hoje temos os seguintes Marketplaces com integração ativa: **B2W**, **CNova**, **Rakuten**, **Buscapé**, **CSU**, **Mercado Livre** e **Walmart** (este apenas na parte de Produtos, Preços e Estoque. Pedidos o próprio Walmart que insere na VTEX).
As movimentações que serão registrados no VETX Bridge serão apenas para aqueles Marketplaces que hoje integramos de forma **ativa**.  
Onde a VTEX quem realiza as exportações de Produtos, Preços e Estoque e/ou onde é capturado os Pedidos realizados no Marketplace.



## Visão geral

![Visão geral_config](V_newconfig.gif)

## Integrações

Hoje a VTEX realiza desde integrações 100% (onde se exportamos o catálogo de produtos, atualizamos Preços e Estoques e consumimos os Pedidos
fechados no Marketplace)  até integrações parcialmente ou totalmente Passival (onde apenas notificamos o Parceiro sobre novos produtos e alterações de Preço e Estoque).

Como exemplo das integrações que temos e do que cuidamos, temos o seguinte quadro:

![Mkt Bridge](MarketPlace.png)

> Para integrações 100% Vtex, temos

**Sugestão de venda**
Todo produto que é enviado ao marketplace, ele é analisado e mapeado,
Só depois o marketplace aprova a sua venda de acordo com a suas regras.

**Atualizações de Preço e Estoque**
As atualizações de Preço e Estoque demoram entre de 1 a 10 minutos após a indexação do Produto na Vtex.
Também pode ocorrer de haver um grande volume de indexação que cause a demora par avisar ao Parceiro.

**Disponibilidade dos Pedidos**
Cada marketplace tem a sua regra para disponibilizar um pedido, não necessariamente um pedido é disponibilizado no momento do fechamento da compra.

**Problemas comuns**

- Erro na falta de disponibilidade (no estoque) do item. Isto é, se tem um item mas em estoque raso e ainda assim o mesmo item aparecer em varios Marketplace além da Loja principal.

- Conflito  de disponibilidade de estoque com um pedido ja fechado.

- Erro no valor do produto e/ou CEP comercializado no Marketplace não ser o mesmo cadastrado na Loja.Onde normalmente isso ocorre porque o markeplace usa uma tabela de frete propria não levando em consideração o valor retornado da simulação do seller.


> A seguir temos instruções específicas para cada Marketplace acima comentado :
>

* [B2W](b2w/index.html)

* [CNova](cnova/index.html)

* [Rakuten](rakuten/index.html)

* [Buscapé](buscape/index.html)

* [Walmart](walmart/index.html)

* [MercadoLivre](mercadolivre/index.html)

* [CSU](csu/index.html)
