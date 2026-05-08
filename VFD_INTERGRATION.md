# Tembowave AAS Documentation 🌐

## About this API

Welcome to Tembowave API 1.0. Getting started with Tembowave is quick and easy!

In this release, you will learn how to use our API endpoints to access Tembowave services. Our APIs are built on REST (Representational State Transfer) thus our data entities are represented as HTTP resources and are accessed using HTTP verbs `GET` and `POST`. Requests and responses are JSON encoded.

Find information about integrating your App with Tembowave. This documentation includes sample codes for each API we have.

## Base URLs

| Environment | URL |
|---|---|
| Sandbox / Testing | `https://saasapi.tembowave.co.tz/` |
| Production / Live | `https://saasapi.tembowave.co.tz/` |

## Error Object

In case an error occurs during any API call, Tembowave will respond with a json string in the following format:

```json
{
  "error": {
    "type": "error_type",
    "code": "response_code",
    "message": "Detailed error message goes here.."
  }
}
```

## AUTHORIZATION

### Bearer Token

Token:
`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0aW4iOiIxNTI0MTUzMTEiLCJpZCI6ImYxODQzNTg1LTUzZjctNDQyNi1iMTFiLWZjYzFmMTVhYzExNiIsInJvbGUiOiJDTElFTlQiLCJpYXQiOjE3MzAyMDM5MDYsImV4cCI6MTczMDgwODcwNn0.epC1v6ObNqflwT2oaL87KCi_E9J4ccDauYFm5itaAlU`

---

## Auth

Tembowave API Authentication requests are done via a `POST` request.

Your Tembowave Business `client_id` and `password` will be used to generate an access token. This access token is valid for a maximum period of 5 minutes. Use this token (sent as a Bearer Token) to access all other Tembowave API 1.0 endpoints.

The URL to the API service is either:
* **Sandbox/Demo URL:** `https://saasapi.tembowave.co.tz/v1/auth/clientLogin`
* **Production/Live URL:** `https://saasapi.tembowave.co.tz/v1/auth/clientLogin`

### Endpoint
`POST /v1/auth/clientLogin`

### HTTP Request Headers
| Parameter | Required | Description |
|---|---|---|
| `Accept` | Required | `application/json` |
| `Content-Type` | Required | `application/json` |

### Request Parameters
Your live/production `client_id` and `password` will be sent to you after registration and/or TRA activation.
Please click here to open a live/production client account.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `password` | String | Required | The password parameter must be set to client’s password as provided. |
| `client_id` | String | Required | The client_id parameter must be set to client’s client_id as provided. |

### Sample Request
```json
{
    "client_id": "xxxxx",
    "password": "xxxxxx"
}
```

### Response Parameters
| Name | Type | Description |
|---|---|---|
| `access_token` | String | The access token string as issued by the server. The access token issued will have permission to invoke all Tembowave API operations. |

### Sample Response
```json
{
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0aW4iOiIxNTI0MTUzMTEiLCJpZCI6ImYxODQzNTg1LTUzZjctNDQyNi1iMTFiLWZjYzFmMTVhYzExNiIsInJvbGUiOiJDTElFTlQiLCJpYXQiOjE2Nzk1NjA1NjMsImV4cCI6MTY4MDE2NTM2M30.YEVojEQmnisQ5EXwWzRY-oGuZdmAASmNcz93S6ruKWQ"
}
```

### Example Request (cURL)
```bash
curl --location 'https://saasapi.tembowave.co.tz/v1/auth/clientLogin' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--data '{a
    "client_id": "f1843585-53f7-4426-b11b-fcc1f15ac116",
    "password": "v*PK1u"
}'
```

---

## Invoice Request

Once you have received the bearer token, the next step will be the actual request to create a fiscalize invoice / receipt.

The URL to our Invoice API is either:
* **Sandbox/Demo URL:** `https://saasapi.tembowave.co.tz/v1/vfd/createInvoice`
* **Production/Live URL:** `https://saasapi.tembowave.co.tz/v1/vfd/createInvoice`

### Endpoint
`POST /v1/vfd/createInvoice`

### HTTP Request Headers
| Parameter | Required | Value |
|---|---|---|
| `Content-type` | Required | `application/json` |
| `Accept` | Required | `application/json` |
| `Authorization`| Required | `Bearer <access_token>` |

### Request Parameters
| Parameter | Type | Required | Description |
|---|---|---|---|
| `deviceId` | String | Required | This refers to your Tembowave VFD Device ID. |
| `customerId` | String | Required | This refers to your customer ID. |
| `customerIdType` | String | Required | This refers to type of customer ID. |
| `customerName` | String | Optional | This refers to customer name. |
| `mobileNumber` | String | Optional | This refers to customer phone number. |
| `items` | Items Object | Required | This refers to an array of invoice / receipt items. |
| `totals` | Totals Object | Required | This refers to an object of invoice / receipt totals. |
| `payments` | Payment Object| Required | This refers to an object of invoice / receipt payment modes. |
| `invoice_id` | String | Optional | This refers to the unique invoice identifier from your system. |

### Request Data Types

#### Customer ID Types (`customerIdType`)
| Type | Description |
|---|---|
| `1` | TIN |
| `2` | Driving Licence |
| `3` | Voters Number |
| `4` | Passport |
| `5` | NID (National Identity) |
| `6` | NIL (If there is no ID) |

#### Receipt / Invoice Items (`items`)
| Parameter | Required | Data type | Description |
|---|---|---|---|
| `id` | Required | int | Standard item code generated by your system |
| `desc` | Required | String | Name of items can either be standard or entered by the user |
| `qty` | Required | int | Quantity |
| `taxcode` | Required | int | Applicable tax on the item |
| `amt` | Required | String | Tax inclusive amount per item |

**Taxcode Definitions:**
| TaxCode | Remark |
|---|---|
| `1` | Standard Rate (18%) - Vatable items |
| `2` | Special Rate (0%) - Special Rate items |
| `3` | Zero Rated (0%) - Zero rated items |
| `4` | Special Relief (0%) - Special relief items |
| `5` | Exempt (0%) - Tax exempt items |

#### Receipt / Invoice Totals (`totals`)
| Parameter | Data type | Required | Description |
|---|---|---|---|
| `totaltaxincl` | String | required | Grand Total - Total amount of all the items inclusive of taxes |
| `discount` | String | required | Total Discount - Amount discounted from the total of all the items exclusive of tax |

#### Receipt / Invoice Payments (`payments`)
| Parameter | Data type | Required | Description |
|---|---|---|---|
| `pmttype` | String | required | Payment types: `1. CASH`, `2. CHEQUE`, `3. CCARD`, `4. EMONEY`, `5. INVOICE`. <br/>*For RECEIPTS use payment types CASH, CHEQUE, CCARD or EMONEY. For INVOICES use INVOICE only.* |
| `pmtamount` | String | required | Payment amount based on payment type used |

### Example Request (cURL)
```bash
curl --location 'https://saasapi.tembowave.co.tz/v1/vfd/createInvoice' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <access_token>' \
--data '{
  "invoice_id": "PID092",
  "deviceId": "9013123-1312df-123312-213122d"
  "customerId": "102",
  "customerIdType": "6",
  "customerName": "MojaONe LTD",
  "mobileNumber": "0787640622",
  "items": [
    {
      "id": 1,
      "desc": "Solar Panel",
      "qty": 3,
      "taxcode": 1,
      "amt": "5000.00"
    }
  ],
  "totals": {
    "totaltaxincl": "15000.00",
    "discount": "0"
  },
  "payments": {
    "pmttype": "INVOICE",
    "pmtamount": "15000.00"
  }
}'
```

*(Note: This request does not return any response body.)*

---

## Invoice History

Use this endpoint to fetch previously created invoices.

The URL to our Invoice API is either:
* **Sandbox/Demo URL:** `https://saasapi.tembowave.co.tz/v1/vfd/invoices`
* **Production/Live URL:** `https://saasapi.tembowave.co.tz/v1/vfd/invoices`

### Endpoint
`POST /v1/vfd/invoices`

### HTTP Request Headers
| Parameter | Required | Value |
|---|---|---|
| `Content-type` | Required | `application/json` |
| `Accept` | Required | `application/json` |
| `Authorization`| Required | `Bearer <access_token>` |

### Example Request (cURL)
```bash
curl --location 'https://saasapi.tembowave.co.tz/v1/vfd/invoices' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <access_token>' \
--data '{
    "dateFrom": "2023-01-01",
    "deviceId": "",
    "dateTo": "2024-10-01"
}'
```

### Example Response
```json
{
  "invoices": [
    {
      "invoice_id": "PID092",
      "invoiceNumber": 1316,
      "invoiceDate": "2024-09-17",
      "traSent": false,
      "discount": "0.00",
      "InvoiceVerificationCode": "1D70461316",
      "invoiceTime": "20:46:39",
      "customerIdType": "6",
      "customerId": "102",
      "customerName": "MojaONe LTD",
      "totalTaxExcl": "12711.86",
      "totalTaxIncl": "15000.00",
      "taxAmount": "2288.14",
      "traResponseText": null,
      "vatRateATaxamount": "2288.14",
      "vatRateANetAmount": "12711.86",
      "vatRateBTaxamount": "0.00",
      "vatRateBNetAmount": "0.00",
      "vatRateCTaxamount": "0.00",
      "vatRateCNetAmount": "0.00",
      "vatRateDNetAmount": "0.00",
      "vatRateENetAmount": "0.00",
      "invoiceItems": [
        {
          "itemDesc": "Solar Panel",
          "itemId": "1",
          "itemQty": 3,
          "itemTaxCode": 1,
          "ItemAmount": "5000.00"
        }
      ]
    },
    {
      "invoice_id": "PID092",
      "invoiceNumber": 1317,
      "invoiceDate": "2024-09-20",
      "traSent": false,
      "discount": "0.00",
      "InvoiceVerificationCode": "1D70461317",
      "invoiceTime": "17:35:37",
      "customerIdType": "6",
      "customerId": "102",
      "customerName": "MojaONe LTD",
      "totalTaxExcl": "12711.86",
      "totalTaxIncl": "15000.00",
      "taxAmount": "2288.14",
      "traResponseText": null,
      "vatRateATaxamount": "2288.14",
      "vatRateANetAmount": "12711.86",
      "vatRateBTaxamount": "0.00",
      "vatRateBNetAmount": "0.00",
      "vatRateCTaxamount": "0.00",
      "vatRateCNetAmount": "0.00",
      "vatRateDNetAmount": "0.00",
      "vatRateENetAmount": "0.00",
      "invoiceItems": [
        {
          "itemDesc": "Solar Panel",
          "itemId": "1",
          "itemQty": 3,
          "itemTaxCode": 1,
          "ItemAmount": "5000.00"
        }
      ]
    }
  ],
  "taxSummary": {
    "totalTaxExcl": 37681761.26,
    "totalTax": 6293837.2,
    "totalTaxIncl": 43975598.46,
    "vatRateANetAmount": 34965761.26,
    "vatRateATaxAmount": 6293837.2,
    "vatRateBNetAmount": 144000,
    "vatRateBTaxAmount": 0,
    "vatRateCNetAmount": 2572000,
    "vatRateCTaxAmount": 0,
    "vatRateDNetAmount": 0,
    "vatRateENetAmount": 0
  }
}
```

---

## Resend Invoice

Use this endpoint to resend a previously created invoice.

The URL to our Invoice API is either:
* **Sandbox/Demo URL:** `https://saasapi.tembowave.co.tz/v1/vfd/resendInvoice`
* **Production/Live URL:** `https://saasapi.tembowave.co.tz/v1/vfd/resendInvoice`

### Endpoint
`POST /v1/vfd/resendInvoice`

### HTTP Request Headers
| Parameter | Required | Value |
|---|---|---|
| `Content-type` | Required | `application/json` |
| `Accept` | Required | `application/json` |
| `Authorization`| Required | `Bearer <access_token>` |

### Example Request (cURL)
```bash
curl --location 'https://saasapi.tembowave.co.tz/v1/vfd/resendInvoice' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <access_token>' \
--data '{
    "invoiceVerificationCode": "1D7046725"
}'
```

### Example Response
```json
{
  "status": 200,
  "message": "Receipt will be re-sent shortly"
}
```
