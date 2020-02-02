# Groups

User can manage only his groups. Groups are giving the possibility to manage and upgrade
multiple devices at once. 

## Get all groups

**URL** : `https://api.iotaap.io/v1/groups`

**Method** : `GET`

**Auth required** : YES

**URL Params**

- search_value `(string)`: filtering value string
- offset `(number)`: number of records to skip
- limit `(number)`: number of records to return
- draw `(string)`: value will be returned back in the response
- order_dir `(string)`: asc/desc

### Success Response

**Code** : `200 OK`

**Response example**

```json
{
    "groups": [
        {
            "_id": "5e1b76bc4315b70eb242567a",
            "name": "cooling_system",
            "token": "AATluvfLURijoH9d",
            "created_at": "2020-01-12T19:42:52.504Z",
            "owner": "5e10fc5e77ba18070312e171",
            "__v": 0
        },
        {
            "_id": "5e1bcd259e233933d7f0ec2c",
            "name": "heating_system",
            "token": "JGGnkJ4gO9zfW4gR",
            "created_at": "2020-01-13T01:51:33.651Z",
            "owner": "5e10fc5e77ba18070312e171",
            "__v": 0
        }
    ],
    "recordsTotal": 6,
    "recordsFiltered": 2,
    "draw": "warp"
}
```

**Response Params**

- groups `(array)`: array of group objects
- recordsTotal `(number)`: total number of records (groups)
- recordsFiltered `(number)`: number of records that match given filter
- draw `(string)`: draw string from request

### Error Response

**Condition** : If groups resources are not available at the moment

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Error when getting groups",
    "error": "<Error information>"
}
```

## Get specific group

**URL** : `https://api.iotaap.io/v1/groups/{group-id}`

**Method** : `GET`

**Auth required** : YES

**Path Params**

- {group-id} `(string)`: specific group ID

**URL example**

`https://api.iotaap.io/v1/groups/5e1b76bc4315b70eb242567a`

### Success Response

**Code** : `200 OK`

**Response example**

```json
{
    "_id": "5e1b76bc4315b70eb242567a",
    "name": "cooling_system",
    "token": "AATluvfLURijoH9d",
    "created_at": "2020-01-12T19:42:52.504Z",
    "owner": "5e10fc5e77ba18070312e171",
    "__v": 0
}
```

### Error Response

**Condition** : If group does not exist or current user is not the owner

**Code** : `404 NOT FOUND`

**Content** :

```json
{
    "message": "No such group"
}
```

**Condition** : If group resources are not available at the moment

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Error when getting group",
    "error": "<Error information>"
}
```

## Get group devices

This will return all devices from the specific group. 

!!! info "Add/Remove device"
    In order to add or remove device from/to the group you need to update device `group` field.
    More info can be found in [Devices category](/rest-api/devices/#update-existing-device)

**URL** : `https://api.iotaap.io/v1/groups/devices/{group-id}`

**Method** : `GET`

**Auth required** : YES

**URL Params**

- search_value `(string)`: filtering value string
- offset `(number)`: number of records to skip
- limit `(number)`: number of records to return
- draw `(string)`: value will be returned back in the response
- order_dir `(string)`: asc/desc

**Path Params**

- {group-id} `(string)`: specific group ID

**URL example**

`https://api.iotaap.io/v1/groups/devices/5e1b76bc4315b70eb242567a`

### Success Response

**Code** : `200 OK`

**Response example**

```json
{
    "devices": [
        {
            "_id": "5e1b5774afd4e404d68aebd6",
            "type": "Magnolia",
            "name": "cooling_1",
            "token": "iIP2C3D4B07O",
            "last_contact": null,
            "created_at": "2020-01-12T17:29:24.180Z",
            "owner": "5e10fc5e77ba18070312e171",
            "__v": 0,
            "group": "5e1b76bc4315b70eb242567a"
        },
        {
            "_id": "5e27048a8b48ba0a780f3124",
            "type": "Magnolia",
            "name": "cooling_2",
            "token": "nbyMJBdp8VR5",
            "last_contact": null,
            "created_at": "2020-01-21T14:02:50.500Z",
            "owner": "5e10fc5e77ba18070312e171",
            "group": "5e1b76bc4315b70eb242567a",
            "__v": 0
        }
    ],
    "recordsTotal": 2,
    "recordsFiltered": 2,
    "draw": "warp"
}
```

**Response Params**

- devices `(array)`: array of device objects in specified group
- recordsTotal `(number)`: total number of records (devices)
- recordsFiltered `(number)`: number of records that match given filter
- draw `(string)`: draw string from request

### Error Response

**Condition** : If group does not exist or current user is not the owner

**Code** : `404 NOT FOUND`

**Content** :

```json
{
    "message": "No such group"
}
```

**Condition** : If group resources are not available at the moment

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Error when getting group",
    "error": "<Error information>"
}
```

**Condition** : If device resources are not available at the moment

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Error when getting devices",
    "error": "<Error information>"
}
```

## Create new group

System will automatically generate new group credentials if there are available groups
in the current billing plan

**URL** : `https://api.iotaap.io/v1/groups`

**Method** : `POST`

**Auth required** : YES

**Body Params**

- name `(string)`: group name

!!! info "Random name"
    If `name` parameter is not provided system will automatically generate random 10 characters
    name for your group

**Body example**

```json
{
	"name": "my_cooling_system"
}
```

### Success Response

**Code** : `201 CREATED`

**Response example**

```json
{
    "_id": "5e277a1c97f08a0e2d5adf30",
    "name": "my_cooling_system",
    "token": "19gx6mBdcUhxkffD",
    "created_at": "2020-01-21T22:24:28.849Z",
    "owner": "5e10fc5e77ba18070312e171",
    "__v": 0
}
```

### Error Responses

**Condition** : If user billing model type doesn't allow group creation

**Code** : `403 FORBIDDEN`

**Content** :

```json
{
    "message": "Groups are not available in free model. Please upgrade your account."
}
```

**Condition** : If user information is not available at the moment

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Error when creating group",
    "error": "<Error information>"
}
```

## Update existing group

**URL** : `https://api.iotaap.io/v1/groups/{group-id}`

**Method** : `PUT`

**Auth required** : YES

**Path Params**

- {group-id} `(string)`: specific group ID

**URL example**

`https://api.iotaap.io/v1/groups/5e277a1c97f08a0e2d5adf30`

**Body Params**

- name `(string)`: group name

**Body example**

```json
{
	"name": "my_new_cooling_system"
}
```

### Success Response

**Code** : `200 OK`

**Response example**

```json
{
    "_id": "5e277a1c97f08a0e2d5adf30",
    "name": "my_new_cooling_system",
    "token": "19gx6mBdcUhxkffD",
    "created_at": "2020-01-21T22:24:28.849Z",
    "owner": "5e10fc5e77ba18070312e171",
    "__v": 0
}
```

### Error Responses

**Condition** : If group does not exist or current user is not the owner

**Code** : `404 NOT FOUND`

**Content** :

```json
{
    "message": "No such group"
}
```

**Condition** : If groups information is not available at the moment

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Error when updating group",
    "error": "<Error information>"
}
```

## Remove group

**URL** : `https://api.iotaap.io/v1/groups/{group-id}`

**Method** : `DELETE`

**Auth required** : YES

**Path Params**

- {group-id} `(string)`: specific group ID

**URL example**

`https://api.iotaap.io/v1/groups/5e277a1c97f08a0e2d5adf30`

!!! warning "Group devices"
    When group is removed, all devices from the group will be automatically updated and
    `group` parameter will be removed from these records.

### Success Response

**Code** : `200 OK`

**Response example**

```json
{
    "message": "Group successfully removed"
}
```

### Error Responses

**Condition** : If group doesn't exist or current user is not the owner

**Code** : `404 NOT FOUND`

**Content** :

```json
{
    "message": "No such group"
}
```

**Condition** : If group ID is not valid

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Group ID is not valid"
}
```

**Condition** : If groups or devices resource is not available at the time

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Error when removing the group",
    "error": "<Error information>"
}
```