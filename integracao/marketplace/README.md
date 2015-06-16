## Marketplaces e Sellers

Modelo de e-commerce entre _Marketplace_ e _Seller_, podendo as lojas estarem hospedadas ou **não** na plataforma VTEX. Vendas entre Marketplace e Seller envolvem uma negociação entre as duas pontas, dono do produto (o Seller) e o dono da vitrine (o Marketplace). Essa negociação geralmente envolve política comercial dos itens comercializados (_SKUs_) e a manutenção do pagamento.</br>
A VTEX além de oferecer as integrações com os grandes players do mercado nativamente, também oferece um conjunto de _APIs_ e instruções para que terceiros se integrem.


> _Seller_ - Responsável por cumprir com a entrega do pedido (o dono do produto).</br>
> _SKU_ - Define uma variação de um produto, Ex. geladeira branca 110V.</br>
> _Marketplace_ - Vitrine onde se expõe os SKUs (o dono da vitrine)</br>
> _SLA_ - Acordo de serviço, Ex. prazo e custo de entrega.</br>
> _API_ - Interfaces de serviço para comunicação entre sistemas de computador.</br>


Seguem abaixo os modelos existentes:

####Seller Vendendo em Marketplace Hospedado na Plataforma VTEX


Este modelo permite a um Seller **não** hospedado na plataforma VTEX expor e vender os seus produtos (SKUs) em Marketplaces hospedados na plataforma VTEX. Para implementar esse modelo o Seller deverá expor alguns endpoints que serão chamados pelo Marketplace e chamar alguns endpoints expostos pelo Marketplace, seguindo o protocolo VTEX. Neste modelo o Marketplace controla todo processo de pagamento. Segue abaixo instruções para realizar essa integração.

[[Guide] Sellers Não VTEX Vendendo em Marketplace Hospedado na VTEX](http://lab.vtex.com/docs/integracao/guide/marketplace/seller-não-vtex/index.html)

- - -

####Seller Vendendo em Marketplace Hospedado na Plataforma VTEX e Processando o Pagamento


Este modelo permite a um Seller **não** hospedado na plataforma VTEX expor e vender os seus produtos (SKUs) em Marketplaces hospedados na plataforma VTEX. Para implementar esse modelo o Seller deverá expor alguns endpoints que serão chamados pelo Marketplace e chamar alguns endpoints expostos pelo marketplace, seguindo o protocolo VTEX. Neste modelo o Marketplace envia os dados de pagamento para serem processados no Seller. Segue abaixo instruções para realizar essa integração.

[[Guide] Sellers Não VTEX Vendendo em Marketplace VTEX Recebendo Pagamento](http://lab.vtex.com/docs/integracao/guide/marketplace/seller-não-vtex-com-pgto/index.html)

- - -

####Marketplace Vendendo para Seller Hospedado na Plataforma VTEX


Este modelo permite a um Marketplace **não** hospedado VTEX expor e vender os produtos (SKUs) de Sellers hospedados na plataforma VTEX. Para implementar esse modelo o Marketplace deverá expor alguns endpoints que serão chamados pelo Seller e chamar alguns endpoints expostos pelo Seller, seguindo o protocolo VTEX. Neste modelo o Marketplace controla toda a transação de pagamento. Segue abaixo instruções para realizar essa integração.

[[Guide] Marketplace Não VTEX vendendo para Sellers VTEX](http://lab.vtex.com/docs/integracao/guide/marketplace/canal-de-vendas-nao-vtex/index.html)

- - -

####Marketplace Vendendo para Seller Hospedado na Plataforma VTEX Enviando o Pagamento

Este modelo permite a um Marketplace **não** VTEX expor e vender os produtos (SKUs) de Sellers hospedados na plataforma VTEX. Para implementar esse modelo o Marketplace deverá expor alguns endpoints que serão chamados pelo Seller e chamar alguns endpoints expostos pelo Seller, seguindo o protocolo VTEX. Neste modelo o Marketplace cria a transação de pagamento no Seller. Segue abaixo instruções para realizar essa integração.

[[Guide] Marketplace Não VTEX vendendo para Sellers VTEX Com Pagamento](http://lab.vtex.com/docs/integracao/guide/marketplace/canal-de-vendas-nao-vtex-com-pgto/index.html)

- - -

####Marketplace Hospedado na VTEX Vendendo para Sellers Hospedado na VTEX e Vice Versa


Nativamente a plataforma VTEX oferece essa possibiidade, através de uma simples configuração no Seller, (criação do afiliado relacionado-o a uma política comercial) e outra configuração no Marketplace (criação do Seller).

- - -

##Ferramentas de Apoio ao Integrador


####POSTMAN

Extensão do navegador chrome, nesta ferramenta pode se testar, armazenar histórico, salvar coleções de requests do acesso de todas as APIs dos modulos VTEX (OMS, Logistics, Pricing, GCS, Catalog, PCI Gateway etc), além de permitir exportar e importar requests de outros desenvolvedores.

####VTEX Bridge

Essa ferramenta permite simular todos os metodos solicitados nas documentações do VTEX Lab, simulando request em uma loja real.
[sandboxintegracao](http://sandboxintegracao.vtexcommercestable.com.br).

<a title="VTEX Bridge" href="http://bridge.vtexlab.com.br/" target="_blank">[Developer] - VTEX Bridge</a>

É de suma importancia que o integrador tenha o conhecimento de ferramentas desse tipo, ou outras parecidas, antes de inciar um processo de integração usando as APIs REST VTEX.

- - -

##Credenciais Para Acesso nas API REST


Toda chamada feita as APIs REST da VTEX devem ser feitas em "https" e devem conter em seu header as chaves de autenticação. As chaves de autenticação do header(para acessar as APIs REST) devem ser solicitados ao administrador da loja, ou ao time de operaçãoes que está implantando a loja.

> ATENÇÂO
>> Deverá ser criada uma chave e token exclusivas para cada integrador, **não** se deve usar a chave da própria loja.

- - -


##Versão Beta
Essa versão de documentação suporta a integração na versão da plataforma VTEX smartcheckout. Ela foi escrita para auxiliar um integrador. A idéia é que através dela, não  restem nenhuma dúvida de como se integrar com a VTEX. Se recebeu essa documentação e ainda restaram dúvidas, por favor, detalhe as suas dúvidas na comunidade, para chegarmos a um documento rico e funcional.

---

Autor: _Jonas Bolognim_</br>
Propriedade: _VTEX_ &copy;</br>
