# Configurações de Marketplace

No Vtex Bridge existe a opção de Configurações de Marketplaces. Nela, é possível acessar todas as configurações dos Marketplaces, ligar, editar ou desativar novas integrações. Hoje temos os seguintes Marketplaces com integração ativa: **B2W**, **CNova**, **Rakuten**, **Buscapé**, **CSU**, **Mercado Livre** e **Walmart** .

>Obervações: No caso do Mercado Livre a Vtex é encarregada apenas de criar o afiliado e liberar o acesso do usuario Mercado Liver na loja.

As movimentações que serão registrados no VETX Bridge, serão apenas para aqueles Marketplaces que hoje integramos de forma **ativa**, ou seja, aquelas onde é a VTEX quem realiza as exportações de Produtos, Preços e Estoque, ou onde é capturado os Pedidos realizados no Marketplace.



## Visão geral

![Visão geral_config](V_newconfig.gif)

## Integrações

Hoje a VTEX realiza desde integrações **ativas** (onde se exportamos o catálogo de produtos, atualizamos Preços e Estoques e consumimos os Pedidos
fechados no Marketplace)  até integrações **parcialmente** ou **totalmente Passivas** (onde apenas notificamos o Parceiro sobre novos produtos e alterações de Preço e Estoque).

O quadro abaixo ilustra a relação entre os Marketplaces e as responsabilidades de cada integração na Vtex:

![Mkt Bridge](MarketPlace.png)

Para integrações ativas, temos:

### Caracteristicas das integrações ativas

**Sugestão de venda**
Todo produto que é enviado ao marketplace, é analisado e mapeado. Só depois o marketplace aprova a sua venda, de acordo com a suas regras.

**Atualizações de Preço e Estoque**
As atualizações de Preço e Estoque, demoram em média de 1 a 10 minutos, após a indexação do Produto na Vtex.
Caso haja um grande volume de indexação, é possível que ocorra uma demora para avisar o Parceiro.

**Disponibilidade dos Pedidos**
Cada marketplace tem a sua regra para disponibilizar um pedido. Assim, um pedido pode vir a não ser disponibilizado no momento do fechamento da compra.

**Problemas comuns:**

- Erro de indisponibilidade (no estoque) do item. Isto é, existe um item em estoque raso, porém este mesmo item aparece em varios Marketplaces, além da Loja principal.

- Conflito de disponibilidade de estoque com um pedido ja fechado.

- Erro de inconsistência entre o valor cadastrado do produto e/ou CEP comercializado no Marketplace e o valor cadastrado na loja. Isto ocorre normalmente, porque o marketplace usa uma tabela de frete própria, não levando em consideração o valor retornado da simulação do seller.


> A seguir temos instruções específicas para cada Marketplace acima comentado :
>

* [B2W](b2w/index.html)

* [CNova](cnova/index.html)

* [Rakuten](rakuten/index.html)

* [Buscapé](buscape/index.html)

* [Walmart](walmart/index.html)

* [Mercado Livre](mercadolivre/index.html)

* [CSU](csu/index.html)

---

Autor: _Ana Izabel Jacobsen_</br>
Propriedade: _VTEX_ &copy;</br>
