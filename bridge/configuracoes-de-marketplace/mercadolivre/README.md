# Configurações iniciais com o Mercado Livre

## Primeiros Passos

> O Mercado Livre **não** precisa de contratação de Channel.

1. Caso deseje usar a mesma politica (Ou seja, as mesmas regras para sortimento, promoções e condições de entrega da loja) use a politica comercial "1".
Caso contrario leia este link [Seleção de política comercial.](http://help.vtex.com/hc/pt-br/articles/214166227)

2. Caso sua politica comercial seja diferente de 1, [Configure a logística para essa política comercial.](http://help.vtex.com/hc/pt-br/articles/214166667-Atualiza%C3%A7%C3%A3o-de-estoque)

3. Entrar em contato com a Mercado Livre e obtenha o Token de acesso a API:

 comercial@mercadolivre.com

## Bridge - Visão geral

>**Importante: As configurações realizadas no Bridge para Mercado Livre realiza a criação automatica do Afiliado no OMS e libera o Perfil de Acesso no E-Commerce.**

### Exemplo de acesso as Configurações de Marketplace

![Config mercado livre](Marketplace_Config.gif)

## Campos

![Campos mercado livre](MercadoLivre.png)

>>Para cada campo temos:

* **Integração ativada**: Quando “Sim” ativa a exportação de Produtos, Preço e Estoque. Quando “Não” inativa a integração inteira.
* **Email para notificação**:Caso um Pedido sofra algum erro no ato de integrar na VTEX, será enviado um email com sua possível causa para o email aqui cadastrado(s).
* **Política Comercial**: ID da Política Comercial que será utilizada para o parceiro Mercado Livre.

##Sobre a integração

Após as configuração acima forem concluídas, será necessário reindexar a base para que todos os itens sejam integrados corretamente.

Para isso, siga os passos abaixo:

* Com o usuário master, entre no link [nomedasualoja].vtexcommercestable.com.br/admin/Site/FullCleanUp.aspx
* Clique no botão "Reidexar Base"


![Campos Cnova](reindexacao.png)

> Obs.: É possível acompanhar quantos produtos estão na fila acessando o módulo de E-Commerce. Lá existe vários dashboards que mostram: o total indexado, itens ainda não enfileirado, total na fila, na fila recentemente, entre outros.


Os itens passarão por um processo de catalogação até serem de fato disponibilizados no Marketplace. Esse prazo é variável de acordo com a demanda que o MarketPlace tiver. À medida que forem exportados, serão informados os preços e itens disponíveis em estoque de cada produto. Toda vez que houver uma alteração no preço de um produto, as informações serão atualizadas junto ao Marketplace. As atualizações relacionadas aos itens em estoque funcionam online toda vez que tiverem 5 unidades ou menos em estoque. Movimentações de estoques diferente dessa regra, não é indexada.
