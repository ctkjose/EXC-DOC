## Documentation Version 0.1.0 ##
[Documentation Index](./doc_index.md)<br>

# Request Object #
**Object:** exc.io.request[[br]]
**File:** exc.io.js[[br]]


The request object provides the basic functionality to perform AJAX/REST requests and fetch content from servers.




 ## Requesting JSON data from a backend server ##

Use the factory function `json` to request JSON data.

| json( url [,data] ) | returns request object |
| ------------------- | ---------------------- |
| url | A string containing the url |
| data | A plain object or string |


```javascript
	exc.io.request.json("http://localhost/test.php").response(function(data){
			console.log("json data recieved");
			console.log(data);
	});
```

The `response` takes a callback function as argument that will be executed when the json data is available. The callback must have one parameter for the json data.

You can also add a callback for `.done()`, `.fail()` and `.always()`.

A `.done()` will be executed when a request is successful. A `.fail()` callback will be executed when it fails. The callback `.always()` is executed upon completion of the request regardless of the status.

**NOTE:** When loading JSON data a `fail()` will be triggered by malformed json data.

You can also specify a plain object with the options to make a request. This apply to all the methods implemented by request.


| Options | Description |
| --- | --- |
| type | A string with "GET" or "POST". Default is "POST" |
| dataType | A string with "script", "text", "json", "jsonstream", "binary", "stream" |
| url | A string with the url |
| cache | A boolean. Default is false |
| headers | A plain object of key value pairs representing additional HTTP headers. |
| user | The optional user name to use for authentication purposes; by default, this is an empty string. |
| password | The optional password to use for authentication purposes; by default, this is an empty string. |
| timeout | The number of milliseconds a request can take before automatically being terminated. A value of 0 (which is the default) means there is no timeout. |

### Example using options ###
 ```javascript
	var options = {
		url: "http://localhost/test/test.php",
		timeout: 2000,
	};

	var request = exc.io.request.json(options);
	request.done(function(request){
			console.log(request.data); //json data
	});
 ```

### Example sending data ###
```javascript
	var params = {empId: 5006, empName: "Jose Cuevas"};

	exc.io.request.json("http://localhost/test.php", params).response(function(data){
			console.log("json data recieved");
			console.log(data);
	});
```


## Fetching javascript code from a backend server and executing the code. ##

Similar to the function `.json()` the `request` object implements the `.script()` function. It behaves exactly the same but it expects to received a string with valid javascript code. This code is executed automatically in the global context.

```javascript
	exc.io.request.script("http://localhost/test.php").done(function(request){
			console.log("script executed");
	}).fail(function(request){
			console.log("script failed");
	});
```

An example of your `test.php` could be:
```php
<?php
header("content-type: application/javascript; charset=UTF-8");
//javascript code...
print "alert('hello world...');\n";
exit;
?>
```


## Fetching large sets of JSON data and processing it as it arrives. ##

In many instances we want to fetch a fairly large data set but without locking out the user interface.

You can request a `jsonstream` an process an incremental feed of JSON records.

The backend can send multiple JSON records separated by a newline `\n`.  When ever a whole JSON record is received the `response` callback will be executed. The `data` parameter points to the JSON record that is available. The `response` callback will continue to be executed for each record received by the backend.

```javascript
	exc.io.request.jsonstream("http://localhost/test.php").response(function(data){
			console.log("json entry recieved");
			var s = "<option value='" + data.tag + "'>" + data.caption + "</>";
			$(".categories").append(s);
	});
```

An example of your `test.php` could be:

```php
<?php

header("content-type: application/json; charset=UTF-8");
//load some large data...
while($rs->read()){
	print json_encode( $rs->fields ) . "\n";
	flush();
}
exit;
?>
```
