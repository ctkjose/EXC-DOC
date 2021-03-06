## Documentation Version 0.1.0 ##
[Documentation Index](./doc_index.md)<br>

## Controllers ##

This guide applies to EXC Server Side Framework or SSF.

The bread and butter of the SSF are controllers, they manage the lifecycle of your app.

### Controller Types ###

There are different types of controllers each with additional functionality depending on your type of application or tasks.

A traditional UI based application uses controllers of type `viewController`, services and cli applications use the `processController`. You will create instances of one of this to implement your app.

EXC creates other controllers in addition to yours. Your application itself is orchestrated by an instance of a `appController`.

### viewController ###

Lets create an instance of `viewController` for an employee record, our controller will be named "record".

Controllers must be created in a file by themselves,  we create a file named `controller.record.php` in our app folder.

Create an instance of `viewController` like this:

```php
<?php
class recordController extends \exc\controller\viewController {
	public function initialize(){
		//this is called when EXC creates the instance of your controller
	}
	public function event_action_main(){
		//this is our default message from the browser
	}
	public function event_appInit(){
		//this is a message function
		//this function will be executed when an ```appInit``` message is published
	}
	public function event_appStart(){
		//this is a message function
		//this function will be executed when an ```appStart``` message is published
	}
}
?>
```
