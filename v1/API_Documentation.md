# meCash API Documentation
## Payments

### Overview

The POST `/api/v1/payments` endpoint is used to initiate a cross-border payment. It takes in details about the sender, recipient, and payment amount, processes the transaction, and returns a response indicating whether the payment was successful or if there was an error. This API is ideal for businesses or applications that need to facilitate international money transfers.

---
### Request

#### Headers
| Header Name       | Value                        | Description                                                                 |
|-------------------|------------------------------|-----------------------------------------------------------------------------|
| `Authorization`   | `Bearer <your_api_key>`      | Your API key for authentication. Replace `<your_api_key>` with your actual key. |
| `Content-Type`    | `application/json`           | Specifies the format of the request body.                                   |

### Body Parameters
The request body must be in JSON format and include the following fields:

| Field                       | Type     | Optional | Description                                                                 |
|-----------------------------|----------|----------|-----------------------------------------------------------------------------|
| `amount`                    | `number` | `false`  | The amount of money to send, in major units (100.00 for USD).               |
| `currency`                  | `string` | `false`  | The currency of the payment (USD for US Dollars).                           |
| `sender`                    | `object` | `false`  | Information about the person sending the money.                             |
| `sender.name`               | `string` | `false`  | Full name of the sender (John Doe).                                         |
| `sender.email`              | `string` | `false`  | Email address of the sender (john.doe@x.com).                               |
| `recipient`                 | `object` | `false`  | Information about the person receiving the money.                           |
| `recipient.name`            | `string` | `false`  | Full name of the recipient (Jane Smith).                                    |
| `recipient.accountNumber`   | `string` | `false`  | Recipient’s bank account number (0987654321).                               |
| `recipient.bankCode`        | `string` | `false`  | A code that identifies the recipient’s bank (XYZ456).                       |
| `recipient.country`         | `string` | `false`  | Recipient’s country (USA).                                                  |
| `reference`                 | `string` | `false`  | A unique identifier for the transaction (INV-12345).                        |

---  
### Response

#### Success Response (HTTP 201)
If the payment is successful, the server responds with a 201 Created status code and a JSON object containing details about the transaction. Here’s what each field means:

| Field | Description |
|-------|-------------|
| **transactionId** | A unique ID for the transaction (e.g., `TXN789456123`). |
| **status** | The status of the transaction. In this case, it’s `SUCCESS`. |
| **createdAt** | The date and time when the transaction was processed (e.g., `2025-01-12T10:15:30Z`). |
| **amount** | The amount of money sent. This is returned in minor units (cents for USD).|
| **currency** | The currency of the payment. |
| **recipient** | An object containing details about the recipient. |
| **recipient.name** | The full name of the recipient. |
| **recipient.country** | The country where the recipient is located. |

---
#### Error Response (HTTP 400)
If there’s an issue with the request, the server responds with a 400 Bad Request status code and a JSON object explaining the error. Here’s what each field means:

| Field | Description |
|-------|-------------|
| **error** | A code indicating the type of error (e.g., `INVALID_REQUEST`). |
| **message** | A human-readable message explaining the error `(Invalid bank code for the recipient).` |

---
### Examples

#### Example 1: Successful Payment

#### Request:
```http
POST /api/v1/payments HTTP/1.1
Authorization: Bearer <your_api_key>
Content-Type: application/json

{
  "amount": 100.00, 
  "currency": "USD",
  "sender": {
    "name": "John Doe",
    "email": "john.doe@x.com"
  },
  "recipient": {
    "name": "Jane Smith",
    "accountNumber": "0987654321",
    "bankCode": "XYZ456",
    "country": "USA"
  },
  "reference": "INV-12345"
}
```

#### Response:
```json
{
  "transactionId": "TXN789456123",
  "status": "SUCCESS",
  "createdAt": "2025-01-12T10:15:30Z",
  "amount": 100,
  "currency": "USD",
  "recipient": {
    "name": "Jane Smith",
    "country": "USA"
  }
}
```

### Example 2: Error Due to Invalid Bank Code

#### Request:
```http
POST /api/v1/payments HTTP/1.1
Authorization: Bearer <your_api_key>
Content-Type: application/json

{
  "amount": 100,
  "currency": "USD",
  "sender": {
    "name": "John Doe",
    "email": "john.doe@x.com"
  },
  "recipient": {
    "name": "Jane Smith",
    "accountNumber": "0987654321",
    "bankCode": "INVALID_CODE",
    "country": "USA"
  },
  "reference": "INV-12345"
}
```

#### Response:
```json
{
  "error": "INVALID_REQUEST",
  "message": "Invalid bank code for the recipient."
}
```

---

## Error Handling
Here are some common errors you might encounter:

| HTTP Status Code | Error Code | Description |
|------------------|------------|-------------|
| **400** | `INVALID_REQUEST` | The request is missing required fields or contains invalid data. |
| **401** | `UNAUTHORIZED` | The API key is missing or incorrect. |
| **403** | `FORBIDDEN` | You don’t have permission to perform this transaction. |
| **404** | `NOT_FOUND` | The recipient’s bank or account could not be found. |
| **500** | `INTERNAL_ERROR` | Something went wrong on the server. |


