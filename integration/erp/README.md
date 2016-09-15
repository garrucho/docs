## Integration of ERPs with VTEX stores


The integration of ERPs with VTEX stores is made through a webservice (SOAP: xml) and the REST API (JSON). The VTEX webservice must be used as less as possible to the integration processes. Currently, on exception the catalog, that has its REST API being developed, all the other VTEX modules have REST APIs well defined and with high performance.

It's highly recommended the usage of the REST APIs in the modules that isn't the **Catalog**.


### Fast Integration of Catalog, Price and Inventory


An express integration with basic and necessary flows, find out more at:
[[Guide] Fast Integration of Catalog, Price and Inventory](./express-catalog/index.html)

### Complete Integration of Catalog, Price and Inventory


A complete integration, most flows, involve more effort on the implementation, find out more at:
[[Guide] Complete Integration of Catalog, Price and Inventory](./complete-catalog/index.html)

### Order, Invoice and Tracking Integration 


The ERP, searches in a list of ready orders to the ERP, searches each order on VTEX and insert it in the ERP, notify the VTEX that the order is in the ERP. After the ERP sends the invoice and Tracking to the VTEX, find out more at:
 [[Guide] Order, Invoice and Tracking Integration ](./order-and-tracking/index.html)

### Pooling


The sending or data consumption in an integration process must be executed only when it's necessary, what means that the data only must be sent from the ERP to the VTEX platform when it really got changed.

It's advised do not make an integration that scans entire entities from the ERP and updates every data from the VTEX platform periodically. Besides it consumes and processes data unnecessarily, it wouldn't work to the stores with more than 5 thousand SKUs in the catalog.



### Support Tools to the Integrator

We recommend some tools that are too important to any integrator:

#### soapUI >=2.5.1

This tool is very important in the integration process, because it allows simulate the webservice methods, generating the XML request automatically. In this tool is possible to do requests to the REST APIs too.


#### POSTMAN

Chrome browser extension, in this tool is possible to test, store history, save access requests collections from all APIs of the VTEX modules (OMS, Logistics, Pricing, GCS, etc). 

It's very important the integrator get the knowledge of this kind of tools, or other similar tools before start an integration process using SOAP webservice or REST VTEX APIs.

### Credentials Access (webservice and REST APIs)

Every call made to the webservice must be in “https” and a username and password are required to start the connection. The calls to the REST APIs must be made in “https” and must have in its header the authentication keys.

How to set the users to the webservice:
http://help.vtex.com/hc/pt-br/articles/210821398-Como-configurar-o-acesso-ao-webservice

How to set users to the REST API:
http://help.vtex.com/hc/pt-br/articles/215110697

For traceability purposes, each ERP must have its key of exclusive access. The VTEX webservice works with scheduling scheme on demand, thus being able to scale new machines according to access demand. That's why clients installed on operating systems that do long caches of DNS are not supported (Windows XP, Windows Server 2003, old versions of Apache).

---

Autor:_Jonas Bolognim_
Propriedade:_VTEX_
