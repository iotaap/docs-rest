# Billing

Billing implements PayPal account linking for monthly subscription, as well as Invoicing and billing agreement management

## Get all transactions

It will return all PayPal transaction records for active user agreement. This info can be used for generating the invoice or 
basic payment information.

**URL** : `https://api.iotaap.io/v1/payments/transactions`

**Method** : `GET`

**Auth required** : YES

**URL Params**

- draw `(string)`: value will be returned back in the response

### Success Response

**Code** : `200 OK`

**Response example**

```json
{
    "transactions": [
        {
            "transaction_id": "I-HCVNSYS1VELK",
            "status": "Created",
            "transaction_type": "Recurring Payment",
            "payer_name": "John Doe",
            "time_stamp": "2020-01-31T16:40:46Z",
            "time_zone": "GMT"
        },
        {
            "transaction_id": "9LH22729BU904112R",
            "status": "Completed",
            "transaction_type": "Recurring Payment",
            "amount": {
                "currency": "USD",
                "value": "24.00"
            },
            "fee_amount": {
                "currency": "USD",
                "value": "-1.00"
            },
            "net_amount": {
                "currency": "USD",
                "value": "23.00"
            },
            "payer_email": "sb-43elhk319008@personal.example.com",
            "payer_name": "John Doe",
            "time_stamp": "2020-01-31T16:45:15Z",
            "time_zone": "GMT"
        },
        {
            "transaction_id": "I-HCVNSYS1VELK",
            "status": "Canceled",
            "transaction_type": "Recurring Payment",
            "payer_name": "John Doe",
            "time_stamp": "2020-02-01T20:52:43Z",
            "time_zone": "GMT"
        }
    ],
    "recordsTotal": 3,
    "recordsFiltered": 3,
    "draw": "warp"
}
```

**Response Params**

- transactions `(array)`: all PayPal transactions (including status changes and payments)
- recordsTotal `(number)`: total number of records (transactions)
- recordsFiltered `(number)`: number of filtered records (will be the same as recordsTotal)
- draw `(string)`: draw string from request

### Error Response

**Condition** : If PayPal transactions are not available at the moment

**Code** : `400 BAD REQUEST`

**Content** :

```json
{
    "message": "There was an error",
    "error": "<Error information>"
}
```

## Generating invoice

**URL** : `https://api.iotaap.io/v1/payments/invoice`

**Method** : `GET`

**Auth required** : YES

**URL Params**

- invoice_date `(string)`: date to be shown on invoice (in format: `2020-01-31T16:40:46Z`)

**URL example**

`https://api.iotaap.io/v1/payments/invoice?invoice_date=2020-01-31T16:40:46Z`

### Success Response

**Code** : `200 OK`

!!! info "PDF response"
    System will generate invoice based on the current active billing agreement (if there is one) and send PDF as the response

### Error Response

**Condition** : If invoice is requested when user has free model active

**Code** : `400 BAD REQUEST`

**Content** :

```json
{
    "message": "Cannot generate invoice for free model."
}
```

**Condition** : If there is no active model

**Code** : `404 NOT FOUND`

**Content** :

```json
{
    "message": "No such model"
}
```

**Condition** : If user resource is not available at the time

**Code** : `404 NOT FOUND`

**Content** :

```json
{
    "message": "No user data available"
}
```

## Create new subscription agreement (activate subscription)

This will initiate the process of creating a new PayPal subscription agreement.

!!! info "Redirection"
    Almost every step of this process is automated. This `GET` request should be called in the new window,
    but with provided Auth headers. 

**URL** : `https://api.iotaap.io/v1/payments/create-agreement`

**Method** : `GET`

**Auth required** : YES

**URL Params**

- model `(number)`: type of the model to be activated (1-4)

**URL example**

`https://api.iotaap.io/v1/payments/create-agreement?model=2`

### Success Response

**Code** : `200 OK`

**Response example**

!!! info "Redirected process"
    After successfull authorization PayPal will return token and agreement will be executed
    automatically, only after successfull execution backend will respond.

```json
{
    "message": "Billing Agreement Created Successfully"
}
```

**Code** : `200 OK`

**Response example**

!!! warning "Canceled authorization"
    After canceled authorization PayPal will return token and agreement will not be executed.

```json
{
    "message": "Agreement creation canceled."
}
```

### Error Responses

**Condition** : If user tries to activate already activated subscription model

**Code** : `400 BAD REQUEST`

**Content** :

```json
{
    "message": "Subscription already activated"
}
```

## Cancel subscription

System will automatically cancel current PayPal subscription agreement

**URL** : `https://api.iotaap.io/v1/payments/cancel-agreement`

**Method** : `POST`

**Auth required** : YES

### Success Response

**Code** : `200 OK`

**Response example**

```json
{
    "message": "Cancelled"
}
```

### Error Responses

**Condition** : If user information is not available at the moment

**Code** : `400 BAD REQUEST`

**Content** :

```json
{
    "message": "There was an error",
    "error": "<Error information>"
}
```