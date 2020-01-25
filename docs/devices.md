# Devices

User can manage only his devices. System will automatically generate new device
parameters. If you are looking for OTA updates, please check 'Devices OTA'.

## Get all devices

**URL** : `https://api.iotaap.io/v1/devices`

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
    "devices": [
        {
            "_id": "5e1a5ecfe2ac3a3bdb45fcc5",
            "type": "Magnolia",
            "name": "spaceship_controller",
            "token": "y1LV50tJkqZz",
            "last_contact": null,
            "created_at": "2020-01-11T23:48:31.148Z",
            "owner": "5e10fc5e77ba18070312e171",
            "__v": 0
        },
        {
            "_id": "5e1a5faee2ac3a3bdb45fcc7",
            "type": "Magnolia",
            "name": "rocket_engine",
            "token": "xA2KFbFshxf9",
            "last_contact": null,
            "created_at": "2020-01-11T23:52:14.431Z",
            "owner": "5e10fc5e77ba18070312e171",
            "__v": 0,
            "group": "5e1b521970170d025fc2ba06"
        }
    ],
    "recordsTotal": 15,
    "recordsFiltered": 2,
    "draw": "warp"
}
```

**Response Params**

- devices `(array)`: array of device objects
- recordsTotal `(number)`: total number of records (devices)
- recordsFiltered `(number)`: number of records that match given filter
- draw `(string)`: draw string from request

### Error Response

**Condition** : If devices resources are not available at the moment

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Error when getting devices",
    "error": "<Error information>"
}
```

## Get specific device

**URL** : `https://api.iotaap.io/v1/devices/{device-id}`

**Method** : `GET`

**Auth required** : YES

**Path Params**

- {device-id} `(string)`: specific device ID

**URL example**

`https://api.iotaap.io/v1/devices/5e1a5ecbe2ac3a3bdb45fcc4`

### Success Response

**Code** : `200 OK`

**Response example**

```json
{
    "_id": "5e1a5ecbe2ac3a3bdb45fcc4",
    "type": "Magnolia",
    "name": "warp_machine",
    "token": "HlQeS1oKcVYx",
    "last_contact": null,
    "created_at": "2020-01-11T23:48:27.322Z",
    "owner": "5e10fc5e77ba18070312e171",
    "__v": 0
}
```

### Error Response

**Condition** : If device does not exist or current user is not the owner

**Code** : `404 NOT FOUND`

**Content** :

```json
{
    "message": "No such device"
}
```

**Condition** : If devices resources are not available at the moment

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Error when getting devices",
    "error": "<Error information>"
}
```

## Create new device

System will automatically generate new device credentials if there are available devices
in the current billing plan

**URL** : `https://api.iotaap.io/v1/devices`

**Method** : `POST`

**Auth required** : YES

**Body Params**

- name `(string)`: device name

!!! info "Random name"
    If `name` parameter is not provided system will automatically generate random 10 characters
    name for your device

**Body example**

```json
{
	"name": "my_machine"
}
```

### Success Response

**Code** : `201 CREATED`

**Response example**

```json
{
    "_id": "5e24a2d083f4ab0549cc4754",
    "type": "Magnolia",
    "name": "my_machine",
    "token": "joWok7C98EYR",
    "last_contact": null,
    "created_at": "2020-01-19T18:41:20.685Z",
    "owner": "5e10fc5e77ba18070312e171",
    "__v": 0
}
```

### Error Responses

**Condition** : If user information is not available at the moment

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Error when creating device",
    "error": "<Error information>"
}
```

## Update existing device

**URL** : `https://api.iotaap.io/v1/devices/{device-id}`

**Method** : `PUT`

**Auth required** : YES

**Path Params**

- {device-id} `(string)`: specific device ID

**URL example**

`https://api.iotaap.io/v1/devices/5e1a5ecbe2ac3a3bdb45fcc4`

**Body Params**

- name `(string)`: device name
- group `(string)`: group ID

**Body example**

```json
{
	"name": "my_new_machine",
	"group": "5e1b76bc4315b70eb2425679"
}
```

!!! warning "Group parameter"
    Notice that `group` parameter is optional. If Group ID is wrong or user is not the owner of the group this
    parameter will be ignored but device name will be updated. If `group` parameter is valid in the background our
    system will add `group` parameter to the device, and by that device will become a part of the specified group.

!!! info "Removing device from the group"
    In order to remove specific device from the group you must pass the `group` parameter with the **string** value: `undefined`.
    In the background our system will remove group parameter from the device and by that device will not be a part of any group.

    **Example:**
    ```json
    {
	"group": "undefined"
    }
    ```

### Success Response

**Code** : `200 OK`

**Response example**

```json
{
    "_id": "5e24a2d083f4ab0549cc4754",
    "type": "Magnolia",
    "name": "my_new_machine",
    "token": "joWok7C98EYR",
    "last_contact": null,
    "created_at": "2020-01-19T18:41:20.685Z",
    "owner": "5e10fc5e77ba18070312e171",
    "__v": 0
}
```

### Error Responses

**Condition** : If device does not exist or current user is not the owner

**Code** : `404 NOT FOUND`

**Content** :

```json
{
    "message": "No such device"
}
```

**Condition** : If devices information is not available at the moment or Group ID is not valid

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Error when updating device",
    "error": "<Error information>"
}
```
```json
{
    "message": "Group ID is not valid"
}
```

## Remove device

**URL** : `https://api.iotaap.io/v1/devices/{device-id}`

**Method** : `DELETE`

**Auth required** : YES

**Path Params**

- {device-id} `(string)`: specific device ID

**URL example**

`https://api.iotaap.io/v1/devices/5e24a2d083f4ab0549cc4754`

### Success Response

**Code** : `200 OK`

**Response example**

```json
{
    "message": "Device successfully removed"
}
```

### Error Responses

**Condition** : If device doesn't exist or current user is not the owner

**Code** : `404 NOT FOUND`

**Content** :

```json
{
    "message": "No such device"
}
```

**Condition** : If user information is not available at the moment

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "Error when removing device",
    "error": "<Error information>"
}
```