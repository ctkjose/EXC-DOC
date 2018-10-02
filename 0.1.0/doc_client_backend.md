## Documentation Version 0.1.0 ##
[Topics](./doc_index.md)

## Interacting with the backend ##

In the client side of your application we use the functionality provided by `exc.app.backend` to interact with your backend.

The basic way to interact with the backend is by means of actions.

**RELATED**<br>
[ Interacting with the client](./doc_server_client.md)

### Create an action ###

```JavaScript
var a = exc.backend.action("@(main.test)");
```

In EXC the simplest way to interact is using an action selector. In our example we use  `@(main.test)`.

An action selector is indicated by the `"@()"`. The arguments are the name of a controller followed by the action we want to send to the controller.

In our case the controller is `"main"` and the action is `"test"`.  

An action may include optional parameters that are send as traditional form data using HTTP POST.

```JavaScript
a.params.name = "Jose";
a.params.qty = 5;
```

We use the function `exc.app.backend.sendAction()` to send our action to the backend.

```JavaScript
exc.app.backend.sendAction(a);
```
We can include data to our backend using the data argument of `sendAction`.

```JavaScript
var data = {
	name = "Jose",
	qty = 5,
	zipcode="00603"
};
exc.app.backend.sendAction(a, data);
```

You can specify a success callback and a failure callback.

```JavaScript
var data = {
	name = "Jose",
	qty = 5,
	zipcode="00603"
};

exc.app.backend.sendAction(a, data, function(state){
	//success
	var shippingCost = state.data.cost;
},
function(){
	//failure
	alert("Unable to compute shipping costs...");
},
);
```

The function `sendAction()` returns a promise.

```JavaScript
var promise = exc.app.backend.sendAction(a, data);
```

Use the `then` method of the promise to set a a callback to execute when action is completed.
```JavaScript
exc.app.backend.sendAction(a, data).then(function(state){
	//success
	var shippingCost = state.data.cost;
});
```

With `then` you can simply specify the name of an event that you want to be triggered.

```JavaScript
exc.app.backend.sendAction(a, data).then("eventToPublish");
```
You may also specify a method of a controller object to execute.
```JavaScript
exc.app.backend.sendAction(a, data).then("@(myController.updateCosts)");
```

See [Interacting with the client](./doc_server_client.md) for details on how to handle actions send to the backend.
