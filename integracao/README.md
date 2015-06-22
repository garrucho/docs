## Integração com a plataforma VTEX

### Integração com os ERPs

[[Guide] Modelo de integrações com ERPs](http://vtex.github.io/docs/integracao/erp/index.html)

### Integração com os Markeplaces

[[Guide] Modelo de integrações com Markeplaces] (http://vtex.github.io/docs/integracao/marketplace/index.html)

###Ferramentas de Apoio ao Integrador

####POSTMAN

Extensão do navegador chrome, nesta ferramenta pode se testar, armazenar histórico, salvar coleções de requests do acesso de todas as APIs dos modulos VTEX (OMS, Logistics, Pricing, GCS, Catalog, PCI Gateway etc), além de permitir exportar e importar requests de outros desenvolvedores.

####VTEX Bridge

Essa ferramenta permite simular todos os metodos solicitados nas documentações do VTEX Lab, simulando request em uma loja real.
[sandboxintegracao](http://sandboxintegracao.vtexcommercestable.com.br).

<a title="VTEX Bridge" href="http://bridge.vtexlab.com.br/" target="_blank">[Developer] - VTEX Bridge</a>

É de suma importancia que o integrador tenha o conhecimento de ferramentas desse tipo, ou outras parecidas, antes de inciar um processo de integração usando as APIs REST VTEX.


###Credenciais Para Acesso nas API REST


Toda chamada feita as APIs REST da VTEX devem ser feitas em "https" e devem conter em seu header as chaves de autenticação. As chaves de autenticação do header(para acessar as APIs REST) devem ser solicitados ao administrador da loja, ou ao time de operaçãoes que está implantando a loja.

> ATENÇÂO
>> Deverá ser criada uma chave e token exclusivas para cada integrador, **não** se deve usar a chave da própria loja.


> ALGUNS CONCEITOS:
>> _ERP_ - (Entrerprise Resource Planning) Controlam os dados e processos de uma organização em um único sistema.</br>
>> _SKU_ - Define uma variação de um produto, Ex. geladeira branca 110V.</br>
>> _Marketplace_ - Vitrine onde se expõe os SKUs (o dono da vitrine)</br>
>> _SLA_ - Acordo de serviço, Ex. prazo e custo de entrega.</br>
>> _API_ - Interfaces de serviço para comunicação entre sistemas de computador.</br>

---

Autor: _Jonas Bolognim_</br>
Propriedade: _VTEX_ &copy;</br>
