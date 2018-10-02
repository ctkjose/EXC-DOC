## Documentation Version 0.1.0 ##
[Topics](./doc_index.md) | [Views](./doc_server_views.md) | [View Class](./doc_php_class_views.md)<br>

## Views ##

A view holds your actual html or the fragments of html used to build your user interface. In EXC we can manipulate views from the server side prior to serving the content to the browser or dynamically in the client side.

A view has placeholders that you can manipulate from code, even add content from other views among other features.

You create a **view** for your app default layout/template. You can have different elements each in a separate view, for example a view with a form to edit or add a record, or a view for a particular header or menu.

The backend can push dynamic content to the app running in the browser using views, for example a dialog to edit some information.

## Views in your PHP backend ##

### Creating a view ###

A **view** is a php file named `view.myname.php`, where `myname` is the actual name of your view.

> **NOTE:** The view can be an HTML file like  `view.myname.html`.

You place your view files in a folder named `views` in your app folder. This is where EXC would look for them by default.

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

### The default view ###

An EXC app has a main view called the **default view**. You load a view as your default using the method `createDefaultView(name)`:
```PHP
$view = \exc\ui\views\manager::createDefaultView("myname");
```
Your default view will be send to the browser.

Once a default view is set you can obtain the default view with:
```PHP
$view = \exc\ui\views\manager::getDefaultView();
```

Besides your default view you can have as many views as you need. You can have a view with a form, with a header and any other fragment of html that you would like to reuse.

You get another view by its name using the function `getView($name)`:
```PHP
$view = \exc\ui\views\manager::getView("edit_form");
```

### Using your view in PHP ###

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
Use `write()` to add the contents of another view:
```PHP
$viewHeader = \exc\ui\views\manager::getView("record_header");
$viewHeader->title->write("Welcome Jose");

$view->contents->write($viewHeader);
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


Directives are another type of placeholders.

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
