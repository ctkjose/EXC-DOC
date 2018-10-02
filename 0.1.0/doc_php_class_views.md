## Documentation Version 0.1.0 ##
[Topics](./doc_index.md) | [Views](./doc_server_views.md) | [View Class](./doc_php_class_views.md)<br>

## Class exc\views\manager ##

### View Manager Methods ###
```php
$viewInstance = \exc\views\manager::createDefaultView($viewName);
```
Initialize the application default view.

```php
$viewInstance = \exc\views\manager::getDefaultView();
```
Get the instance of your default view.

```php
$viewInstance = \exc\views\manager::getView($viewName);
```
Get an instance of a view.

## Class exc\views\view ##

### Class Methods ###
```php
public static function default($viewInstance)
public static function default($viewName)
```
Sets and returns an instance of the default view. You may omit the $viewName if you have configured the name of your default view in your config option "views.default". for your "app.php" file.

```php
public static function default()
```
Returns an instance of the default view.

```php
public static function createWithFile($path, $name='default')
```
Factory method to create an instance of `view` from a file.

```php
public static function createWithSource($source, $name='default')
```
Factory method to create an instance of `view` from a $source string containing HTML.


```php
public static function constantSetValue($key, $value)
public static function constantSetValue($key1, $value1, $key2, $value2...)
public static function constantSetValue($anArrayOfValuePairs)
```
Sets the value of a **@CONSTANT** placeholder.

```php
public static function constantCopyValues($anArrayOfValuePairs, $prefix='')
```
Creates a **@CONSTANT** placeholder for each key in an array of value-pairs.


### Object Methods ###

```php
$viewInstance->placeholderName->set($html)
```
Call the `set($HTML)` method on a placeholder to sets its contents.

```php
$viewInstance->placeholderName->write($html)
```

Call the `write($HTML)` method on a placeholder to append to its contents.

```php
$viewInstance->js->src($urlToJS)
$viewInstance->css->src($urlToCSS)
$viewInstance->css->src($urlToCSS, $mediaType)
```

Call the helper method `src()` on the `js` or `css` placeholders to create a script or style tag.


```php
$viewInstance->getHTML()
```
Renders the view and returns the compiled HTML code.

```php
$viewInstance->initializeSource($srcHTML)
```
Initialize a view with its HTML source.
