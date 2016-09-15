##Integration with VTEX platform

### Integration of ERPs with VTEX stores

[[Guide] Integration of ERPs with VTEX stores Model](./erp/index.html)

### Integration of Markeplaces with VTEX stores

[[Guide] Integration of Markeplaces with VTEX stores Model](./marketplace/index.html)

###Support tools to the integrator

####POSTMAN

Chrome browser extension, in this tool is possible to test, store history, save access requests collections from all APIs of the VTEX modules (OMS, Logistics, Pricing, GCS, Catalog, PCI Gateway, etc), besides it allows to export and import requests from other developers.

####VTEX Bridge

This tool allows to simulate every requested method in the VTEX Lab documentation, simulating the request in a real store.
[sandboxintegracao](http://sandboxintegracao.vtexcommercestable.com.br).

<a title="VTEX Bridge" href="http://bridge.vtexlab.com.br/" target="_blank">[Developer] - VTEX Bridge</a>

It's very important the integrator get the knowledge of this kind of tools, or other similar tools before start an integration process using the REST VTEX APIs.


###Credentials of access on the REST APIs


Every call made to REST APIs from VTEX must be made in “https” and must have in its header the authentication keys. The authentication keys of the header (to access the REST APIs) must be requested to the store administrator or to the operations team that is deploying the store.

> WARNING
>> An exclusive key and token must be created to each integrator, a key from the own store mustn't be used.


> SOME CONCEPTS:
>> _ERP_ - (Enterprise Resource Planning) Controls the data and processes of a company in a single system.</br>
>> _SKU_ - Defines a variation of a product, Ex. 110V white refrigerator.</br>
>> _Marketplace_ - showcase where the SKUs (owners of the showcase) are showed.</br>
>> _SLA_ - Service agreement, Ex: deadline and cost of delivery.</br>
>> _API_ - Service interface to communication between the computer systems.</br>

---

Autor: _Jonas Bolognim_</br>
Propriedade: _VTEX_ &copy;</br>
