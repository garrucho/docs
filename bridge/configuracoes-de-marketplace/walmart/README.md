# Configurações iniciais com o Walmart

## Primeiros Passos

1. [Contrate o channel.](http://help.vtex.com/hc/pt-br/articles/213517308-Channel)

2. Caso deseje usar a mesma politica (Ou seja, as mesmas regras para sortimento, promoções e condições de entrega da loja) use a politica comercial "1".
Caso contrario leia este link [Seleção de política comercial.](http://help.vtex.com/hc/pt-br/articles/214166227)

3. Caso sua politica comercial seja diferente de 1: [Configure a logística para essa política comercial.](http://help.vtex.com/hc/pt-br/articles/214166667-Atualiza%C3%A7%C3%A3o-de-estoque)

4. Entrar em contato com o Walmart e obtenha o usuario, senha e id do seller para acesso a API:

https://marketplace.walmart.com.br/prospection

## Bridge - Visão geral


>**Importante: As configurações realizadas no Bridge para Walmart realiza a criação automatica do Afiliado no OMS e libera o Perfil de Acesso no E-Commerce.**

### Exemplo de acesso as Configurações de Marketplace

![Config walmart](Walmartgif.gif)

## Campos

![Campos walmart](walmart.png)

>>Para cada campo temos:

* **Integração ativada**: Quando “Sim” ativa a exportação de Produtos, Preço e Estoque. Quando “Não” inativa a integração apagando o afiliado e permissão de acesso no Walmart.
* **Email para notificação**: Email que recebe notificações quando houver problemas na integração de Pedidos.
* **Usuário Walmart**: Usuário para autenticação das importações de Produtos, Preço e Estoque.
* **Senha Walmart**: Senha para autenticação das importações de Produtos, Preço e Estoque.
* **SellerId do Walmart**: ID da Loja no Walmart.
* **Endereço do site**: Endereço da Loja Principal (com "http://" ou "https") exemplo: http://www.lojavtex.com.br.
* **Política Comercial**: ID da Política Comercial que será utilizada para o parceiro Walmart.

**Despois de configurado, os itens que forem indexados serão enviados automaticamente para o Walmart!**


##Sobre a integração

Após as configuração acima forem concluídas, será necessário reindexar a base para que todos os itens sejam integrados corretamente.

Para isso, siga os passos abaixo:

* Com o usuário master, entre no link [nomedasualoja].vtexcommercestable.com.br/admin/Site/FullCleanUp.aspx
* Clique no botão "Reidexar Base"


![Campos Cnova](reindexacao.png)

> Observações: É possível acompanhar quantos produtos estão na fila acessando o módulo de E-Commerce. Lá existe vários dashboards que mostram: o total indexado, itens ainda não enfileirado, total na fila, na fila recentemente, entre outros.


Os itens passarão por um processo de catalogação até serem de fato disponibilizados no Marketplace. Esse prazo é variável de acordo com a demanda que o MarketPlace tiver. À medida que forem exportados, serão informados os preços e itens disponíveis em estoque de cada produto. Toda vez que houver uma alteração no preço de um produto, as informações serão atualizadas junto ao Marketplace. As atualizações relacionadas aos itens em estoque funcionam online toda vez que tiverem 5 unidades ou menos em estoque. Movimentações de estoques diferente dessa regra, não é indexada.

> Observações: O Walmart é o responsável pela integração dos Pedidos.

---

Autor: _Ana Izabel Jacobsen_</br>
Propriedade: _VTEX_ &copy;</br>
