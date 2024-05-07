---
layout: default
title: API
parent: Documentation
nav_order: 1
---

<h2>Create an Internet On-Demand connection to a data center</h2><div class="-mb--2 -mt--5 -text--bold">On this page</div><ul class="-m--0 -p--0" style="list-style-type:none;"><li class="-mb--1"><a href="#create-a-connection">Create an Internet On-Demand connection</a></li><li class="-mb--1"><a href="#providers">Step 1: Find providers</a></li><li class="-mb--1"><a href="#location">Step 2: Browse locations</a></li><li class="-mb--1"><a href="#qualify">Step 3: Qualify</a></li><li class="-mb--1"><a href="#pricing">Step 4: Pricing</a></li><li class="-mb--1"><a href="#billing">Step 5: Find billing account</a></li><li class="-mb--1"><a href="#order">Step 6: Order</a></li><li class="-mb--1"><a href="#check-your-webhooks">Step 7: Check your webhooks</a></li></ul><div class="-mt--7 -text--h3" id="create-a-connection">Create an Internet On-Demand connection</div><p>Follow the steps below to create a Lumen Internet On-Demand connection to one of our partner data centers.</p><div class="-mt--5 -text--h3" id="providers">Step 1: Find providers</div><p>Send a GET request to the <code>/Network/v1/provider</code> endpoint to retrieve a list of providers or partners who connect with the Lumen network.</p><pre>GET /Network/v1/provider</pre><div class="-mt--5 -text--h5">Response</div><pre style="white-space:pre-wrap;">{
    "pagination": [
        {
            "pageNumber": 1,
            "pageSize": 1,
            "totalRecords": 2
        }
    ],
    "listPartners": [
        {
            "id": "43558",
            "name": "Equinix",
            "datacenterFabricUrl": "https://ecxfabric.equinix.com/",
            "partnerType": "CLOUD_PARTNER"
        },
        {
            "id": "362232",
            "name": "Digital Realty",
            "datacenterFabricUrl": "https://www.digitalrealty.com/platform-digital/connectivity/service-fabric/connect",
            "partnerType": "CLOUD_PARTNER"
        }
    ]
}
</pre><chi-alert class="-mt--3 -mb--3 sc-chi-alert-h sc-chi-alert-s hydrated" color="info" icon="circle-info" type="bubble" size="md"><!----><div class="chi-alert -bubble -info -md sc-chi-alert" role="alert"><chi-icon class="sc-chi-alert sc-chi-icon-h sc-chi-icon-s hydrated" color="info" icon="circle-info" extra-class="chi-alert__icon"><div class="chi-icon -icon--info chi-alert__icon sc-chi-icon"><svg xmlns="http://www.w3.org/2000/svg" xmlnsXlink="http://www.w3.org/1999/xlink" aria-hidden="true" class="sc-chi-icon"><use xlink:href="#icon-circle-info" class="sc-chi-icon"></use></svg></div></chi-icon><div class="chi-alert__content sc-chi-alert"><p class="chi-alert__text sc-chi-alert sc-chi-alert-s">
<p>In the response, an <code>id</code> is returned for each partner. This ID must be passed as the <code>partnerId</code> when you submit a GET request to the <code>/Network/v1/location</code> endpoint.</p>
</p></div></div></chi-alert><p class="-mb--4"><a href="#top">Back to top</a></p><div class="-mt--7 -text--h3" id="location">Step 2: Browse locations</div><p>Send a GET request to the <code>/Network/v1/location</code> endpoint to retrieve a list of NaaS-enabled locations on the Lumen network.</p><pre>GET /Network/v1/location</pre><div class="-mt--5 -text--h5">Parameters</div><p>The table below lists the required query parameter you must pass to complete this call successfully.</p><table class="chi-table -hover -mt--4"><thead><tr><th>Name</th><th>Type</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td style="white-space:nowrap;"><code>partnerId</code></td><td>string</td><td><p>The unique ID of a data center or cloud provider.</p><p>You can retrieve a list of providers and their partner information by making a GET request to the <code>/Network/v1/provider</code> endpoint.</p><p>&nbsp;</p></td><td><code>/Network/v1/location?partnerId=43558</code></td></tr></tbody></table><div class="-mt--5 -text--h5">Response</div><pre>{
    "pagination": [
        {
        "pageNumber": 1,
        "pageSize": 10,
        "totalRecords": 1021
        }
    ],
    "location": [
        {
        "masterSiteId": "PL0001247634",
        "streetAddress": "100 CenturyLink Dr",
        "city": "Monroe",
        "stateOrProvince": "LA",
        "locality": "string",
        "country": "US",
        "postCode": "71203",
        "postCodeExtension": "2041"
        }
    ],
    "networkStatus": "ON_NET",
    "naasEnabled": true,
    "partner": "Equinix Ethernet Exchange - Silicon Valley IBX",
    "market": "WASHINGTON DC",
    "productFamily": {
        "productFamily": "Internet On-Demand",
        "accessCapabilities": [
        {
            "accessCapability": [
            {
                "speed": "1000 mbps",
                "displayValue": "1 gbps",
                "circuitType": "Hosted-NNI Internet"
            }
            ]
        }
        ]
    }
}
</pre><p>Extract the <code>masterSiteId</code> from the response. Using the example response above, you would extract the ID by copying the highlighted <code>masterSiteId</code> string seen below:</p><pre>    {
        "location": [
            {
            "masterSiteId": "<mark>PL0001247634</mark>",
            "streetAddress": "100 CenturyLink Dr",
            "city": "Monroe",
            "stateOrProvince": "LA",
            "locality": "string",
            "country": "US",
            "postCode": "71203",
            "postCodeExtension": "2041"
            }
        ],
    }
</pre><chi-alert class="-mt--4 sc-chi-alert-h sc-chi-alert-s hydrated" color="info" icon="circle-info" type="bubble" size="md"><!----><div class="chi-alert -bubble -info -md sc-chi-alert" role="alert"><chi-icon class="sc-chi-alert sc-chi-icon-h sc-chi-icon-s hydrated" color="info" icon="circle-info" extra-class="chi-alert__icon"><div class="chi-icon -icon--info chi-alert__icon sc-chi-icon"><svg xmlns="http://www.w3.org/2000/svg" xmlnsXlink="http://www.w3.org/1999/xlink" aria-hidden="true" class="sc-chi-icon"><use xlink:href="#icon-circle-info" class="sc-chi-icon"></use></svg></div></chi-icon><div class="chi-alert__content sc-chi-alert"><p class="chi-alert__text sc-chi-alert sc-chi-alert-s">The <code>masterSiteId</code> is a required parameter when using the <code>/Product/v1/price</code> endpoint to request a location's available products and pricing information.</p></div></div></chi-alert><p class="-mb--4"><a href="#top">Back to top</a></p><div class="-mt--7 -text--h3" id="qualify">Step 3: Qualify</div><p>Send a GET request to the <code>/Product/v1/price</code> endpoint to retrieve a list of product offerings based on location.</p><pre>GET /Product/v1/price</pre><div class="-mt--5 -text--h5">Parameters</div><p>The table below lists the required query parameters you must pass to complete this call successfully.</p><table class="chi-table -hover -mt--4"><thead><tr><th>Name</th><th>Type</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td style="white-space:nowrap;"><code>masterSiteId</code></td><td>string</td><td>A unique server-side identifier for a network location.</td><td><code>/Product/v1/price?masterSiteId=PL0001247634</code></td></tr><tr><td style="white-space:nowrap;"><code>productCode</code></td><td>string</td><td><p>A unique product code ID used to identify the product to be priced.</p><p>The NaaS <code>productCode</code> is <code>718</code>.</p></td><td><code>/Product/v1/price?productCode=718</code></td></tr></tbody></table><div class="-mt--5 -text--h5">Response</div><pre>{
    "location": [
        {
        "masterSiteId": "PL0001247634",
        "streetAddress": "100 CenturyLink Dr",
        "city": "Monroe",
        "stateOrProvince": "LA",
        "locality": "string",
        "country": "US",
        "postCode": "71203",
        "postCodeExtension": "2041"
        }
    ],
    "pricePerSpeed": [
        {
        "speed": "100 Mbps",
        "price": "$0.46",
        "term": "Hourly Rate",
        "displayValue": "100 Mbps"
        }
    ]
}
</pre><p class="-mb--4"><a href="#top">Back to top</a></p><div class="-mt--7 -text--h3" id="pricing">Step 4: Pricing</div><p>Send a POST request to the <code>/Product/v1/priceRequest</code> endpoint to create a request for pricing based on product, product attributes, and location.</p><pre>POST /Product/v1/priceRequest</pre><chi-alert class="-mt--4 sc-chi-alert-h sc-chi-alert-s hydrated" color="warning" icon="warning" type="bubble" size="md"><!----><div class="chi-alert -bubble -warning -md sc-chi-alert" role="alert"><chi-icon class="sc-chi-alert sc-chi-icon-h sc-chi-icon-s hydrated" color="warning" icon="warning" extra-class="chi-alert__icon"><div class="chi-icon -icon--warning chi-alert__icon sc-chi-icon"><svg xmlns="http://www.w3.org/2000/svg" xmlnsXlink="http://www.w3.org/1999/xlink" aria-hidden="true" class="sc-chi-icon"><use xlink:href="#icon-warning" class="sc-chi-icon"></use></svg></div></chi-icon><div class="chi-alert__content sc-chi-alert"><p class="chi-alert__text sc-chi-alert sc-chi-alert-s"><p>This call returns a unique quote ID that is valid for <strong>15 minutes</strong>. While the quote ID is still valid, you must use the quote ID to promote the pricing quote to an order using the <code>/Customer/v3/Ordering/orderRequest</code> endpoint.</p>

<p class="-mt--2">You will need to generate a new quote if the quote is not promoted to an order within the 15-minute window. The unique quote ID follows this format: <code>"id": "NaaS-1466"</code></p></p></div></div></chi-alert><div class="-mt--5 -text--h5">Properties</div><p>The table below lists the required properties you must pass to complete this call successfully.</p><table class="chi-table -hover -mt--4"><thead><tr><th>Name</th><th>Type</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td style="white-space:nowrap;"><code>sourceSystem</code></td><td>string</td><td><p>The name used to identify the system type requesting prices for a product or service.</p><p>Enter <code>NaaS ExternalAPI</code> for this value to indicate a NaaS product or service price request.</p></td><td><code>"sourceSystem": "NaaS ExternalAPI"</code></td></tr><tr><td style="white-space:nowrap;"><code>customerNumber</code></td><td>string</td><td>A Lumen-defined customer ID, also known as a BusOrg number.</td><td><code>"customerNumber": "1-ABCDE"</code></td></tr><tr><td style="white-space:nowrap;"><code>currencyCode</code></td><td>string</td><td><p>A three-letter ISO currency code (not case-sensitive).</p><p>The US dollar is represented as USD. <code>USD</code> is the only supported currency code.</p></td><td><code>"currencyCode": "USD"</code></td></tr><tr><td style="white-space:nowrap;"><code>masterSiteId</code></td><td>string</td><td>A unique server-side identifier for a network location.</td><td><code>"masterSiteId": "PL0001247634"</code></td></tr><tr><td style="white-space:nowrap;"><code>partnerId</code></td><td>string</td><td>The data center or cloud provider ID.</td><td><code>"partnerId": "43558",</code></td></tr><tr><td style="white-space:nowrap;"><code>productCode</code></td><td>string</td><td><p>A unique product code ID used to identify the product to be priced.</p><p>The NaaS <code>productCode</code> is <code>718</code>.</p></td><td><code>"productCode": "718"</code></td></tr><tr><td style="white-space:nowrap;"><code>speed</code></td><td>string</td><td>The bandwidth available for a service.</td><td><code>"speed": "1000 Mbps"</code></td></tr></tbody></table><div class="-mt--5 -text--h5">Request body</div><pre>{
    "sourceSystem": "NaaS ExternalAPI",
    "customerPriceRequestDescription": "NaaS Price Request",
    "customerPurchaseOrderNumber": "PO-12345",
    "customerNumber": "1-ABCDE",
    "currencyCode": "USD",
    "masterSiteId": "PL0001247634",
    "productCode": "718",
    "partnerId": "43558",
    "productName": "Internet On-Demand",
    "speed": "100 Mbps"
}
</pre><div class="-mt--5 -text--h5">Response</div><pre>{
    "id": "NaaS-1466",
    "priceRequest": {
        "sourceSystem": "NaaS ExternalAPI",
        "customerPriceRequestDescription": "NaaS Price Request",
        "customerPurchaseOrderNumber": "PO-12345",
        "customerNumber": "1-ABCDE",
        "currencyCode": "USD",
        "masterSiteId": "PL0001247634",
        "productCode": "718",
        "productName": "Internet On-Demand",
        "speed": "100 Mbps",
        "partnerId": "43558"
    },
    "price": [
        {
            "speed": "100 Mbps",
            "price": "$0.46",
            "term": "Hourly Rate"
        },
        {
            "speed": "100 Mbps",
            "price": "$331.20000000000005",
            "term": "Monthly Rate"
        }
    ]
}
</pre><p class="-mb--4"><a href="#top">Back to top</a></p><div class="-mt--7 -text--h3" id="billing">Step 5: Find billing account</div><p>Send a GET request to the <code>/Account/v1/billingAccount</code> endpoint to retrieve a list of your billing accounts based on <code>x-customer-number</code>.</p><pre>GET /Account/v1/billingAccount</pre><div class="-mt--5 -text--h5">Parameters</div><p>The table below lists the required header parameter you must pass to complete this call successfully.</p><table class="chi-table -hover -mt--4"><thead><tr><th>Name</th><th>Type</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td style="white-space:nowrap;"><code>x-customer-number</code></td><td>string</td><td>The customer number assigned by Lumen.</td><td><code>x-customer-number: 1-ABC-23</code></td></tr></tbody></table><p class="-mt--5">The table below lists the optional query parameters you can pass.</p><table class="chi-table -hover -mt--4"><thead><tr><th>Name</th><th>Type</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td style="white-space:nowrap;"><code>entitled</code></td><td>boolean</td><td><p>Set to <code>true</code> to only return billing account numbers tied to a <code>x-customer-number</code> with Lumen Platform entitlement.</p><p>Leave null to show all billing account numbers.</p></td><td><code>/Account/v1/billingAccount?entitled=True</code></td></tr><tr><td style="white-space:nowrap;"><code>countryCode</code></td><td>string</td><td><p>Only return billing account numbers tied to a specific country.</p><p>The US is represented as <code>100</code> and is the only supported country code.</p></td><td><code>/Account/v1/billingAccount?countryCode=100</code></td></tr><tr><td style="white-space:nowrap;"><code>currencyCode</code></td><td>string</td><td><p>A three-letter ISO currency code (not case-sensitive). Only return billing account numbers using a specific currency.</p><p>The US dollar is represented as USD. <code>USD</code> is the only supported currency code.</p></td><td><code>/Account/v1/billingAccount?currencyCode=usd</code></td></tr></tbody></table><div class="-mt--5 -text--h5">Response</div><pre>{
    "billingAccount": [
        {
            "name": "Company or account name",
            "id": "5-RHQBGCGK",
            "invoiceDisplayNumber": "5-RHQBGCGK",
            "stringAddress": [
                {
                    "streetAddress": "1025 ELDORADO BLVD.,  14C-511",
                    "city": "BROOMFIELD",
                    "stateOrProvince": "CO",
                    "country": "USA",
                    "postcode": "80021"
                }
            ],
            "currency": "USD",
            "countryCode": "100",
            "hqCountry": "US"
        }
    ],
    "pageNumber": 1,
    "pageSize": 10,
    "resultCount": 4
}
</pre><p>In the response, a <code>name</code> and <code>id</code> is returned for each billing account. This name and ID must be passed through the <code>billingAccount</code> object when you create an order request in <a href="#order">Step 6: Order</a>. Using the example response above, you would extract the name and ID by copying the highlighted strings seen below:</p><pre>{
    "billingAccount": [
        {
            "name": "<mark>Company or account name</mark>",
            "id": "<mark>5-RHQBGCGK</mark>",
            "invoiceDisplayNumber": "5-RHQBGCGK",
            "stringAddress": [
                {
                    "streetAddress": "1025 ELDORADO BLVD.,  14C-511",
                    "city": "BROOMFIELD",
                    "stateOrProvince": "CO",
                    "country": "USA",
                    "postcode": "80021"
                }
            ],
            "currency": "USD",
            "countryCode": "100",
            "hqCountry": "US"
        }
}   
</pre><chi-alert class="-mt--3 -mb--3 sc-chi-alert-h sc-chi-alert-s hydrated" color="warning" icon="warning" type="bubble" size="md"><!----><div class="chi-alert -bubble -warning -md sc-chi-alert" role="alert"><chi-icon class="sc-chi-alert sc-chi-icon-h sc-chi-icon-s hydrated" color="warning" icon="warning" extra-class="chi-alert__icon"><div class="chi-icon -icon--warning chi-alert__icon sc-chi-icon"><svg xmlns="http://www.w3.org/2000/svg" xmlnsXlink="http://www.w3.org/1999/xlink" aria-hidden="true" class="sc-chi-icon"><use xlink:href="#icon-warning" class="sc-chi-icon"></use></svg></div></chi-icon><div class="chi-alert__content sc-chi-alert"><p class="chi-alert__text sc-chi-alert sc-chi-alert-s">
<p>The <code>/Account/v1/billingAccount</code> response also returns a list of your billing accounts with headquarters in the US and outside of the US. When you place an Internet On-Demand <a href="#order">order request</a>, you must use a billing account with <code>"hqCountry": "US"</code>.</p>
</p></div></div></chi-alert><pre>{
    "billingAccount": [
        {
            "name": "Company or account name",
            "id": "5-RHQBGCGK",
            "invoiceDisplayNumber": "5-RHQBGCGK",
            "stringAddress": [
                {
                    "streetAddress": "1025 ELDORADO BLVD.,  14C-511",
                    "city": "BROOMFIELD",
                    "stateOrProvince": "CO",
                    "country": "USA",
                    "postcode": "80021"
                }
            ],
            "currency": "USD",
            "countryCode": "100",
            "hqCountry": "<mark>US</mark>"
        }
}   
</pre><p class="-mb--4"><a href="#top">Back to top</a></p><div class="-mt--7 -text--h3" id="order">Step 6: Order</div><p>Send a POST request to the <code>/Customer/v3/Ordering/orderRequest</code> endpoint to convert your quote to an order.</p><pre>POST /Customer/v3/Ordering/orderRequest</pre><div class="-mt--5 -text--h5">Properties</div><p>The table below lists the required properties you must pass to complete this call successfully.</p><table class="chi-table -hover -mt--4"><thead><tr><th>Name</th><th>Type</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td style="white-space:nowrap;"><code>externalId</code></td><td>string</td><td><p>A custom ID provided by the customer and only understandable by them to facilitate their searches afterward.</p><p>The <code>externalId</code> can be a maximum of 20 characters.</p></td><td><code>"externalId": "EXT-000027740"</code></td></tr><tr><td style="white-space:nowrap;"><code>billingAccount.id</code></td><td>string</td><td>A unique identifier for the billing account.</td><td><code>"id": "5-RHQBGCGK"</code></td></tr><tr><td style="white-space:nowrap;"><code>billingAccount.name</code></td><td>string</td><td>The customer name associated with the billing account number as determined by the requestor.</td><td><code>"name": "Customer Name"</code></td></tr><tr><td style="white-space:nowrap;"><code>channel.id</code></td><td>string</td><td>The channel ID used for product orders. Use channel ID <code>99</code> for NaaS external API product orders.</td><td><code>"id": "99"</code></td></tr><tr><td style="white-space:nowrap;"><code>channel.name</code></td><td>string</td><td>The name of the channel the product order is submitted through. Use the name <code>NaaS ExternalAPI</code> for NaaS external API product orders.</td><td><code>"name": "NaaS ExternalAPI"</code></td></tr><tr><td style="white-space:nowrap;"><code>productOrderItem.id</code></td><td>string</td><td>A unique ID the customer provides to represent an order item.</td><td><code>"id": "e4d542d0-5403-11ed-be44-b7526dccc9dd"</code></td></tr><tr><td style="white-space:nowrap;"><code>productOrderItem.quantity</code></td><td>integer</td><td>Indicator of how many product items are in your order. Currently, only one product item can be submitted in a single order.</td><td><code>"quantity": 1</code></td></tr><tr><td style="white-space:nowrap;"><code>productOrderItem.action</code></td><td>string</td><td>Indicator of whether to add, modify, or delete an order.</td><td><code>"action": "add"</code></td></tr><tr><td style="white-space:nowrap;"><code>productOrderItem.product.productCharacteristic</code></td><td>array</td><td colspan="2"><a href="#productCharacteristic-array-table">View productCharacteristic array table</a></td></tr><tr><td style="white-space:nowrap;"><code>productOrderItem.product.productSpecification.id</code></td><td>string</td><td><p>A unique identifier used to represent a specific product or service.</p><p>Enter <code>5001</code> for this value. This ID value represents NaaS Internet.</p></td><td><code>"id": "5001"</code></td></tr><tr><td style="white-space:nowrap;"><code>productOrderItem.product.productSpecification.name</code></td><td>string</td><td><p>The name of the product.</p><p>Enter <code>NaaS Internet</code> for this value. This name value represents the NaaS Internet product.</p></td><td><code>"name": "NaaS Internet"</code></td></tr><tr><td style="white-space:nowrap;"><code>productOrderItem.productOffering.id</code></td><td>string</td><td><p>A unique identifier used to represent a specific product or service.</p><p>Enter <code>718</code> for this value. This ID value represents NaaS Internet On-Demand.</p></td><td><code>"id": "718"</code></td></tr><tr><td style="white-space:nowrap;"><code>productOrderItem.productOffering.name</code></td><td>string</td><td><p>The name of the product.</p><p>Enter <code>NaaS Internet On-Demand</code> for this value. This name value represents the NaaS Internet On-Demand product.</p></td><td><code>"name": "NaaS Internet On-Demand"</code></td></tr><tr><td style="white-space:nowrap;"><code>quote.id</code></td><td>string</td><td>The unique identifier returned after submitting a quote via the <code>/priceRequest</code> endpoint.</td><td><code>"id": "NaaS-7898"</code></td></tr><tr><td style="white-space:nowrap;"><code>quote.name</code></td><td>string</td><td>A custom name provided by the customer to reference a specific quote.</td><td><code>"name": "QuoteId"</code></td></tr><tr><td style="white-space:nowrap;"><code>relatedContactInformation.number</code></td><td>string</td><td><p>The telephone number of the designated order contact.</p><p>The <code>number</code> property only supports United States (US) telephone numbers.</p></td><td><code>"number": "+16157691234"</code></td></tr><tr><td style="white-space:nowrap;"><code>relatedContactInformation.emailAddress</code></td><td>string</td><td>The email address of the designated order contact.</td><td><code>"emailAddress": "first.lastname@domain.com"</code></td></tr><tr><td style="white-space:nowrap;"><code>relatedContactInformation.role</code></td><td>string</td><td>The role of the designated order contact.</td><td><code>"role": "Order Contact"</code></td></tr><tr><td style="white-space:nowrap;"><code>relatedContactInformation.organization</code></td><td>string</td><td>The organization of the designated order contact.</td><td><code>"organization": "Org Name"</code></td></tr><tr><td style="white-space:nowrap;"><code>relatedContactInformation.name</code></td><td>string</td><td>The first name and last name of the designated order contact.</td><td><code>"name": "FirstName LastName"</code></td></tr><tr><td style="white-space:nowrap;"><code>relatedContactInformation.numberExtension</code></td><td>string</td><td><p>The telephone number extension of the designated order contact.</p><p>The <code>numberExtension</code> property is not required.</p></td><td><code>"numberExtension": ""</code></td></tr></tbody></table><p class="-text--bold -mt--5" id="productCharacteristic-array-table">productCharacteristic array</p><p>The <code>productCharacteristic</code> array describes a given characteristic of a product through a name-value pair. This array contains the following required properties:</p><table class="chi-table -hover -mt--4"><thead><tr><th>attributeName</th><th>attributeValue Description</th><th>Type</th><th>Example</th></tr></thead><tbody><tr><td style="white-space:nowrap;">Customer Service Name</td><td>A custom name provided by the customer to reference a specific product order.</td><td>string</td><td><code>"name": "Customer Service Name",</code><br><code>"value": "My First NaaS Order"&nbsp;</code></td></tr><tr><td>VLAN Assignment</td><td><code>Auto Assign</code> is the only supported VLAN assignment.</td><td>string</td><td><code>"name": "VLAN Assignment",</code><br><code>"value": "Auto Assign"&nbsp;</code></td></tr><tr><td>IPV4 Block Size</td><td><p>The size of the IP address block.</p><p>Enter one of the following options:</p><ul><li><code>/28</code></li><li><code>/29</code></li><li><code>/30</code></li></ul></td><td>string</td><td><code>"name": "IPV4 Block Size",</code><br><code>"value": "/29"&nbsp;</code></td></tr><tr><td>Routing Options</td><td><code>Static</code> is the only supported routing option.</td><td>string</td><td><code>"name": "Routing Options",</code><br><code>"value": "Static"&nbsp;</code></td></tr></tbody></table><div class="-mt--5 -text--h5">Request body</div><pre>{
    "externalId": "EXT-000027740",
    "billingAccount": {
        "id": "5-RHQBGCGK",
        "name": "Company or account name"
    },
    "channel": [
        {
            "id": "99",
            "name": "NaaS ExternalAPI"
        }
    ],
    "note": [
        {
            "text": "This is your reference note for tracking purposes."
        }
    ],
    "productOrderItem": [
        {
            "id": "Customer internal order line ID.",
            "quantity": 1,
            "action": "add",
            "product": {
                "productCharacteristic": [
                    {
                        "name": "Customer Service Name",
                        "valueType": "String",
                        "value": "My First NaaS"
                    },
                    {
                        "name": "VLAN Assignment",
                        "valueType": "String",
                        "value": "Auto Assign"
                    },
                    {
                        "name": "IPV4 Block Size",
                        "valueType": "String",
                        "value": "/29"
                    },
                    {
                        "name": "Routing Options",
                        "valueType": "String",
                        "value": "Static"
                    }
                ],
                "productSpecification": {
                    "id": "5001",
                    "name": "NaaS Internet"
                }
            },
            "productOffering": {
                "id": "718",
                "name": "NaaS Internet"
            }
        }
    ],
    "quote": [
        {
            "id": "NaaS-7888",
            "name": "quoteId"
        }
    ],
    "relatedContactInformation": [
        {
            "number": "+16157691234",
            "emailAddress": "first.lastname@domain.com",
            "role": "Order Contact",
            "organization": "Org Name",
            "name": "FirstName LastName",
            "numberExtension": ""
        }
    ]
}
</pre><div class="-mt--5 -text--h5">Response</div><pre>{
    "id": "1ec0b2a0-41e7-11ee-86e8-0b78fbf26f76",
    "externalId": "1692817140",
    "billingAccount": {
        "id": "5-RHQBGCGK",
        "name": "Company or account name"
    },
    "channel": [
        {
            "id": "99",
            "name": "NaaS ExternalAPI"
        }
    ],
    "note": [
        {
            "text": "This is your reference note for tracking purposes"
        }
    ],
    "productOrderItem": [
        {
            "id": "Customer internal order line ID",
            "quantity": 1,
            "action": "add",
            "appointment": null,
            "billingAccount": null,
            "itemPrice": null,
            "itemTerm": null,
            "itemTotalPrice": null,
            "payment": null,
            "product": {
                "productCharacteristic": [
                    {
                        "name": "Customer Service Name",
                        "valueType": "String",
                        "value": "My First NaaS"
                    },
                    {
                        "name": "VLAN Assignment",
                        "valueType": "String",
                        "value": "Auto Assign"
                    },
                    {
                        "name": "IPV4 Block Size",
                        "valueType": "String",
                        "value": "/29"
                    },
                    {
                        "name": "Routing Options",
                        "valueType": "String",
                        "value": "Static"
                    }
                ],
                "productSpecification": {
                    "id": "5001",
                    "name": "NaaS Internet"
                }
            },
            "productOffering": {
                "id": "718",
                "name": "NaaS Internet"
            },
            "productOfferingQualificationItem": null,
            "productOrderItem": null,
            "productOrderItemRelationship": null,
            "qualification": null,
            "quoteItem": null,
            "state": null,
            "@baseType": null,
            "@schemaLocation": null,
            "@type": "ProductOrderItem"
        }
    ],
    "quote": [
        {
            "id": "NaaS-1468",
            "name": "quoteId"
        }
    ],
    "relatedContactInformation": [
        {
            "number": "+16157691234",
            "emailAddress": "first.lastname@domain.com",
            "role": "Order Contact",
            "organization": "Org Name",
            "name": "FirstName LastName",
            "numberExtension": ""
        }
    ]
}
</pre><p class="-mb--4"><a href="#top">Back to top</a></p><div class="-mt--5 -text--h3" id="check-your-webhooks">Step 7: Check your webhooks</div><p>Once you request an Internet On-Demand connection, review your webhook notifications to verify if your order has been completed or failed. Visit our <a href="https://developer.lumen.com/apis/internet-on-demand#webhooks_notification-types">Notification types</a> page to learn more about the expected webhook notifications you should receive when creating an Internet On-Demand connection.</p><chi-alert class="-mb--3 -mt--3 sc-chi-alert-h sc-chi-alert-s hydrated" color="info" icon="circle-info" type="bubble" size="md"><!----><div class="chi-alert -bubble -info -md sc-chi-alert" role="alert"><chi-icon class="sc-chi-alert sc-chi-icon-h sc-chi-icon-s hydrated" color="info" icon="circle-info" extra-class="chi-alert__icon"><div class="chi-icon -icon--info chi-alert__icon sc-chi-icon"><svg xmlns="http://www.w3.org/2000/svg" xmlnsXlink="http://www.w3.org/1999/xlink" aria-hidden="true" class="sc-chi-icon"><use xlink:href="#icon-circle-info" class="sc-chi-icon"></use></svg></div></chi-icon><div class="chi-alert__content sc-chi-alert"><p class="chi-alert__text sc-chi-alert sc-chi-alert-s">
<p>Don't have webhooks set up? In that case, we recommend calling our <code>/ProductInventory/v1/inventory</code> endpoint to retrieve the service status of your Internet On-Demand connection to ensure it is <code>Active</code>.</p>

<p>Visit our <a href="https://developer.lumen.com/apis/internet-on-demand#how-tos_check-your-service-details"> Check your service details</a> page for instructions on how to retrieve the status details of your service.</p>
</p></div></div></chi-alert><p class="-mb--4"><a href="#top">Back to top</a></p></div></div> <!----></div></div></div>
