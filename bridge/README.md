#  VTEX Bridge
O **VTEX Bridge** é o módulo responsável por intermediar e gerenciar todas as comunicações entre a sua loja VTEX e os Marketplaces. Nele você controla os possíveis erros e os sucessos que ocorrer nas diversas interfaces de **Pedidos**, **Produtos**, **Preço**, **Estoque** e **Tracking**. Além de poder tratar esses erros e reprocessar você mesmo a reintegração na interface que se encontrar. Também terá as configurações que ativam essa integração diretamente em sua Loja.

>>As movimentações que serão registradas no VTEX Bridge serão apenas para aqueles Marketplaces que hoje integramos de forma **ativa**. Isto é, onde nós quem realizamos as exportações de Produtos, Preços e Estoque e/ou onde nós campturamos os Pedidos realizados no Marketplace. Os Marketplaces que temos hoje com a integração ativa são: B2W, CNova, Rakuten, Buscapé e Walmart (apenas na parte de Protudos, Preços e Estoque. Pedidos o próprio Walmart quem insere na VTEX).

## Visão geral

![VTEX Bridge](VTEX Bridge3.png)

Nas diversas interfaces do VTEX Bridge teremos as coluna de **Código**, **Afiliado**, **Status** e **Data de Processamento** indicando diferentes informações sobre os registros que lhe auxiliarão a melhor controlora-los:

![Colunas de informações](V_pedidos_colunas.png)
>Para cada uma temos:

**Código**: Indica o código do Pedido no Marketplace ou do ID do SKU na VTEX cujo o qual está relacionado o erro ou o sucesso do registrado no VTEX Bridge.

**Afiliado**: Indica qual o Marketplace que originou o respectivo registro.

**Status**: Indica o atual status do registro, podendo variar entre sucesso, erro e em processamento.

**Data de Processamento**: Indica quando a captura do sucesso ou do erro foi realizada. Isto é, por exemplo, para os pedidos com sucesso, será essa a data que indicará a criação do Pedido.

## Configurações de Marketplaces
Temos hoje integrações de caráter nativo, isto é, onde parte da VTEX a exportação do Catálogo e o consumo dos Pedidos desde o Parceiro, com os Marketplaces da **B2W**, da **CNova**, do **Walmart** (apenas para catálogo) e o da **Rakuten**:

![Configurações de Marketplaces](V_newconfig.gif)

Onde cada um destes terá um formulário de configuração com suas ações, funções, particularidades e dados que precisam ser fornecidos para ativa-las na seguin. Por exemplo na seguinte estrutura de configuração da **CNova**:

![Configuração Bridge](V_newconfig_cnova.gif)

## Combinações de Filtros

Os filtros do VTEX Bridge lhe permitem agrupar os registros de Pedidos por apenas os que deram **erro**, por apenas o que integraram com **sucessos** e por aquele que todavia encontram-se em processamento. Além também de poder combinar esse agrupamento com a origem de qual **Marketplace** refere-se o registro:

![Filtnnado Pedido](V_Pedidos_Filtro.gif)

## Buscas Específicas

AS buscas feitas no VTEX Bridge estão, em cada interface, relacionadas com o ID do **Pedido** no Marketplace para as Interfaces de **Pedidos** e de **Tracking**, e relacionadas com o ID do **SKU** para as Interfaces de **Produtos**, de **Preço** e de **Estoque**.

![Ralizando busca](V_newsearch.gif)

Caso queira limpar a busca e voltar para a listagem que estava anteriomente, basta clicar no "**X**" ao lado direito do campo de Busca.

##  Análise e Reprocessamento de registros
Todas as integrações, tanto de Produto, Preço, Estoque e Tracking, quanto do consumo dos Pedidos junto ao Marketplace que obtiverem sucesso ou não, estarão registradas e ilustradas para você poder atuar em suas respectivas interfaces. Caso a integração não tenha ocorrido por conta de um erro, este possuirá opções de reprocessamento após seu tratamento para seu devido reenvio para o Parceiro:

![Reprocessando erro](V_produto_removendo_erro.gif)

##Começando a usar

Criado para oferecer a flexibilidade máxima em relação às Configurações de Marketplace e controle do fluxo de integração tanto do Catálogo quanto de Pedidos, o VTEX Bridge também fornece a possibilidade de reprocessar eventuais erros que podem surgir durante essa comunicação com esses Parceiros. Entenda cada um deles:

* [Configurações de Marketplaces](configuracoes-de-marketplace\README.md)

* [Interface de Pedidos](./interface-de-pedidos/README.md)

* [Interface de Produtos](interface-de-produto\README.md)

* [Interface de Tracking](interface-de-tracking\README.md)

* [Interface de Preço](interface-de-preco\README.md)

* [Interface de Estoque](interface-de-estoque\README.md)
