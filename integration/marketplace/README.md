## Marketplaces and Sellers

E-commerce model between Marketplace and Seller, may stores are hosted or not on VTEX platform. Sales between Marketplace and Seller involve a negotiation between the two parts, the owner of the product (the Seller) and the owner of the showcase (the Marketplace). This negotiation usually involves commercial policy of traded items (SKUs) and maintenance of payment. The VTEX besides offering integrations with the biggest market players, also offers a set of APIs and instructions to third parties integrate themselves. 


> _Seller_ - Responsible for accomplish the order's delivery (the owner of the product).</br>
> _SKU_ - Defines a variation of a product, Example: 110V white refrigerator.</br>
> _Marketplace_ - Showcase where the SKUs are exposed (the owner of the showcase).</br>
> _SLA_ - Service agreement, Example: deadline and cost of delivery.</br>
> _API_ - Service interfaces for communication between computer systems.</br>

### Support tools to the Integrator

#### POSTMAN

Chrome browser extension, in this tool is possible to test, store history, save access requests collections from all APIs of the VTEX modules (OMS, Logistics, Pricing, GCS, Catalog, PCI Gateway, etc), besides it allows to export and import requests from other developers.

#### VTEX Bridge

This tool allows to simulate every requested method in the VTEX Lab documentation, simulating the request in a real store. 
[sandboxintegracao](http://sandboxintegracao.vtexcommercestable.com.br).

<a title="VTEX Bridge" href="http://bridge.vtexlab.com.br/" target="_blank">[Developer] - VTEX Bridge</a>

It's very important the integrator get the knowledge of this kind of tools, or other similar tools before start an integration process using the REST VTEX APIs.


### Credentials of access on the REST APIs


Every call made to REST APIs from VTEX must be made in “https” and must have in its header the authentication keys. The authentication keys of the header (to access the REST APIs) must be requested to the store administrator or to the operations team that is deploying the store.

> WARNING
>> An exclusive key and token must be created to each integrator, a key from the own store mustn't be used.

- - -

### Find below the existing models:

#### Seller Selling in Marketplace Hosted on VTEX Platform


This model allows a seller not hosted on VTEX platform, exhibit and sell their products (SKUs) in Marketplaces hosted on VTEX platform. To implement this model, the Seller shall exhibit some endpoints that will be called by the Marketplace and call some endpoints exposed by the Marketplace, following the VTEX protocol. In this model the Marketplace controls all the payment process.  Find the instructions to do this integration below:

[[Guide] Seller Selling in Marketplace Hosted on VTEX Platform](./seller-non-vtex/index.html)

- - -

#### Seller Selling in Marketplace Hosted on VTEX Platform and Processing the Payment


This model allows a seller not hosted on VTEX platform, exhibit and sell their products (SKUs) in Marketplaces hosted on VTEX platform. To implement this model, the Seller shall exhibit some endpoints that will be called by the Marketplace and call some endpoints exposed by the Marketplace, following the VTEX protocol. In this model the Marketplace sends payment data to be processed at the Seller. Following are the instructions to do this integration.

[[Guide] Seller Selling in Marketplace Hosted on VTEX Platform and Processing the Payment](./seller-non-vtex-with-payment/index.html)

- - -

#### Marketplace Selling to Seller Hosted on VTEX Platform


This model allows a seller not hosted on VTEX platform, exhibit and sell their products (SKUs) from Sellers hosted on VTEX platform. To implement this model, the Marktplace shall exhibit some endpoints that will be called by the Seller and call some endpoints exposed by the Seller, following the VTEX protocol. In this model the Marketplace controls all the payment transaction. Following are the instructions to do this integration.

[[Guide] Marketplace Selling to Seller Hosted on VTEX Platform](./marketplace-non-vtex/index.html)

- - -

#### Marketplace Selling to Seller Hosted on VTEX Platform Sending Payment

This model allows a marketplace not VTEX exhibit and sell products (SKUs) from Sellers hosted on VTEX platform. To implement this model, the Marktplace shall exhibit some endpoints that will be called by the Seller and call some endpoints exposed by the Seller, following the VTEX protocol. In this model the Marketplace creates the payment transaction in the Seller. Following are the instructions to do this integration.

[[Guide] Marketplace Selling to Seller Hosted on VTEX Platform Sending Payment](./marketplace-non-vtex-with-payment/index.html)

- - -

#### Marketplace Hosted on VTEX Selling to Sellers Hosted on VTEX and Vice Versa


By default the VTEX platform offers this possibility, through a simple configuration in the Seller, (creation of the affiliated relating it to a commercial policy) and another configuration in the Marketplace (creation of Seller).

- - -


## Beta Version

This documentation version supports integration in VTEX smartcheckout platform version. It was written to assist an integrator. The idea is to avoid any doubt of how to integrate with the VTEX. If you received this documentation and still have remaining questions, please detail your doubts on the community, then we can reach a rich and functional document.

---

Autor: _Jonas Bolognim_</br>
Propriedade: _VTEX_ &copy;</br>
