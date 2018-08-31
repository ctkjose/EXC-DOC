## EXC Documentation Version 0.1.0 ##
[Documentation Index](./doc_index.md)<br>

## Formula Helper ##

A formula can be implemented with the helper class `\exc\helper\formula`.

```PHP
<?php
$formula = new \exc\helper\formula();

$values = [
	'location_id' => "RUM",
	'fname' => "Jose",
	"lname"=>"Cuevas",
	"sn"=>"8021234567"
];

$a_url = "/LOWER(%location_id%)/records/index.php?sn=%SN%";

$formula->expressionSetValue($values);
$url = $formula->expressionInterpolate($a_url);
?>
```

Also any class can use the trait `expressionBuilder` to evaluate expressions.

```php
<?php
class MyClass {
	use \exc\helper\expressionBuilder;


}

$formula = new MyClass();
?>
```

An expression can substitute values in a string with the function `expressionInterpolate($expr=null)` or it can ve evaluated to a value with `expressionEvaluate($expr=null)`.
Both of these function take as an argument the expression that you want to evaluate. In most cases when you will re-use the same expression is best to set the expression using the following:

```php
<?php

$formula->expressionSetFormula($expr); //this is faster, because we only compile the formula once...

//do some loop
foreach($locations as $definition){
	$formula->expressionSetValue($definition);
	$url = $formula->expressionInterpolate();
}
```


### Values ###

Values are available in an expression as `%NAME_OF_VARABLE%`. You add values to an expression with one of the followings:

```
$obj->expressionSetValue($anArrayOfValues);
```
```
$obj->expressionSetValue("name1", $value1);
```
```
$obj->expressionSetValue("name1", $value1, "name2", $value2, ...);
```

You can also reset all the values and assign new ones with `expressionInitializeValues()`:
```
$obj->expressionInitializeValues($anArrayOfValues);
```
*Calling **expressionInitializeValues()** will also reset values stored by reference!*

In many instances is faster not to copy values into the expression, thats specially true when working with large data or intensive interactions. In that case you can make values available to the expression by reference:

```
$course = [
	'course_title' => "Mate",
	'course_code' => "MATE3031",
];

$formula->expressionUseValues($course, 'COURSE');

```

When using references you always have to use a context prefix, in this example is "COURSE". The `course_code` value will be available in the variable `%COURSE_COURSE_CODE%`.

You may also use objects as reference:

```php
<?php
$course = new stdClass();

$course->course_title = "Mate";
$course->course_code = "MATE3032";

$formula->expressionUseValues($course, 'COURSE');

```

A value may be a simple array/hash. An array value is available in an expression with the variable `%NAME[INDEX]%`. For example:
```php
<?php
$values = [
	'location_id' => "RUM",
	'fname' => "Jose",
	"lname"=>"Cuevas",
	"sn"=>"8021234567",
	"degree" => [
		"name"=> "BS in Computer",
		"code"=> "1209"
	]
];


$expr = "%DEGREE[CODE]%";

$formula = new MyClass();
$formula->expressionSetValue($values);
$value = $formula->expressionInterpolate($expr);

```
**IMPORTANT:** Only one dimension can be used.

Similar to arrays a value can be an object and you access the object properties the same way as you do with an array:
```
$std_record->term = rea_sis_terms::loadActiveTerm(); //load an object into term...
$formula->expressionUseValues($std_record, 'RECORD'); //assing as a reference...

$expr = "%RECORD_TERM[TERM_CODE]%";  //represents $std_record->term->term_code
```

When re-using an expression you may need to reset the values, specially if not all records may have the same fields. To reset the values you can use one of the following:

```
$formula->expressionCleanValues();  //reset copied values...
```

```
$formula->expressionCleanValues(true);  //reset copied values and references...
```

```
$formula->expressionCleanReferences(); //reset only references
```




### Functions ###

A formula may use functions similar to those found in spreadsheets. Here is the list of supported functions:

 | SYNTAX | DESCRIPTION |
 | ------ | ----------- |
 | MID( %VALUE% , START, LEN ) | Returns a portion of the string specified by the START and LEN parameters. The first character is at position 0. |
 | MID( %VALUE% , START ) | Returns the substring starting from START until the end of the string. |
 | REPLACE( %VALUE%, FIND, REPLACE ) | Returns a string with all instances of FIND replaced with REPLACE. |
 | LOWER( %VALUE% ) | Converts text to lowercase |
 | UPPER( %VALUE% ) | Converts text to uppercase |
 | TRIM( %VALUE% ) | Removes spaces from the begining and end of the text |
 | LTRIM( %VALUE% ) | Removes spaces from the begining of the text |
 | RTRIM( %VALUE% ) | Removes spaces from the end of the text |
 | DATEVALUE( %VALUE% ) | Converts a date in the form of text to a UNIX timestamp. |
 | DATE( TIMESTAMP , FORMAT ) | Returns a date formated. |
 | DAY( %VALUE% ) | Returns the day of the month. |
 | MONTH( %VALUE% ) | Returns the month. |
 | YEAR( %VALUE% ) | Returns the year. |

### Specials ###

 | SYNTAX | DESCRIPTION |
 | ------ | ----------- |
 | #DAY# | Current day of the month |
 | #MONTH# | Current month |
 | #YEAR# | Current year |
 | #NOW# | Current unix time stamp |
 | #TODAY# | Current day at midnight as a time stamp |
 | #DATETIME# | Current date time string |
 | #2017-12-25# | A literal date as a time stamp. |
 | #2017-12-25 23:59:59# | A literal date-time as a time stamp. |
 | #Y-m-d H:i:s# | Current date time formatted, See PHP Date Function for options. |
