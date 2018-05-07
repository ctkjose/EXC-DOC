## Documentation Version 0.1.0 ##
[Documentation Index](./doc_index.md)[[BR]]

## Getting Started ##

This guide applies to EXC Server Side Framework or SSF.

We will create a basic sample employee application to learn the basics of EXC.  Lets create a folder named `employee`  for our app.

### Your app definition ###

Every app needs an `app.php` file which tells EXC about your app. Inside `app.php` we use the `$options` array to configure our app.

```php
<?php

$options['uid'] = "ACMEEMP"; //our app identifier, every app needs an identifier...

//add some controllers controllers
$options['controllers']=[
	"record" => "./controller.record.php",
	"findEmployeeController" => "./controller.findEmployee.php",
];

?>
```




### Your controllers ###

The bread and butter of the your app are controllers. A traditional UI based application uses controllers of type `viewController`, services and cli applications use the `processController`. You will create instances of one of these to implement your app.

Lets create an instance of `viewController` for an employee record, our controller will be named "record".

Controllers must be created in a file by themselves,  we create a file named `controller.record.php` in our app folder.

Create an instance of `viewController` like this:

```php
class recordController extends \exc\controller\viewController {
	public function initialize(){
		//this is called when EXC creates the instance of your controller
	}
	public function action_main(){
		//this is our default action
	}
	public function action_show(){
		//show our employee record...
	}
	public function message_appInit(){
		//this is a message function
		//this function will be executed when an ```app_ready``` message is published
	}
	public function unhandled($msg){
		//message not implemented
		error_log('[ACME EMPLOYEE APP] MESSAGE [' . $msg . '] NOT IMPLEMENTED');
	}
}
```

In EXC a function in your controller is invoked by a **message** or an **action**. You can receive an **action** from your client side of the app or you may get a **message** by the framework itself.



Any controller can listen to a "message" by creating a message function. A message function is simply a function named `message_` + `message_name`.



For example when the app is ready a message `appReady` will be published, to execute code on the `appReady` message we just add the function `message_appReady` to our controller.

```php
	public function message_appReady(){
		//this is a message function
		//this function will be executed when an ```app_ready``` message is published
	}
```
See the section on messages to learn more on the different messages that EXC will publish and how to publish your own.

In the other hand the `client` can post an action to your server side controller, lets  said the action `showRecord` to show an employee record. A function for this action will be simply:
```php
	public function action_showRecord(){
		//my code here...
	}
```

#### Building the user interface ####
The user interface is build using views and EXC's built-in templating functionality.

A view is used to build your user interface. In EXC we can manipulate views from the server side prior to serving the content to the browser or dynamically in the client side.

A "view" is a php file named `view.myname.php`, where `myname` is the actual name of your view.

See [Views Documentation](./doc_server_views.md) to learn more about views.

You place your views in a folder named `views` in your app folder.

A view file contains html in combination with the templeting functionality.





### Messages ###

The following is a list of messages send by the framework.

| Message | Description |
| -- | -- |
| appInit | App initializing |
| appStart | App started running |
| appAbort | App aborted |
| appEnd | App ended |
| appSendHeaders | Send headers |
| appSendOutput | About to send output |
| ViewCommit | The view is about to be written to output |
