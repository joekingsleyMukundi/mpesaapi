# M-Pesa API Integration Demo

## Overview

This Express.js application demonstrates integration with Safaricom's M-Pesa API. It provides endpoints to interact with various M-Pesa functionalities, including token generation, URL registration, transaction simulation, balance querying, and STK push payments.

## Dependencies

- `express`: Web framework for Node.js
- `body-parser`: Middleware for parsing JSON request bodies
- `axios`: Promise-based HTTP client for making API requests
- `moment`: Library for date-time manipulation and formatting

## Setup

### 1. Install Dependencies

Ensure you have Node.js installed. Then, install the necessary packages using npm:

```bash
npm install express body-parser axios moment

2. Environment Configuration

The application uses hardcoded credentials for the Safaricom API. For production, use environment variables to securely manage these credentials.

Example .env file:

plaintext

CONSUMER_KEY=your_consumer_key
CONSUMER_SECRET=your_consumer_secret

Example of loading environment variables in your code:

js

require('dotenv').config();

const consumer_key = process.env.CONSUMER_KEY;
const consumer_secret = process.env.CONSUMER_SECRET;

Middleware
bodyParser.json()

    Parses JSON formatted request bodies.

API Endpoints
1. Root Endpoint
GET /

Description: Returns a simple "hey" message.

Response:

json

"hey"

Example:

bash

curl http://localhost:3000/

2. Access Token
GET /accesstoken

Description: Retrieves an OAuth access token from Safaricom's API and returns it.

Response:

json

{
  "access_token": "your_access_token_here"
}

Example:

bash

curl http://localhost:3000/accesstoken

Notes: The access token is required for other API operations.
3. Register URLs
GET /registerurl

Description: Registers confirmation and validation URLs with Safaricom.

Response:

json

{
  "ResponseCode": "0",
  "ResponseDescription": "Success"
}

Example:

bash

curl http://localhost:3000/registerurl

Notes: Replace ShortCode, ConfirmationURL, and ValidationURL as needed.
4. Confirmation URL
POST /confirmation

Description: Receives confirmation data from Safaricom after a transaction.

Request Body:

json

{
  "TransactionType": "Paybill",
  "TransID": "123456",
  "TransTime": "20240809123456",
  "Amount": "100",
  "SenderNumber": "254708374149",
  "ReceiverNumber": "603021",
  "OrgAccountBalance": "1000"
}

Response: No response is required.

Example:

bash

curl -X POST http://localhost:3000/confirmation -d '{"TransactionType":"Paybill","TransID":"123456","TransTime":"20240809123456","Amount":"100","SenderNumber":"254708374149","ReceiverNumber":"603021","OrgAccountBalance":"1000"}' -H "Content-Type: application/json"

5. Validation URL
POST /validation

Description: Receives validation data from Safaricom before processing a transaction.

Request Body:

json

{
  "TransactionType": "Paybill",
  "TransID": "123456",
  "TransTime": "20240809123456",
  "Amount": "100",
  "SenderNumber": "254708374149",
  "ReceiverNumber": "603021"
}

Response: No response is required.

Example:

bash

curl -X POST http://localhost:3000/validation -d '{"TransactionType":"Paybill","TransID":"123456","TransTime":"20240809123456","Amount":"100","SenderNumber":"254708374149","ReceiverNumber":"603021"}' -H "Content-Type: application/json"

6. Simulate Transaction
GET /simulate

Description: Simulates a transaction with Safaricom's API.

Response:

json

{
  "ResponseCode": "0",
  "ResponseDescription": "Success"
}

Example:

bash

curl http://localhost:3000/simulate

Notes: Adjust ShortCode, CommandID, Amount, Msisdn, and BillRefNumber as needed.
7. Account Balance
GET /balance

Description: Retrieves the account balance from Safaricom's API.

Response:

json

{
  "ResponseCode": "0",
  "ResponseDescription": "Success",
  "Balance": "1000"
}

Example:

bash

curl http://localhost:3000/balance

Notes: Adjust Initiator, SecurityCredential, PartyA, and other parameters as needed.
8. Timeout URL
POST /timeout_url

Description: Receives timeout responses from Safaricom's balance query request.

Request Body:

json

{
  "ResponseCode": "500",
  "ResponseDescription": "Request Timeout"
}

Response: No response is required.

Example:

bash

curl -X POST http://localhost:3000/timeout_url -d '{"ResponseCode":"500","ResponseDescription":"Request Timeout"}' -H "Content-Type: application/json"

9. Result URL
POST /result_url

Description: Receives results from Safaricom's account balance query request.

Request Body:

json

{
  "ResponseCode": "0",
  "ResponseDescription": "Success",
  "Balance": "1000"
}

Response: No response is required.

Example:

bash

curl -X POST http://localhost:3000/result_url -d '{"ResponseCode":"0","ResponseDescription":"Success","Balance":"1000"}' -H "Content-Type: application/json"

10. STK Push
GET /stk

Description: Initiates a STK push payment request with Safaricom's API.

Response:

json

{
  "ResponseCode": "0",
  "ResponseDescription": "Success",
  "CheckoutRequestID": "ws_CO_123456"
}

Example:

bash

curl http://localhost:3000/stk

Notes: Adjust BusinessShortCode, Password, Timestamp, Amount, PhoneNumber, and other parameters as needed.
11. Callback URL
POST /callback

Description: Receives callback data from Safaricom's STK push response.

Request Body:

json

{
  "ResultCode": "0",
  "ResultDesc": "Success",
  "MerchantRequestID": "123456",
  "CheckoutRequestID": "ws_CO_123456",
  "PaymentRequestID": "123456"
}

Response: No response is required.

Example:

bash

curl -X POST http://localhost:3000/callback -d '{"ResultCode":"0","ResultDesc":"Success","MerchantRequestID":"123456","CheckoutRequestID":"ws_CO_123456","PaymentRequestID":"123456"}' -H "Content-Type: application/json"

Server Configuration

    Port: The server listens on the port defined in the environment variable PORT or defaults to 3000.

js

const port = process.env.PORT || 3000;
app.listen(port, () => {
    console.log("server started");
});

Error Handling

Errors are logged to the console. For production, consider implementing comprehensive error handling and logging.

js

.catch((error) => {
    console.log(error);
});
