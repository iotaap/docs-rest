# Settings

Settings age giving the possibility to the user to customize their app experience. 

## Get settings

**URL** : `https://api.iotaap.io/v1/settings/current`

**Method** : `GET`

**Auth required** : YES

### Success Response

**Code** : `200 OK`

**Response example**

```json
{
    "themeName": "dark"
}
```

### Error Response

**Condition** : If settings are not available for the current user

**Code** : `404 NOT FOUND`

**Content** :

```json
{
    "message": "No settings data available"
}
```

**Condition** : If settings are not available for the current user

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Error when getting settings data",
    "error": "<Error information>"
}
```

## Update settings

**URL** : `https://api.iotaap.io/v1/settings/current`

**Method** : `PUT`

**Auth required** : YES

**Body Params**

- themeName `(string)`: "default/dark"

**Body example**

```json
{
	"themeName": "default"
}
```

### Success Response

**Code** : `200 OK`

**Response example**

```json
{
    "themeName": "default"
}
```

### Error Response

**Condition** : If settings are not available for the current user

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Error when getting settings data",
    "error": "<Error information>"
}
```