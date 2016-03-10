# Interface de Estoque
Teremos aqui todas as relações de sucessos e de erros que ocorrem nas integrações de **Estoque**, desde a VTEX até o respectivo Marketplace. Além da visualização dos erros e sucessos é possível reprocessá-los manualmente pela própria interface ou limpar essa informação.


**Importante**: Quando houver **apenas alterações de Estoque** do SKU na VTEX, a seguinte regra será aplicada: Só serão consideradas mudanças a serem exportadas, onde o saldo "De" ou saldo "Por" for menor ou igual a 5 unidades.

[Atualização de Estoque entre Marketplace e VTEX](http://help.vtex.com/hc/pt-br/articles/206572797-Atualiza%C3%A7%C3%A3o-de-Estoque-entre-Marketplace-e-VTEX)

> Lembrando que, as movimentações entre a VTEX e os Marketplaces atuam por uma interface Web, que possui protocolo HTTP, impedindo de ter uma unidade de tempo definida para o reflexo no Parceiro. Identificaremos como anormal, aquelas atualizações que levaram mais de 1 (uma) hora após a alteração.

## Visão geral

![Estoque - Visão geral](V_visaogeral_estoque.gif)

## Estoque integrado com sucesso

Aqui temos o controle dos estoque que integraram com sucesso no maketplace. Por ser um registro de sucesso, apenas uma única ação é possível, a de excluir registro. Essa ação permite limpar esse registro da tela.
Exemplo:

![Remover Pedido](V_estoque_remoçao_sucesso.gif)

Para excluir o registro, primeiro clique na linha do estoque. Em seguida clique no box de nome Ações. Uma nova opção chamada "Remover Log" irá surgir, clique nela.

## Estoque não integrado com erro

Aqui teremos o controle dos estoque que não integraram no marketplace desde a VTEX por algum erro. Como é um registro de erro, nesta tela conseguiremos tomar ações sobre o esses.
Para isso, basta clicar no mesmo registro para visualizar a origem que levou a este não ser integrado.
Exemplo:

![Visão geral Bridge](V_analiseerro_estoque.gif)


## Reprocessamento de Estoque

>> Atenção: O reprocessamento não necessariamente consegue mudar o status para "sucesso". A ação de reprocessar também não é instantânea, ela demora para ser concluida pois é colocada em uma fila de ações.



Para solicitar o reprocessamento, clique no pedido sinalizado com erro, abra a combo da ações posicionada à esquerda do registro e selecione "Reprocessar".
Exemplo:

![Visão geral Bridge](V_reprocessandoerro_estoque.gif)

## Remover Log

Essa ação nada mais é do que retirar o log da interface, a fim de "limpar" a tela. Para remover o log, clique sobre a linha e em seguida no botão "Ações" e então em "Remover Log". Esta ação pode ser feita nas integrações com ambos os status (sucesso e erro).
Exemplo:

![Visão geral Bridge](RemoverLog_Estoque.gif)
