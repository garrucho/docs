##Configurações iniciais com a Cnova

## Primeiros Passos


> A Cnova **não** precisa de contratação de Channel.

1. [Seleção de política comercial.](http://help.vtex.com/hc/pt-br/articles/214166227)

2. [Configure a logística para essa política comercial.](http://help.vtex.com/hc/pt-br/articles/214166667-Atualiza%C3%A7%C3%A3o-de-estoque)

3. Entrar em contato com a Cnova e obtenha o Token de acesso a API:

 http://www.extra.com.br/marketplace/venda-no-extra.aspx

## Bridge - Visão geral


>**Importante: As configurações realizadas no Bridge para Cnova realiza a criação automatica do Afiliado no OMS e libera o Perfil de Acesso no E-Commerce.**

###Exemplo de acesso as Configurações de Marketplace

![Config CNova](V_newconfig_cnova2.gif)

##Campos

![Campos Cnova](cnova.png)

>>Para cada campo temos:

* **Integração de Produtos**: Liga ou Desliga a exportação de Produtos

* **Integração de Pedidos**: Liga ou Desliga a exportação de Pedidos

* **Integração de Preços**: Liga ou Desliga a exportação de Preços

* **Integração de Estoques**: Liga ou Desliga a exportação de Estoque

* **ApiKey**: Chave de identificação da sua Loja no Marketplace fornecida diretamente pela B2W

* **Id do Afiliado**: ID do Afiliado da Rakuten cadastrado em OMS > Configurações > Afiliados > ID

* **Taxa da Divergência de Preço**: Taxa de aceitação da divergência de valor no Pedido integrado

* **Nome do campo de Especificação de Produto que contém sua Descrição**: Será aqui mapeado, nas Especificações do Produto, o campo que contém o conteúdo utilizado como Descrição do mesmo.

* **E-mails Para Notificação de Pedidos Não Integrados**: Caso um Pedido sofra algum erro no ato de integrar na VTEX, será enviado um email com sua possível causa para o(s) email(s) aqui cadastrado(s)


##Sobre a integração

Após as configuração acima forem concluídas, será necessário reindexar a base para que todos os itens sejam integrados corretamente.

Para isso, siga os passos abaixo:

Entre no link [nomedasualoja].vtexcommercestable.com.br/admin/Site/FullCleanUp.aspx com o usuário master
Clique em "Reidexar Base"
Obs.: É possível acompanhar quantos produtos estão na fila acessando o módulo de E-Commerce > Relatórios > Relatório de indexação. Lá tem vários dashboards que mostram: total indexado, itens ainda não enfileirado, total na fila, na fila recentemente, entre outros. Para saber mais acesse o artigo no help Como funciona a indexação.


![Campos Cnova](reindexacao.png)

Os itens passarão por um processo de catalogação até serem de fato disponibilizados nos sites da CNova. Esse prazo é variável de acordo com a demanda que o MarketPlace tiver. À medida que forem exportados, serão informados os preços e itens disponíveis em estoque de cada produto. Toda vez que houver uma alteração no preço de um produto, as informações serão atualizadas junto à CNova. As atualizações relacionadas aos itens em estoque funcionam online toda vez que tiverem 5 unidades ou menos em estoque. Movimentações de estoques diferente dessa regra, não é indexada.
