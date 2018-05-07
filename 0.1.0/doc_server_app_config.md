## Application Configuration ##

The file ```app.php``` is where we tell EXC about our application and configure it.

Place this file in the root folder of your application, for example:
```
  + /MyApp
    -  app.php
```

## Loading modules and includes ##



Add an entry to the array key ```$options['using']``` for each module you want to load.

Each entry in ```$options['using']``` points to a resource that you want to load. For example lets load the module ```exc.io.store```:

```
$options['using'] = [   //which modules to load
	"exc://exc.io.store"=> [
		"default_path" => "emr/records/patients/",
		"engines" => [
			[
				"engine"=>"MYSQL",
				"role"=>"master",
				"connection"=>["host"=>"127.0.0.1", "port"=>3366, "username"=>"exc.store","password"=>"anicepassword"]
			]
		]
	],
]
```

An entry (in our example ```"exc://exc.io.store"```)  points to an array of optional parameters/configurations passed to the module.


To tell **EXC** where to look for a resource we use URL protocols. **EXC** supports the following:

 Protocol | Description
 -------- | -----------
 ```exc://``` | A standard EXC module, located in ```\exc\server\```.
 ```app://``` | An app specific module, located in your app folder.
 ```file://``` | A php include, the path relative to document root must be provided.

#### Example loading an app module ####

Lets load the class ```\emr\record``` in the file ```emr.record.php``` located in our app folder.

```
$options['using'] = [
	//example of a module in the app domain
    "app://emr.record" => [  //an app-specific module, "emr.record" is the namespace "\emr" and maps to the class "\emr\record"
        //exc will attempt to execute the static method initialize() for a class named manager if one is found.
        "enable_inmuno"=> true, //optional value-pairs passed to manager::initialize($params) as an array in the $params argument
    ],

];
```
#### Example loading a php include/require ####

Lets load a php file named ```myinclude.php``` located in the folder ```myapp/lib/```.

Paths are relative to your document root.

```
$options['using'] = [
    "file://myapp/lib/myinclude.php" => [],
];
```

Since in this example the file is under your app folder you could also use the following to load ```myinclude.php```:

```
$options['using'] = [
    "app://lib/myinclude.php" => [],
];
```

#### Example loading composer ####

Lets load composer support for our app.

```
$options['using'] = [
    "app://vendor/autoload.php"=>[],
];
```



#### Other examples ####

```
$options['using'] = [   //which modules to load
	"exc://exc.io.store"=> [
		"default_path" => "emr/records/patients/",
		"engines" => [
			[
				"engine"=>"MYSQL",
				"role"=>"master",
				"connection"=>["host"=>"127.0.0.1", "port"=>3366, "username"=>"exc.store","password"=>"webexc2013"]
			]
		]
	],
	"exc://exc.ui"=>[
		"views.paths"=>[
			'./views/',
			'../test02B/views/',
			'../test/views/'
		]
	],
	//example of a module in the app domain
	"app://emr.record" => [  //an app-specific module, "emr.record" is the namespace "\emr" and maps to the class "\emr\record"
		//exc will attempt to execute the static method initialize() for a class named manager if one is found.
		"enable_inmuno"=> true, //optional value-pairs passed to manager::initialize($params) as an array in the $params argument
	],

	//examples of hot to include a file
	//"app://test_include.php" => [], //will include this php from the app folder
	//"file://exc/test02A/test_include.php" => [], //will include a php in a particular path relative to doc root

	//load composer support
	//"app://vendor/autoload.php"=>[],

];
```
