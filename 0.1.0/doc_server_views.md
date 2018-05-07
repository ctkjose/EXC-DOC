## Views ##

A view is used to build your user interface. In EXC we can manipulate views from the server side prior to serving the content to the browser or dinamically in the client side.

A **view** is a php file named `view.myname.php`, where `myname` is the actual name of your view.

> **NOTE:** The view can be an HTML file like  `view.myname.html`.

You place your view files in a folder named `views` in your app folder. This is where EXC would look for them by default.

### Creating a view ###

A view file contains the actual HTML code, it is our template, like for example:

```HTML
<!doctype html>
<html>
<head>
	<title>{{title}}</title>
</head>
<body class='layout'>
Hello {{username}}...<br>
{{contents}}
</body>
</html>
```
You can add value placeholders that you replace from PHP. For example a placeholder for a "username" is defined as `{{username}}`.

In the example above we have the placeholders `{{username}}`, `{{contents}}` and `{{title}}`.

In PHP we set the value of a placeholder with the `set($html)` method:

```PHP
$username = "supercoolcat99";
$view->username->set($username);
```

You can append to a placeholder with the `write($html)` method:
```PHP
$view->contents->write($someHTML);
$view->contents->write($moreHTML);
```

### The default view ###

An EXC app has a main view called the **default view**. The contents of your default view will be send to the browser. You load a view as your default view using the method `createDefaultView(name)`:

```PHP
$view = \exc\ui\views\manager::createDefaultView("myname");
```

Once a default view is set you can obtain the default view with:
```JS
$view = \exc\ui\views\manager::getDefaultView();
```

You can get a view other than the default view using:

```
$view = \exc\ui\views\manager::getView("student_record");
```

### Beyond placeholders ###



You can set values that can be used as placeholders in all your views using a **@CONSTANT**.

You can add a constant value in PHP like this:

```PHP
$location = 'RUM';
\exc\ui\views\view::constantSetValue("location", $location);
```
In your view file a constant placeholder is available as:

```
{{@location}}
```

Special framework values are available as @CONTANTS in your view file.

| NAME | DESCRIPTION |
| -- | -- |
| @URL_BASE | URL path of your app. |
| @URL_CONTROLLER_DIRECTORY | URL path to the folder with current controller. |
| @URL_CONTROLLER | URL pointing to the current controller. |
| @BASE | Path of your app. |
| @CONTROLLER_DIRECTORY | Path to the folder with current controller. |


Directives are another type of placeholders that make your view more flexible.

Use the **#view** directive to include the contents of another view:
```HTML
{{#view record_header}}
```

Use the **#file** directive to include a file:

```html
{{#file /absolute/path/file.js}}
```
You can use `@CONSTANTS` with **#file**, for example the constants `@BASE` and `@CONTROLLER_DIRECTORY`:

```HTML
{{#file @BASE/file.js}}

{{#file @CONTROLLER_DIRECTORY/file.js}}
```

The **#if** directive will include its contents when the value of a variable evaluates to true.
```HTML
{{#if ind_new_account}}
	<b>Welcome to EXC...</b>
{{end if ind_new_account}}
```






### Placing views in other locations ###

In many instances you may want to place your views in another folder or even share views between apps.

You can tell EXC where to look for views in your **app definition** using the option `$options['views.paths']`. Set `$options['views.paths']` to a string with the path to a folder with your views.

```php
$options['views.paths'] = '../employee_common/views/';
```

> **NOTE:** *A path can be an absolute path or relative to your application.*

You can also use an array with multiple paths as needed:
```php
$options['views.paths'] = [
	'../employee_common/views/',
	'/Library/WebServer/Documents/examples/shared/views/',
];
```


### Views and Modules ###

EXC knows that you intend to build or interact with an UI if the controller running is an instance of `viewController`, but also you can explicitly tell EXC to load the UI by adding the module `"exc://exc.ui"` in your app definition.

In your **app definition** you can specify the modules that you want to use in your app with the option `$options['using']`.

When adding the ui module you can also specify the location of your views, for example:

```php
$options['using'] = [
    "exc://exc.ui"=>[
		"views.paths"=>[
			'../employee_support/views/',
			'../shared/views/'
		]
	],
];
```


## Class exc\ui\views\view ##

### Class Methods ###
```
public static function createWithFile($path, $name='default')
```

Factory method to create an instance of `view` from a file.

```
public static function createWithSource($source, $name='default')
```
Factory method to create an instance of `view` from a $source string containing HTML.


```
public static function constantSetValue($key, $value)
public static function constantSetValue($key1, $value1, $key2, $value2...)
public static function constantSetValue($anArrayOfValuePairs)
```
Sets the value of a **@CONTANT** placeholder.

```
public static function constantCopyValues($anArrayOfValuePairs, $prefix='')
```
Creates a **@CONTANT** placeholder for each key in an array of value-pairs.


### Object Methods ###

```
$viewInstance->placeholderName->set($html)
```
Call the `set($HTML)` method on a placeholder to sets its contents.

```
$viewInstance->placeholderName->write($html)
```

Call the `write($HTML)` method on a placeholder to append to its contents.

```
$viewInstance->js->src($urlToJS)
$viewInstance->css->src($urlToCSS)
$viewInstance->css->src($urlToCSS, $mediaType)
```

Call the helper method `src()` on the `js` or `css` placeholders to create a script or style tag.


```
$viewInstance->getHTML()
```
Renders the view and returns the compiled HTML code.

```
$viewInstance->initializeSource($srcHTML)
```
Initialize a view with its HTML source.
