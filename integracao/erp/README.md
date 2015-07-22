##Integração de ERPs com lojas VTEX


A integração de ERPs com lojas VTEX é realizada através de webservice (SOAP:XML), e API REST(JSON). O webservice VTEX deve ser usado o mínimo possível para os processos de integração. Hoje com exceção do **Catálogo**, que está com sua API REST em desenvolvimento, todos os outros módulos da VTEX possuem APIs REST bem definidas e de alta performance.

É altamente recomendado que se use as APIs REST nos módulos que não seja o **Catálogo**.

Abaixo seguem os modelos propostos:

###Integração Rápida de Catálogo, Preço e Estoque


Uma integração expressa, somente com os fluxos básicos e necessários, saiba mais em:
[[Guide] Integração Rápida de Catálogo e Preço e Estoque](http://vtex.github.io/docs/integracao/erp/catalogo-expresso/index.html)

###Integração Completa de Catálogo, Preço e Estoque


Uma integração completa, a maioria dos fluxos, envolve mais esforço na implementação, saiba mais em:
[[Guide] Integração Completa de Catálogo e Preço e Estoque](http://vtex.github.io/docs/integracao/erp/catalogo-completo/index.html)

###Integração de Pedido, Nota Fiscal e Tracking


O ERP busca uma lista de pedidos prontos para o ERP, busca cada pedido na VTEX e insere no ERP, avisa a VTEX que o pedido está no ERP. Depois o ERP envia Nota Fiscal e Tracking para a VTEX, saiba mais em:
 [[Guide] Integração de Pedido, Nota Fiscal e Tracking](http://vtex.github.io/docs/integracao/erp/pedido-e-tracking/index.html)

### Pooling


O envio ou consumo de dados num processo de integração deve ser executado somente quando necessário, ou seja, o dado só deve ser enviado do ERP para a plataforma VTEX quando ele realmente for alterado.

É aconselhado **NÃO** fazer uma integração que varre entidades inteiras do ERP e atualiza todos os dados na plataforma VTEX de tempos  em tempos. Além de consumir e processar dados desnecessariamente, isso não funcionaria para lojas com mais de 5 mil SKUs no catálogo.


### Ferramentas de Apoio ao Integrador


Recomendamos algumas ferramentas que são de extrema importância para qualquer integrador:

####soapUI >=2.5.1

Esta ferramenta é muito importante no processo de integração, pois ela permite simular os métodos do webservice,
gerando automaticamente o request XML.

Nesta ferramenta pode se fazer as chamadas para as APIs REST também.

####POSTMAN
Extensão do navegador chrome, nesta ferramenta pode se testar, armazenar histórico, salvar coleções de requests do acesso de todas as APIs dos modulos VTEX  (OMS, Logistics, Pricing, GCS, etc).

É de suma importância que o integrador tenha o conhecimento de ferramentas desse tipo, ou outras parecidas, antes de inciar um processo de integração usando webservice SOAP ou APIs REST VTEX.

###Credenciais Para Acesso (webservice e API REST)

Toda chamada feita ao web service deve ser em "https" e um usuário e senha são exigidos para iniciar a conexão. As chamadas para as APIs REST também devem ser feitas em "https" e devem conter em seu header as chaves de autenticação .Tanto o usuário e senha (para acessar o webservice) e chaves de autenticação do header(para acessar as APIs REST) devem ser solicitados ao suporte VTEX, ou ao time de operações que está implantando a loja. Para fins de rastreabilidade, cada ERP deve possuir a sua chave de acesso exclusiva.

O webservice VTEX trabalha com esquema de escalonamento na demanda, podendo assim escalar novas máquinas de acordo com a demanda de acesso. Por esse motivo, clients instalados em sistemas operacionais que fazem caches longos de DNS não são suportados (Windows XP, Windows Server 2003, versões antigas de Apache.


---

Autor:_Jonas Bolognim_
Propriedade:_VTEX_
