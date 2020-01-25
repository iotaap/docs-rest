# IoTaaP Console REST API

IoTaaP's API (Application Programming Interface) gives you the power to controll our software layer 
which runs behind the scenes. Managing and connecting resources is quick and easy. 

## General API information

API runs on a separate cloud instance and it can be accessed via [https://api.iotaap.io/v1](https://api.iotaap.io/v1) for API version 1.
This URL must be followed by the managed model (/user, /groups, etc.) or you will get the following error:

**Code** : `404 NOT FOUND`

**Content** :

```html
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="utf-8">
	<title>Error</title>
</head>

<body>
	<pre>Cannot GET /v1</pre>
</body>

</html>
```

## API Reference

Our API is resource-oriented, accepts JSON-encoded (application/json) by default or from-data (multipart/form-data) in special cases. API returns JSON-encoded 
responses, and uses standard HTTP response codes. 

!!! note "Future API versions"
	API might change in the future and you will be able to access new API versions using '/v2' or similar.
