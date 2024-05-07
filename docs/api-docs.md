---
layout: default
title: API
parent: Documentation
nav_order: 1
---

Create an Internet On-Demand connection to a data center
--------------------------------------------------------

On this page

*   [Create an Internet On-Demand connection](#create-a-connection)
*   [Step 1: Find providers](#providers)
*   [Step 2: Browse locations](#location)
*   [Step 3: Qualify](#qualify)
*   [Step 4: Pricing](#pricing)
*   [Step 5: Find billing account](#billing)
*   [Step 6: Order](#order)
*   [Step 7: Check your webhooks](#check-your-webhooks)

Create an Internet On-Demand connection

Follow the steps below to create a Lumen Internet On-Demand connection to one of our partner data centers.

Step 1: Find providers

Send a GET request to the `/Network/v1/provider` endpoint to retrieve a list of providers or partners who connect with the Lumen network.

GET /Network/v1/provider

Response

{
    "pagination": \[
        {
            "pageNumber": 1,
            "pageSize": 1,
            "totalRecords": 2
        }
    \],
    "listPartners": \[
        {
            "id": "43558",
            "name": "Equinix",
            "datacenterFabricUrl": "https://ecxfabric.equinix.com/",
            "partnerType": "CLOUD\_PARTNER"
        },
        {
            "id": "362232",
            "name": "Digital Realty",
            "datacenterFabricUrl": "https://www.digitalrealty.com/platform-digital/connectivity/service-fabric/connect",
            "partnerType": "CLOUD\_PARTNER"
        }
    \]
}

In the response, an `id` is returned for each partner. This ID must be passed as the `partnerId` when you submit a GET request to the `/Network/v1/location` endpoint.

[Back to top](#top)

Step 2: Browse locations

Send a GET request to the `/Network/v1/location` endpoint to retrieve a list of NaaS-enabled locations on the Lumen network.

GET /Network/v1/location

Parameters

The table below lists the required query parameter you must pass to complete this call successfully.

Name

Type

Description

Example

`partnerId`

string

The unique ID of a data center or cloud provider.

You can retrieve a list of providers and their partner information by making a GET request to the `/Network/v1/provider` endpoint.

`/Network/v1/location?partnerId=43558`

Response

{
    "pagination": \[
        {
        "pageNumber": 1,
        "pageSize": 10,
        "totalRecords": 1021
        }
    \],
    "location": \[
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
    \],
    "networkStatus": "ON\_NET",
    "naasEnabled": true,
    "partner": "Equinix Ethernet Exchange - Silicon Valley IBX",
    "market": "WASHINGTON DC",
    "productFamily": {
        "productFamily": "Internet On-Demand",
        "accessCapabilities": \[
        {
            "accessCapability": \[
            {
                "speed": "1000 mbps",
                "displayValue": "1 gbps",
                "circuitType": "Hosted-NNI Internet"
            }
            \]
        }
        \]
    }
}

Extract the `masterSiteId` from the response. Using the example response above, you would extract the ID by copying the highlighted `masterSiteId` string seen below:

    {
        "location": \[
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
        \],
    }

The `masterSiteId` is a required parameter when using the `/Product/v1/price` endpoint to request a location's available products and pricing information.

[Back to top](#top)

Step 3: Qualify

Send a GET request to the `/Product/v1/price` endpoint to retrieve a list of product offerings based on location.

GET /Product/v1/price

Parameters

The table below lists the required query parameters you must pass to complete this call successfully.

Name

Type

Description

Example

`masterSiteId`

string

A unique server-side identifier for a network location.

`/Product/v1/price?masterSiteId=PL0001247634`

`productCode`

string

A unique product code ID used to identify the product to be priced.

The NaaS `productCode` is `718`.

`/Product/v1/price?productCode=718`

Response

{
    "location": \[
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
    \],
    "pricePerSpeed": \[
        {
        "speed": "100 Mbps",
        "price": "$0.46",
        "term": "Hourly Rate",
        "displayValue": "100 Mbps"
        }
    \]
}

[Back to top](#top)

Step 4: Pricing

Send a POST request to the `/Product/v1/priceRequest` endpoint to create a request for pricing based on product, product attributes, and location.

POST /Product/v1/priceRequest

This call returns a unique quote ID that is valid for **15 minutes**. While the quote ID is still valid, you must use the quote ID to promote the pricing quote to an order using the `/Customer/v3/Ordering/orderRequest` endpoint.

You will need to generate a new quote if the quote is not promoted to an order within the 15-minute window. The unique quote ID follows this format: `"id": "NaaS-1466"`

Properties

The table below lists the required properties you must pass to complete this call successfully.

Name

Type

Description

Example

`sourceSystem`

string

The name used to identify the system type requesting prices for a product or service.

Enter `NaaS ExternalAPI` for this value to indicate a NaaS product or service price request.

`"sourceSystem": "NaaS ExternalAPI"`

`customerNumber`

string

A Lumen-defined customer ID, also known as a BusOrg number.

`"customerNumber": "1-ABCDE"`

`currencyCode`

string

A three-letter ISO currency code (not case-sensitive).

The US dollar is represented as USD. `USD` is the only supported currency code.

`"currencyCode": "USD"`

`masterSiteId`

string

A unique server-side identifier for a network location.

`"masterSiteId": "PL0001247634"`

`partnerId`

string

The data center or cloud provider ID.

`"partnerId": "43558",`

`productCode`

string

A unique product code ID used to identify the product to be priced.

The NaaS `productCode` is `718`.

`"productCode": "718"`

`speed`

string

The bandwidth available for a service.

`"speed": "1000 Mbps"`

Request body

{
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

Response

{
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
    "price": \[
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
    \]
}

[Back to top](#top)

Step 5: Find billing account

Send a GET request to the `/Account/v1/billingAccount` endpoint to retrieve a list of your billing accounts based on `x-customer-number`.

GET /Account/v1/billingAccount

Parameters

The table below lists the required header parameter you must pass to complete this call successfully.

Name

Type

Description

Example

`x-customer-number`

string

The customer number assigned by Lumen.

`x-customer-number: 1-ABC-23`

The table below lists the optional query parameters you can pass.

Name

Type

Description

Example

`entitled`

boolean

Set to `true` to only return billing account numbers tied to a `x-customer-number` with Lumen Platform entitlement.

Leave null to show all billing account numbers.

`/Account/v1/billingAccount?entitled=True`

`countryCode`

string

Only return billing account numbers tied to a specific country.

The US is represented as `100` and is the only supported country code.

`/Account/v1/billingAccount?countryCode=100`

`currencyCode`

string

A three-letter ISO currency code (not case-sensitive). Only return billing account numbers using a specific currency.

The US dollar is represented as USD. `USD` is the only supported currency code.

`/Account/v1/billingAccount?currencyCode=usd`

Response

{
    "billingAccount": \[
        {
            "name": "Company or account name",
            "id": "5-RHQBGCGK",
            "invoiceDisplayNumber": "5-RHQBGCGK",
            "stringAddress": \[
                {
                    "streetAddress": "1025 ELDORADO BLVD.,  14C-511",
                    "city": "BROOMFIELD",
                    "stateOrProvince": "CO",
                    "country": "USA",
                    "postcode": "80021"
                }
            \],
            "currency": "USD",
            "countryCode": "100",
            "hqCountry": "US"
        }
    \],
    "pageNumber": 1,
    "pageSize": 10,
    "resultCount": 4
}

In the response, a `name` and `id` is returned for each billing account. This name and ID must be passed through the `billingAccount` object when you create an order request in [Step 6: Order](#order). Using the example response above, you would extract the name and ID by copying the highlighted strings seen below:

{
    "billingAccount": \[
        {
            "name": "Company or account name",
            "id": "5-RHQBGCGK",
            "invoiceDisplayNumber": "5-RHQBGCGK",
            "stringAddress": \[
                {
                    "streetAddress": "1025 ELDORADO BLVD.,  14C-511",
                    "city": "BROOMFIELD",
                    "stateOrProvince": "CO",
                    "country": "USA",
                    "postcode": "80021"
                }
            \],
            "currency": "USD",
            "countryCode": "100",
            "hqCountry": "US"
        }
}   

The `/Account/v1/billingAccount` response also returns a list of your billing accounts with headquarters in the US and outside of the US. When you place an Internet On-Demand [order request](#order), you must use a billing account with `"hqCountry": "US"`.

{
    "billingAccount": \[
        {
            "name": "Company or account name",
            "id": "5-RHQBGCGK",
            "invoiceDisplayNumber": "5-RHQBGCGK",
            "stringAddress": \[
                {
                    "streetAddress": "1025 ELDORADO BLVD.,  14C-511",
                    "city": "BROOMFIELD",
                    "stateOrProvince": "CO",
                    "country": "USA",
                    "postcode": "80021"
                }
            \],
            "currency": "USD",
            "countryCode": "100",
            "hqCountry": "US"
        }
}   

[Back to top](#top)

Step 6: Order

Send a POST request to the `/Customer/v3/Ordering/orderRequest` endpoint to convert your quote to an order.

POST /Customer/v3/Ordering/orderRequest

Properties

The table below lists the required properties you must pass to complete this call successfully.

Name

Type

Description

Example

`externalId`

string

A custom ID provided by the customer and only understandable by them to facilitate their searches afterward.

The `externalId` can be a maximum of 20 characters.

`"externalId": "EXT-000027740"`

`billingAccount.id`

string

A unique identifier for the billing account.

`"id": "5-RHQBGCGK"`

`billingAccount.name`

string

The customer name associated with the billing account number as determined by the requestor.

`"name": "Customer Name"`

`channel.id`

string

The channel ID used for product orders. Use channel ID `99` for NaaS external API product orders.

`"id": "99"`

`channel.name`

string

The name of the channel the product order is submitted through. Use the name `NaaS ExternalAPI` for NaaS external API product orders.

`"name": "NaaS ExternalAPI"`

`productOrderItem.id`

string

A unique ID the customer provides to represent an order item.

`"id": "e4d542d0-5403-11ed-be44-b7526dccc9dd"`

`productOrderItem.quantity`

integer

Indicator of how many product items are in your order. Currently, only one product item can be submitted in a single order.

`"quantity": 1`

`productOrderItem.action`

string

Indicator of whether to add, modify, or delete an order.

`"action": "add"`

`productOrderItem.product.productCharacteristic`

array

[View productCharacteristic array table](#productCharacteristic-array-table)

`productOrderItem.product.productSpecification.id`

string

A unique identifier used to represent a specific product or service.

Enter `5001` for this value. This ID value represents NaaS Internet.

`"id": "5001"`

`productOrderItem.product.productSpecification.name`

string

The name of the product.

Enter `NaaS Internet` for this value. This name value represents the NaaS Internet product.

`"name": "NaaS Internet"`

`productOrderItem.productOffering.id`

string

A unique identifier used to represent a specific product or service.

Enter `718` for this value. This ID value represents NaaS Internet On-Demand.

`"id": "718"`

`productOrderItem.productOffering.name`

string

The name of the product.

Enter `NaaS Internet On-Demand` for this value. This name value represents the NaaS Internet On-Demand product.

`"name": "NaaS Internet On-Demand"`

`quote.id`

string

The unique identifier returned after submitting a quote via the `/priceRequest` endpoint.

`"id": "NaaS-7898"`

`quote.name`

string

A custom name provided by the customer to reference a specific quote.

`"name": "QuoteId"`

`relatedContactInformation.number`

string

The telephone number of the designated order contact.

The `number` property only supports United States (US) telephone numbers.

`"number": "+16157691234"`

`relatedContactInformation.emailAddress`

string

The email address of the designated order contact.

`"emailAddress": "first.lastname@domain.com"`

`relatedContactInformation.role`

string

The role of the designated order contact.

`"role": "Order Contact"`

`relatedContactInformation.organization`

string

The organization of the designated order contact.

`"organization": "Org Name"`

`relatedContactInformation.name`

string

The first name and last name of the designated order contact.

`"name": "FirstName LastName"`

`relatedContactInformation.numberExtension`

string

The telephone number extension of the designated order contact.

The `numberExtension` property is not required.

`"numberExtension": ""`

productCharacteristic array

The `productCharacteristic` array describes a given characteristic of a product through a name-value pair. This array contains the following required properties:

attributeName

attributeValue Description

Type

Example

Customer Service Name

A custom name provided by the customer to reference a specific product order.

string

`"name": "Customer Service Name",`  
`"value": "My First NaaS Order"` 

VLAN Assignment

`Auto Assign` is the only supported VLAN assignment.

string

`"name": "VLAN Assignment",`  
`"value": "Auto Assign"` 

IPV4 Block Size

The size of the IP address block.

Enter one of the following options:

*   `/28`
*   `/29`
*   `/30`

string

`"name": "IPV4 Block Size",`  
`"value": "/29"` 

Routing Options

`Static` is the only supported routing option.

string

`"name": "Routing Options",`  
`"value": "Static"` 

Request body

{
    "externalId": "EXT-000027740",
    "billingAccount": {
        "id": "5-RHQBGCGK",
        "name": "Company or account name"
    },
    "channel": \[
        {
            "id": "99",
            "name": "NaaS ExternalAPI"
        }
    \],
    "note": \[
        {
            "text": "This is your reference note for tracking purposes."
        }
    \],
    "productOrderItem": \[
        {
            "id": "Customer internal order line ID.",
            "quantity": 1,
            "action": "add",
            "product": {
                "productCharacteristic": \[
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
                \],
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
    \],
    "quote": \[
        {
            "id": "NaaS-7888",
            "name": "quoteId"
        }
    \],
    "relatedContactInformation": \[
        {
            "number": "+16157691234",
            "emailAddress": "first.lastname@domain.com",
            "role": "Order Contact",
            "organization": "Org Name",
            "name": "FirstName LastName",
            "numberExtension": ""
        }
    \]
}

Response

{
    "id": "1ec0b2a0-41e7-11ee-86e8-0b78fbf26f76",
    "externalId": "1692817140",
    "billingAccount": {
        "id": "5-RHQBGCGK",
        "name": "Company or account name"
    },
    "channel": \[
        {
            "id": "99",
            "name": "NaaS ExternalAPI"
        }
    \],
    "note": \[
        {
            "text": "This is your reference note for tracking purposes"
        }
    \],
    "productOrderItem": \[
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
                "productCharacteristic": \[
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
                \],
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
    \],
    "quote": \[
        {
            "id": "NaaS-1468",
            "name": "quoteId"
        }
    \],
    "relatedContactInformation": \[
        {
            "number": "+16157691234",
            "emailAddress": "first.lastname@domain.com",
            "role": "Order Contact",
            "organization": "Org Name",
            "name": "FirstName LastName",
            "numberExtension": ""
        }
    \]
}

[Back to top](#top)

Step 7: Check your webhooks

Once you request an Internet On-Demand connection, review your webhook notifications to verify if your order has been completed or failed. Visit our [Notification types](https://developer.lumen.com/apis/internet-on-demand#webhooks_notification-types) page to learn more about the expected webhook notifications you should receive when creating an Internet On-Demand connection.

Don't have webhooks set up? In that case, we recommend calling our `/ProductInventory/v1/inventory` endpoint to retrieve the service status of your Internet On-Demand connection to ensure it is `Active`.

Visit our [Check your service details](https://developer.lumen.com/apis/internet-on-demand#how-tos_check-your-service-details) page for instructions on how to retrieve the status details of your service.

[Back to top](#top)
