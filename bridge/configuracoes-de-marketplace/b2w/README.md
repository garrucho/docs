# Configurações iniciais com a  B2W

## Primeiros Passos


> A B2W **não** precisa de contratação de Channel.

1. Caso deseje usar a mesma politica (Ou seja, as mesmas regras para sortimento, promoções e condições de entrega da loja) use a politica comercial "1".
Caso contrario leia este link [Seleção de política comercial.](http://help.vtex.com/hc/pt-br/articles/214166227)

2. Caso sua politica comercial seja diferente de 1: [Configure a logística para essa política comercial.](http://help.vtex.com/hc/pt-br/articles/214166667-Atualiza%C3%A7%C3%A3o-de-estoque)

3. Entrar em contato com a B2W e obtenha o Token de acesso a API:

 http://www.submarino.com.br/marketplace

## Bridge - Visão geral


>**Importante: As configurações realizadas no Bridge para B2W realiza a criação automatica do Afiliado no OMS, libera o Perfil de Acesso no E-Commerce e reindexa a base automaticamente**

###Exemplo de acesso as Configurações de Marketplace

![Config B2W](V_newconfig_b2w.gif)

##Campos

![Campos B2W](b2wcampos.png)

>>Para cada campo temos:

* **Integração de Produtos**: Liga ou Desliga a exportação de Produtos.
* **Integração de Pedidos**: Liga ou Desliga a exportação de Pedidos.
* **Integração de Preço**: Liga ou Desliga a exportação de Preços.
* **Integração de Estoques**: Liga ou Desliga a exportação de Estoque.
* **Id do Afiliado**: ID do Afiliado da B2W que irá ser cadastrado no OMS.
* **Política Comercial**: ID da Política Comercial que será utilizada para o parceiro B2W.
* **ApiKey**: Chave de identificação da sua Loja no Marketplace fornecida diretamente pela B2W.
* **Estrutura Mercadológica Genérica**:
Quando essa função estiver ativa, o produto será enviado na categoria 9999 da B2W, caso o EAN (codigo de barra) cadastrado no SKU seja inválido ou  nulo, não o enviaremos.
* **Concatenar nome de Produto com nome do SKU**: Quando ativada essa função, a exportação dos SKUs para a B2W terá um tratamento onde o titulo do SKU será formado pelo nome do produto acrescido do nome do SKU (Exemplo : Sapato Social Nike - 38, onde o Sapato Social Nike é o nome do produto e  o 38 numero do SKU).
* **Exportar catálogo como produtos únicos**: Quando ativada essa opção todos os SKUS serão enviados como produtos, ou seja, não haverá agrupamento de produto.
* **Taxa da Divergência de Preço**: Taxa de tolerancia de diferença de preço entre a Vtex e o Parceiro nos Pedidos integrados [padrão: até 10% de diferença].
* **Mapeamento de Atributos (Nome B2W -> Nome VTEX)**: serve para lojistas que possuem suas especificações diferentes da B2W (Marca, tamanho, etc.).No primeiro campo deve-se colocar o nome da especificação, na B2W, que deseja mapear. No segundo campo deve colocar o nome da especificação aqui na VTEX. Exemplo: Tamanho -> TAM. Lembrando que atualmente a B2W só aceita duas especificações, COR e TAMANHO. Qualquer outro mapeamento será ignorado.
* **E-mails Para Notificação de Pedidos Não Integrados**: Caso um Pedido sofra algum erro no ato de integrar na VTEX, será enviado um email com sua possível causa para o(s) email(s) aqui cadastrado(s).
