---
layout: docs
title: Considerações Marketplace VTEX
application: marketplace
docType: guide
---

## Marketplace e Sellers VTEX e Não VTEX
Modelo de venda entre Marketplaces* e Sellers* VTEX e não VTEX. Vendas entre Marketplace e Seller envolvem uma negociação entre as duas pontas (dono do produto e dono da vitrine). A VTEX além de oferecer as integrações com grandes players do mercado nativamente, também oferece uma conjunto de APIs e instruções para que terceiros se integrem. Segue abaixo os modelos existentes:

- - -

###Sellers Não VTEX Vendendo em Marketplace VTEX
{: #1 .slug-text}

Conjunto de instruções e exemplos que permitem um Seller Não VTEX expor e vender os seus produtos (SKUs)* em Marketplaces hospedados na VTEX, através de chamadas e exposição de APIs* seguindo o protocolo VTEX. Neste modelo o Marketplace controla todo o pagamento.

[[Guide] Sellers Não VTEX Vendendo em Marketplace VTEX](http://lab.vtex.com/docs/integracao/guide/marketplace/seller-n%C3%A3o-vtex/index.html)

- - -

###Sellers Não VTEX Vendendo em Marketplace VTEX Recebendo Pagamento
{: #2 .slug-text}

Conjunto de instruções e exemplos que permitem um Seller Não VTEX expor e vender os seus produtos (SKUs)* em Marketplaces hospedados na VTEX através de chamadas e exposição de APIs* seguindo o protocolo VTEX. Neste modelo o Marketplace envia o pagamento para ser processado no Seller.

[[Guide] Sellers Não VTEX Vendendo em Marketplace VTEX Recebendo Pagamento](http://lab.vtex.com/docs/integracao/guide/marketplace/seller-n%C3%A3o-vtex-com-pgto/index.html)

- - -

###Marketplace Não VTEX vendendo para Sellers VTEX 
{: #3 .slug-text}

Conjunto de instruções e exemplos que permitem um Marketplace Não VTEX expor e vender os produtos (SKUs) de Sellers hospedados na VTEX, através de chamadas e exposição de APIs seguindo o protocolo VTEX. Neste modelo o Marketplace controla toda a transação de pagamento.

[[Guide] Marketplace Não VTEX vendendo para Sellers VTEX](http://lab.vtex.com/docs/integracao/guide/marketplace/canal-de-vendas-nao-vtex/index.html)

- - -

###Marketplace Não VTEX vendendo para Sellers VTEX Com Pagamento 
{: #3 .slug-text}

Conjunto de instruções e exemplos que permitem um Marketplace Não VTEX expor e vender os produtos (SKUs) de Sellers hospedados na VTEX, através de chamadas e exposição de APIs seguindo o protocolo VTEX. Neste modelo o Marketplace cria a transação de pagamento no Seller.

[[Guide] Marketplace Não VTEX vendendo para Sellers VTEX Com Pagamento](http://lab.vtex.com/docs/integracao/guide/marketplace/canal-de-vendas-nao-vtex-com-pgto/index.html)

- - -

###Marketplace VTEX vendendo para Sellers VTEX e Vice Versa
{: #4 .slug-text}

Conjunto de instruções que permitem um Seller VTEX expor e vender os seu produtos (SKUs) em um Marketplace VTEX, e vice verso,  através de simples configurações em ambas as pontas.

- - -

###Ferramentas de Apoio ao Integrador
{: #5 .slug-text}


####POSTMAN

Extensão do navegador chrome, nesta ferramente pode se testar, armazenar histórico, salvar coleções de requests do acesso de todas as APIs dos modulos VTEX (OMS, Logistics, Pricing, GCS, Catalog, PCI Gateway etc), além de permitir exportar e importar requests de outros desenvolvedores. 

####VTEX Bridge

Essa ferramenta permite simular todos os metodos solicitados nas documentações do VTEX Lab, simulando request em uma loja real.  
[sandboxintegracao](https://sandboxintegracao.vtexcommercestable.com.br).

<a title="VTEX Bridge" href="http://bridge.vtexlab.com.br/" target="_blank">[Developer] - VTEX Bridge</a>

É de suma importancia que o integrador tenha o conhecimento de ferramentas desse tipo, ou outras parecidas, antes de inciar um processo de integração usando as APIs REST VTEX.

- - -

###Credenciais Para Acesso  as API REST  
{: #6 .slug-text}

Toda chamada feita as APIs REST da VTEX devem ser feitas em "https" e devem conter em seu header as chaves de autenticação .As chaves de asutenticação do header(para acessar as APIs REST) devem ser solicitados ao administrador da loja, ou ao time de operaçãoes que está implantando a loja.

- - -

###Glossário

Seller* - Responsável por fazer a entrega do pedido.  
SKU* - Define uma variação de um produto, Ex. geladeira branca 110V.  
Marketplace* - Vitrine onde se expõe os SKUs (produtos).  
SLA* - Acordo de serviço, Ex. prazo e custo de entrega.  
API* - Interfaces de serviço para comunicação entre sistemas de computador.

- - -

###Versão Beta
Essa versão de documentação suporta a integração na versão da plataforma VTEX smartcheckout. Ela foi escrita para auxiliar um integrador. A idéia e que através dela, não  restem nenhuma dúvida de como se integrar com a VTEX. Se recebeu essa documentação e ainda restaram dúvidas, por favor, detalhe as suas dúvidas na comunidade, para chegarmos a um documento rico e funcional.

---

Autor: _Jonas Bolognim_  
Propriedade: _VTEX_ &copy;