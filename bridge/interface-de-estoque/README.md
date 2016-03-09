# Interface de Estoque
Teremos aqui toda a relação de Sucessos e de Erros que ocorrem nas integrações dos **Estoque** desde a VTEX para o respectivo Marketplace. Além da visualização dos erros e sucessos é possível reprocessá-los manualmente pela própria interface ou limpar essa informação.


**Importante**: Quando houver **apenas alterações do Estoque** no SKU na VTEX, entrará na seguinte regra de que, só serão consideradas mudanças a serem exportadas, onde o saldo "De" ou saldo "Por" for menor ou igual a 5 unidades: [Atualização de Estoque entre Marketplace e VTEX](http://help.vtex.com/hc/pt-br/articles/206572797-Atualiza%C3%A7%C3%A3o-de-Estoque-entre-Marketplace-e-VTEX)

> Lembrando que, as movimentações entre a VTEX e os Marketplaces atuam por uma interface Web que possui protocolo HTTP que impede a ter uma unidade de tempo definida para o reflexo no Parceiro. Identificaremos como anormal, aqueles atualizações que levaram mais de 1 (uma) hora após a alteração.

## Visão geral

![Estoque - Visão geral](V_visaogeral_estoque.gif)

## Estoque integrado com sucesso
Aqui teremos o controle dos estoque que integraram com sucesso no matketplace desde a VTEX. Como é um registro de sucesso, é um dado apenas para métrica, onde, a única ação possível sobre este é excluir esse registro de sucesso que serve unicamente para limpar esse registro na tela.
Exemplo:

![Remover Pedido](V_estoque_remoçao_sucesso.gif)

Para excluir o registro, primeiro clique na linha do estoque. Será ilustrado em seguida um box de nome Ações. Abra essa combo e selecione a única opção disponível de "Remover Log".

## Estoque não integrado com erro

AAqui teremos o controle dos estoque que não integraram no marketplace desde a VTEX por algum erro. Como é um registro de erro, nesta tela conseguiremos tomar ações sobre o esses.
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
