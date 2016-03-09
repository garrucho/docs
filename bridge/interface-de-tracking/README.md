# Interface de Tracking
Teremos aqui toda a relação de **Tracking**, ou seja, após o Pedido já ter integrado, ocorrerá a integração da atualização do seu Status junto aos Marketplaces até seu Faturamento, além de possíveis erros que ocorrem nos envios destes novos Status. Também temos o controle desses erros ao passo de trata-los e reprocessá-los diretamente nessa própria interface.

> IMPORTANTE!

O Workflow dos tracking da VTEX seguem até o status de **Faturado**, onde é fornecida a Nota Fiscal e os dados de rastreio para os mesmos. Marketplaces que possuam Status além de Faturado, **que atendem ao processo logístico do Pedido** como Entregue à Transportador e Entregue (ao cliente final), precisarão do tratamento pela interface dos Parceiros.

## Visão geral

![Visão geral Tracking](V_visaogeral_tracking.gif)


## Tracking integrado com sucesso
Aqui teremos o controle dos tracking que integraram com sucesso no matketplace desde a VTEX. Como é um registro de sucesso, é um dado apenas para métrica, onde, a única ação possível sobre este é excluir esse registro de sucesso que serve unicamente para limpar esse registro na tela.
Exemplo:

![Visão de sucesso Tracking](V_analisesucesso_tracking.gif)

Para excluir o registro, primeiro clique na linha do tracking. Será ilustrado em seguida um box de nome Ações. Abra essa combo e selecione a única opção disponível de "Remover Log".

## Tracking não integrado com erro

AAqui teremos o controle dos tracking que não integraram no marketplace desde a VTEX por algum erro. Como é um registro de erro, nesta tela conseguiremos tomar ações sobre o esses.
Para isso, basta clicar no mesmo registro para visualizar a origem que levou a este não ser integrado.
Exemplo:

![Visão de erro Tracking](V_analiseerro_tracking.gif)


## Reprocessamento de Tracking

>> Atenção: O reprocessamento não necessariamente consegue mudar o status para "sucesso". A ação de reprocessar também não é instantânea, ela demora para ser concluida pois é colocada em uma fila de ações.



Para solicitar o reprocessamento, clique no pedido sinalizado com erro, abra a combo da ações posicionada à esquerda do registro e selecione "Reprocessar".
Exemplo:


![Visão de reprocessamento Tracking](V_reprocessandoerro_tracking.gif)

## Remover Log

Essa ação nada mais é do que retirar o log da interface, a fim de "limpar" a tela. Para remover o log, clique sobre a linha e em seguida no botão "Ações" e então em "Remover Log". Esta ação pode ser feita nas integrações com ambos os status (sucesso e erro).
Exemplo:

![Visão de remover Log](RemoverLog_tracking.gif)
