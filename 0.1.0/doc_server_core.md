# EXC Core #


## Extendable Objects ##

The **trait** ```\exc\core\objectExtendable``` allows you to add methods to an object dynamically at runtime using php anonymous functions.

```php
<?php
class myClass {
	use \exc\core\objectExtendable;
}

$o = new myClass();

//add a method to our object
$o->addMethod('doThis', function(){
	error_log("called do this...");
});

$o->doThis(); //lets call our new method...

?>
```

### Using delegation with objectExtendable ###
An object that uses the trait ```\exc\core\objectExtendable``` has two helper functions to obtain and set delegates.

The method ```delegateFor($name)``` will return an anonymous function for an object's method. The anonymous function returned still executes under the scope of the owner object. Things like ```$this``` will still work and would point to the owner instance.  

The method ```delegate($name, $fn)``` allows you to set or add a method to an object that uses objectExtendable.

```php
<?php
class myClass {
	use \exc\core\objectExtendable;
}
class myHelperClass {
	use \exc\core\objectExtendable;
	public $name = '';
	public function doThis(){
		//$this points to the instance of myHelperClass
		$this->setName('Joe');
		error_log("Name is " . $this->name);
	}
	public function setName($s){
		$this->name = $s;
	}
}

$a = new myClass();
$b = new myHelperClass();

//add the method doSomething() to $a, that will be implemente by $b->doThis()
$a->delegate("doSomething", $b->delegateFor('doThis'));

$a->doSomething(); //lets call our new method...

?>
```
