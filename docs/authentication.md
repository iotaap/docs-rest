# Authentication

Initial authentication is based on email and password. After initial documentation
Bearer token will be returned and it must be included in the header of every request.

- Bearer Token expires every 30 days

## Basic Auth

**URL** : `https://api.iotaap.io/v1/auth/login`

**Method** : `POST`

**Body Params**

- email `(string)`: valid e-mail
- password `(string)`: valid password
- rememberMe `(bool)`: true/false (optional)

**Body example**

```json
{
    "email": "your@email.com",
    "password": "yourPassword",
    "rememberMe": "false"
}
```

### Success Response

**Code** : `200 OK`

**Response example**

```json
{
    "user": {
        "id": "5e10fc5e77ba18070312e171",
        "role": "user"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInzwqaI6IkpXVCJ9.eyJpZCI6IjVlMTBmYzVlNzdiYTE4MDcwMzEyZTE3MSIskeq4eGUiOiJhZG1pbiIsImlhdCI6MTU3OTQ0MjI1NSwiZXhwIjoxNTgyMDM0MjU1fQ.7EUmUzwqbcR35asesYwpsjy2PVAb8BC2ETPipT-gjZq8"
}
```

### Error Response

**Condition** : If 'email' and 'password' combination is wrong

**Code** : `401 UNAUTHORIZED`

**Content** :

```json
{
    "error": "Login or password is wrong"
}
```

## Token Auth

Every request must include Bearer token in the header for successfull authentication. 

**Header Params**

- Authorization `(string)`: valid Bearer token

**Header Param Example**

```json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInsfcCI6IkpXVCJ9.eyJpZCI6IjVlMTBmYzVlNzdiYTE4MDcwMzEyZTE3MSIsInJvbGUiOswhZG1pbiIsImlhdCI6MTU3OTQ0MTA2OSwiZXhwIjoxNTgyMDMzMDY5fQ.OCXq05RLQv4xQsxxrtTZYk3OZ4jEu9vnwAQORYHlqRw
```

### Success Response

Success response for the called method

### Error Response

**Condition** : If Bearer token is not valid

**Code** : `401 UNAUTHORIZED`

**Content** :

```json
Unauthorized
```