# meCash API Documentation

## Overview

This repository contains the documentation for the meCash API, which allows businesses and applications to facilitate cross-border payments. The API provides endpoints to initiate payments, handle transactions, and manage errors effectively.

The documentation includes details about the API's request and response formats, error handling, and examples to help you integrate the API into your application.

## Repository Structure

The repository is organized as follows:

- `README.md`: This file provides an overview of the repository and instructions for viewing the documentation.
- **API Documentation**: The detailed API documentation is included in this repository.

## How to View the Documentation

The API documentation is provided in Markdown format within this repository. To view it:

### Locally:
Clone this repository to your local machine using the following command:

```bash
git clone [<repository_url>](https://github.com/herchioma/meCash_API_Doc.git)
```

Open the `README.md` file or any other Markdown files in the repository using a Markdown viewer or text editor.

## API Documentation Summary

The meCash API provides the following key features:

- **Initiate Payments**: Use the `POST /api/v1/payments` endpoint to send cross-border payments.
- **Request Format**: The API expects a JSON payload with details about the sender, recipient, and payment amount.
- **Response Format**: The API returns a JSON response indicating the status of the transaction.
- **Error Handling**: Detailed error codes and messages are provided for troubleshooting.

For detailed documentation, refer to the sections below or explore the repository files.

## Getting Started

To use the meCash API, follow these steps:

### Authentication
Include your API key in the Authorization header of each request:

```http
Authorization: Bearer <your_api_key>
```

### Make a Payment
Send a `POST` request to the `/api/v1/payments` endpoint with the required JSON payload. Refer to the Request section for details.

### Handle Responses
Check the response status code and body to determine if the payment was successful or if an error occurred. Refer to the Response section for details.

## Example Usage

### Successful Payment

**Request:**

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

**Response:**

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

## Error Handling

The API returns specific error codes and messages to help you troubleshoot issues. Common errors include:

| HTTP Status Code | Error Code       | Description                                              |
|-----------------|-----------------|----------------------------------------------------------|
| 400             | INVALID_REQUEST  | The request is missing required fields or contains invalid data. |
| 401             | UNAUTHORIZED     | The API key is missing or incorrect.                     |
| 403             | FORBIDDEN        | You don’t have permission to perform this transaction.   |
| 404             | NOT_FOUND        | The recipient’s bank or account could not be found.     |
| 500             | INTERNAL_ERROR   | Something went wrong on the server.                     |
