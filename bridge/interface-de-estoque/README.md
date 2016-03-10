# Interface de Estoque
Temos aqui todas as relações de sucessos e de erros que ocorrem nas integrações de **Estoque**, desde a VTEX até o respectivo Marketplace. Além da visualização dos erros e sucessos, é possível reprocessá-los manualmente pela própria interface ou limpar essa informação.


**Importante**: Quando houver **apenas alterações de Estoque** do SKU na VTEX, a seguinte regra será aplicada: Só serão consideradas mudanças a serem exportadas, onde o saldo "De" ou saldo "Por" for menor ou igual a 5 unidades.

Mais informações : [Atualização de Estoque entre Marketplace e VTEX](http://help.vtex.com/hc/pt-br/articles/206572797-Atualiza%C3%A7%C3%A3o-de-Estoque-entre-Marketplace-e-VTEX)


## Visão geral

![Estoque - Visão geral](V_visaogeral_estoque.gif)

## Estoque integrado com sucesso

Controlamos a visualização dos estoques que integraram com sucesso no maketplace. Por ser um registro de sucesso, apenas uma única ação é possível, a de excluir registro. Essa ação consiste em limpar o registro da tela.
Exemplo:

![Remover Pedido](V_estoque_remoçao_sucesso.gif)

Para excluir um registro, primeiro clique na linha do estoque. Em seguida clique no box de nome Ações. Uma nova opção chamada "Remover Log" irá surgir, clique nela.

## Estoque não integrado com erro

Controlamos a visualização dos estoque que não integraram no marketplace a VTEX por algum erro. Como é um registro de erro, nesta tela conseguiremos tomar mais ações sobre esses.
Para visualizar o motivo do erro, basta clicar no mesmo registro e visualizar a origem que o levou a não ser integrado.
Exemplo:

![Visão geral Bridge](V_analiseerro_estoque.gif)


## Reprocessamento de Estoque

>> Atenção: O reprocessamento não necessariamente consegue mudar o status do registro para "sucesso". A ação de reprocessar também não é instantânea, ela demora para ser concluida pois é colocada em uma fila de ações.

Para solicitar o reprocessamento, clique no registro sinalizado com erro, abra a combo de ações e selecione a opção "Reprocessar".
Exemplo:

![Visão geral Bridge](V_reprocessandoerro_estoque.gif)

## Remover Log

Essa ação nada mais é do que retirar o log da interface, a fim de "limpar" a tela.
Para remover o log, clique no registro, abra a combo de ações e selecione a opção "Remover Log". Esta ação pode ser feita nas integrações com ambos os status (sucesso ou erro).
Exemplo:

![Visão geral Bridge](RemoverLog_Estoque.gif)
