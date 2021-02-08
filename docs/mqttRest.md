# MQTT REST Bridge

MQTT REST Bridge gives user the possiblity to send `POST` request to the server and transfer payload to the specific topic. 

## Send request

Send data to the specific topic using options and credentials

**URL** : `https://api.iotaap.io/v1/mqtt-rest`

**Method** : `POST`

**Auth required** : YES

**Body Params**

- username `(string)`: MQTT Instance username
- password `(string)`: MQTT Instance password
- topic `(stirng)`: Topic to transfer data to
- retain `(string)`: true/false
- qos `(string)`: QoS (0,1,2) 
- message `(string)/(number)/(object)`: Payload
- server `(string)`: MQTT Instance Server

!!! info "Retained message"
    If `retain` parameter is not provided, default value of `false` will be used.

!!! info "QoS"
    QoS (Quality of Service) can be configured in this request. If `qos` parameter is 
    not provided, default value of `0` will be used.

**Body example `number`**

```json
{
	"username" : "oLP6MA9r",
	"password" : "abdI2FxeHStx",
	"topic" : "/motor-speed",
	"retain": true,
	"qos": 2,
	"message" : 23,
    "server" : "mqtt2.iotaap.io"
}
```

**Body example `string`**

```json
{
	"username" : "oLP6MA9r",
	"password" : "abdI2FxeHStx",
	"topic" : "/motor-speed",
	"message" : 23,
    "server" : "mqtt2.iotaap.io"
}
```

**Body example `object`**

```json
{
    "username": "oLP6MA9r",
    "password": "abdI2FxeHStx",
    "topic": "/motor-speed",
    "message": {
        "start": 80,
        "stop": 23,
        "idle": "min",
        "regulator": {
            "P": 23,
            "I": 56,
            "D": 10
        }
    },
    "server" : "mqtt2.iotaap.io"
}
```

!!! info "Root topic"
    Root topic (your username) is automatically included in the request. You only have to pass the target topic(s).
    In the above example, data will be published to the topic `/oLP6MA9r/motor-speed`. **Leading `/` must be included**.

### Success Response

**Code** : `200 OK`

**Response example**

```json
{
    "message": "Message published"
}
```

### Error Responses

**Condition** : If system timeouted

**Code** : `400 BAD REQUEST`

!!! info "Payload transfer timeout"
    If there is no positive response from the MQTT server, or there was an issue with
    initial server connection (wrong server, wrong credentials, etc.), message transfer
    request will be terminated after **8000ms**.

**Content** :

```json
{
    "message": "There was a problem connecting MQTT server"
}
```

**Condition** : If mandatory parameter is missing, if Group ID is not valid, or if file is not uploaded

**Code** : `400 BAD REQUEST`

**Content** :

```json
{
    "message": "<field-name> is a mandatory field"
}
```

**Condition** : If there was an issue with MQTT server communication

**Code** : `500 INTERNAL SERVER ERROR`

**Content** :

```json
{
    "message": "There was an error",
    "error": "<Error information>"
}
```