# User

User can manage only his own data. Every user is recognized by Bearer token provided in the Authentication header. 

## Get current user

**URL** : `https://api.iotaap.io/v1/users/current`

**Method** : `GET`

**Auth required** : YES

### Success Response

**Code** : `200 OK`

**Response example**

```json
{
    "id": "5e10fc5effba18cc0f12e111",
    "email": "my@email.com",
    "role": "user",
    "firstName": "Doc",
    "lastName": "Brown",
    "address": {
        "street": "1640 Riverside Drive",
        "city": "Hill Valley, California",
        "zipCode": "90202"
    },
    "company": "Future is back Ltd.",
    "vatId": "98765432112",
    "settings": {
        "themeName": "dark"
    }
}
```

### Error Response

**Condition** : If settings are not available for the current user

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Error when getting user data",
    "error": "<Error information>"
}
```

## Update current user

**URL** : `https://api.iotaap.io/v1/users/current`

**Method** : `PUT`

**Auth required** : YES

**Body Params (all are optional)**

- email `(string)`: updated email
- firstName `(string)`: updated first name
- lastName `(string)`: updated last name
- address `(object)`: 
    - street `(string)`: updated street
    - city `(string)`: updated city
    - zipCode `(string)`: updated zip code
- company `(string)`: updated company name
- vatId `(string)`: updated VAT ID

**Body example**

```json
{
    "email": "mynew@email.com",
    "firstName": "Jacky",
    "lastName": "Jones",
    "address": {
        "street": "83 Beacon Street",
        "city": "Boston",
        "zipCode": "94256"
    },
    "company": "Cosby Creations Ltd.",
    "vatId": "64187521641"
}
```

### Success Response

**Code** : `200 OK`

**Response example**

```json
{
    "message": "User successfully updated"
}
```

### Error Responses

**Condition** : If user cannot be found

**Code** : `404 NOT FOUND`

**Content** :

```json
{
    "message": "No such user"
}
```


**Condition** : If user information is not available at the moment

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Error when updating user",
    "error": "<Error information>"
}
```