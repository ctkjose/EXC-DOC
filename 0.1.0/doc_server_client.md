## Documentation Version 0.1.0 ##
[Topics](./doc_index.md)

## Handling requests from a client ##

Lets start with an example of a javascript code calling our backend:
```JavaScript
var a = new exc.app.backend.action("@(main.getShippingCost)");

var data = {'zipcode':'00603'};
exc.app.backend.sendAction(a, data).then(function(name, cost){
		console.log('@success...');
		console.log('cost=' + cost);

});
```
In this example we call the action "getShippingCost" of our "main" controller.

Notice that we setup a "success" callback with `then` to be executed when the action is completed. This callback expects two values a `name` and a `cost`.

To handle this action we add the following function to our controller:

```PHP
<?php
public event_action_getShippingCost($msg){

}
```
Our "main" controller now may look something like this.

```PHP
<?php
class mainController extends \exc\controller\viewController {
	public function initialize(){

	}
	public function unhandled($msg){

	}
	public event_action_getShippingCost($msg){

	}
}
```

Lets add some code to our handler function:
```php
<?php
public function event_action_getShippingCost(){
	$client = \exc\client::instance();

	//lets read values sent by the client
	$zipcode = $client->values['zipcode'];

    //here we explicitly set the values that we want to
	//call the callback
	$client->callback("USPS", 50.25);

	$client->done(); //we are done interacting with client
}
```

Not every call to a backend needs to use a promise or callback. For example we could call our backend and use **interactions** to send a response.

From the backend we can publish events using interactions. For example:

```php
<?php
	...
	$client->publish("updateShippingCost", {'carrier': 'USPS', 'cost': 50.25});

```
Lets refactor our `getShippingCost` handler to use `publish()` instead or callbacks:

```php
<?php
public function event_action_getShippingCost(){
	$client = \exc\client::instance();

	//lets read values sent by the client
	$zipcode = $client->values['zipcode'];

	$ok = true;
	... //some code here...

    if($ok){
		$client->publish("updateShippingCost", ['carrier'=> 'USPS', 'cost'=> 50.25]);
	}else{
		$client->publish("errorWithShippingCost", ['error'=> 'We do not ship to your zipcode!!!']);
	}

	$client->done(); //we are done interacting with client
}
```
Like you see we are now sending our response by publishing a message, in this case either `"updateShippingCost"` or `"errorWithShippingCost"`.

Now our call to the backend becomes just this:

```JavaScript
var a = new exc.app.backend.action("@(main.getShippingCost)");

var data = {'zipcode':'00603'};
exc.app.backend.sendAction(a, data);
```

The next step is to add our two new handlers:

```JavaScript
var myController = {

	event_updateShippingCost: function(msg){
		console.log('cost=' + msg.cost);
	},
	event_errorWithShippingCost: function(msg){
		alert(msg.error);
	}
}
```

### Getting parameters ###

A request from the client may include parameters for the request. Like for example:

```JavaScript
var a = new exc.app.backend.action("@(main.getShippingCost)");

a.params.client_id = '543245421';

var data = {'zipcode':'00603'};
exc.app.backend.sendAction(a, data);
```

In our php handler parameters are received as standard FORM values that we access using `$client->values`.

```php
<?php
$client_id = $client->values['client_id'];
```
