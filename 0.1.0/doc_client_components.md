
## Element ##

In EXC an **element** is an instance of a [JQUERY Object](https://api.jquery.com/Types/#jQuery) with additional properties and functionality.

Unlike your traditional JQUERY object which may point to multiple matched DOM elements, an **element** represents only one DOM element.

The **element** is the basic object used in EXC for most of its UI interactions and functionality. EXC uses composition to extend the **element** object. For example a **component** is just a specialized instance of an **element**.


**Properties of an element:**

| Name | Type | Description |
| --- | --- | --- |
| isElement | boolean | indicates that this JQUERY object has the element functionality. |
| uiw | string | Name of component represented by this element.
| tag | string | Name of the actual html element represented for example "div". |
| elementName | string | The value of the html attribute ```name``` if one is present. |
| node | DOM element | The instance of the DOM element represented by this element. Similar to ```o[0]``` in JQUERY, where ```o``` is a JQUERY object. |


### Setting Properties in an EXC Element ###
In an element you may store private data using the function ```property()```, for example:

```javascript
	var o = exc.$("field_emp_name");
	o.property("current_value", "Jose"); //sets a property

	if( o.hasProperty("current_value") ){ //check if a property is available
		var current_value = o.property("current_value"); //reads a property
	}

```

### Getting an EXC Element ###

You may have noticed the helper function ```exc.$( )``` used to get an element. There is also the global function ```jq()``` .

These functions may take a selector, a JQUERY object, a DOM element, the name of a DOM element or the ID of a DOM element.

### Other extensions to the JQUERY Object ###

EXC adds many extensions to the default JQUERY object that are available to any instance of JQUERY not only EXC elements. See [JQUERY extensions](#this) for more information.


## Component Definition ##

| Name | Type | Declaration | Description |
| -- | -- | -- | -- |
| setter | function | function(component, value) | **Optional** Called to set the value of the widget represented by this component. Must return the instance of THIS to enable chaining. |
| getter | function | function(component) | **Optional** Returns the value of the widget represented by this component. |
| vs | string or plain object | | **Optional** A string with the name of a build in value provider interface (eg: ```"value"```, ```"attr"```, ```"hidden"```) or a plain object with a ```getter``` function and a ```setter``` function.<br><br>The signature of a ```getter``` function is ```function(elm)``` and must return a value.<br><br>The signature of a ```setter``` function is ```function(elm, value)``` and must return the elm passed to enable chaining. <br><br>You may use the ```getter``` or ```setter``` functions instead. |
| propertyChanged | callback | function(name, oldValue, newValue) | **Optional**  Function called when the properties of a component have changed. This callback runs when an attribute is changed by a call to ```component.set(n,v)``` not when they are changed by ```component.property(n,v)```. |
| selectors | array of strings | | **Required** A list of selectors that identify or match this component.<br><br>For example:<br> ```selectors: [ "[data-uiw='textbox']" ],```|
| init | function | function(elm) | **Optional** Called to initialize the element. |
| isContainer | boolean | | **Optional** Indicates that this component is a container for other components or HTML content. Default value is false. |
| fn | plain object | | **Optional**  Additional functionality implemented by this component. The functions in this object will be added to the element. |
| inherits| array of strings | | **Optional** Name of other components from where we get functionality.<br><br>Example: ```inherits: ['container']``` |
| renderCompleted | callback | function(o) | **Optional**  An optional function called when an instance of this component was rendered. The function receives a jquery instance of the element.<br><br>This function should be used to perform post processing that requires you to have the DOM of the component created. You may add event listeners required to implements the component's behavior. |



## Creating custom functionality for components ##

The easiest way to add custom functionality to EXC's components is by extending built-in events.

For example lets say we want buttons to have a confirmation prompt by just adding the property ```actionConfirm```.

We use the function ```exc.components.events.define()``` to register a pre-defined event handler that is available to components.

```JavaScript
exc.components.events.define({
	event: "click.actionConfirm.validation()",
	applies: ["button"],
	apply: function(o){ //returns true if this event can be used with this component
		var msg = o.property("actionConfirm");
		if( (typeof(msg) != "string") || (msg.length == 0) ) return false;

		return true;
	},
	callback: function(e){ //static callback
		var o = exc.$($(e.target).closest("[data-uiw]"));
		if(o.hasClass('is-disabled')) return false;

		var ok = confirm(o.property("actionConfirm"));
		if (ok) return true;
	}
});
```

The definition indicates in which event we install our predefined handler. In our example we are listening to the ```click``` event. The event name ```"click"``` if followed by the namespace ```"actionConfirm"``` and the scope function ```"validation()"``` which tells EXC that our hook must be run as a validation prior to running any actual hooks listening to the ```click``` event.

The definition object has an array of component names in ```applies``` to tell EXC which component may implement this event.

We use the ```apply()``` function of our definition to check if a particular "button" component has the property ```"actionConfirm"``` if so we tell EXC to add this event handler by returning ```true```.

The function ```callback``` in our definition is an actual callback that will be called when the button is clicked.



## Custom event definition ##

| Name | Type | Declaration | Description |
| -- | -- | -- | -- |
| event | string | | **Required** Name of event handler. See ```$.onAction()``` in [JQUERY extensions](#this). |
| applies | string or array of strings | | **Optional** Tells EXC what components may use these custom event. It may containg the string ```"*"``` to indicate any component, the name of a component, or an array with a list of component names. |
| apply | function | function(comp) | **Optional** If present this function is called to determine if the event is applied. Return ```true``` to apply the event else return ```false```.
| createCallback | function | function(comp) | **Optional**  A factory function to create the callback that will handle the event. This function must return a callback function with the signature ```function(event)```.
| callback | callback | function(event) | **Optional** A static callback function that implements the actual event.<br><br>NOTE: If you implement ```createCallback``` in your definition, then ```callback``` will not be used. |

```JavaScript
exc.components.events.define({
	event: "click.actionConfirm",
	applies: ["menuitem", "button"],
	apply: function(o){ //returns true if this event can be used with this component
		var msg = o.property("actionConfirm");
		if( (typeof(msg) != "string") || (msg.length == 0) ) return false;

		return true;
	},
	createCallback1: function(o){ //callbackfactory

		var fn = function(e){
			var o = exc.$($(e.target).closest("[data-uiw]"));
			if(o.hasClass('is-disabled')) return false;

			var ok = confirm(o.property("actionConfirm"));
			if (ok) return true;
		}

		return fn;
	},
	callback: function(e){ //static callback
		console.log("is static...");
		console.log(e);

		var o = exc.$($(e.target).closest("[data-uiw]"));
		if(o.hasClass('is-disabled')) return false;

		var ok = confirm(o.property("actionConfirm"));
		if (ok) return true;
	}
});
```


# TO ORGANIZE #

```
exc.ui.components.definitions["textbox"] = {
	uiw: 'textbox',
	selectors: ["[data-uiw='textbox']"],
	vs: "value", //value source

	methods : { //additional functionality added to the element
		play: function(){
			console.log(this.uiw);
			console.log("at play...");
		}
	},
	//init: function(elm){ }

	build: function(elm){
		return obj;
	}


}
```

# Components #

## Widget exc-toggle ##

### Definition of exc-toggle ###
| Name | Type | Description |
| -- | -- | -- |
| name | string | **Required** Identifier of this widget. |
| disabled | boolean | **Optional** If true widget is disabled. |
| default | integer | **Optional** Default state. |
| help | string | **Optional** Help or Alt/Title attribute. |


### Events of exc-toggle ###

| Name  | Description |
| -- | -- |
| name_change | Value changed |
| name_on | Value changed to ON |
| name_off | Value changed to OFF |

### Eventful properties of exc-textbox ###

| Property | Description |
| -- | -- |
| onChangePublish | Publishes a message, when the widget is changed.<br><br>Example: ```"onChangePublish":"checkEmpType"``` |



## Widget exc-textbox ##

### Definition of exc-textbox ###
| Name | Type | Description |
| -- | -- | -- |
| name | string | **Required** Identifier of this widget. |
| disabled | boolean | **Optional** If true widget is disabled. |
| border | boolean | **Optional** If true widget has border. |
| size | integer | **Optional** Size attribute of textbox. |
| placeholder | string | **Optional** Placeholder attribute of the textbox. |
| help | string | **Optional** Help or Alt/Title  attribute of the textbox. |
| prefix | string | **Optional** Decorates a textbox with an icon or text at the beginning. |
| suffix | string | **Optional** Decorates a textbox with an icon or text at the end. |

### Definition Example ###

```HTML
<div data-component='{"type": "textbox", "name":"email", "default":"" , "border":true, "suffix":"@upr.edu", "placeholder": "Enter an email..." }'></div>
```

### Methods of exc-textbox ###

The component ```exc-textbox``` inherits methods from ```exc-generic```.

| Function | Syntax | Description |
| -- | -- | -- |
| enable | ```enabled()``` |  |
| disable | ```disable()``` | . |
| setValue | ```setValue(value)```| . |
| getValue | ```getValue()```| . |
| focus | ```focus()```| . |


### Eventful properties of exc-textbox ###

| Property | Description |
| -- | -- |
| onBlurPublish | Publishes a message, when the widget looses keyboard focus.<br><br>Example: ```"onBlurPublish":"checkEmpType"``` |
| onFocusPublish | Publishes a message, when the widget gains keyboard focus.<br><br>Example: ```"onFocusPublish":"checkEmpType"``` |
| onChangePublish | Publishes a message, when the widget is changed.<br><br>Example: ```"onChangePublish":"checkEmpType"``` |




## Eventful Properties ##

| Property | Description |
| -- | -- |
| onClickPublish | Publishes a message, when the widget is clicked.<br><br>Example: ```"onClickPublish":"saveRecord"``` |
| onClickEnable | Enable a component, when the widget is clicked.<br><br>Example: ```"onClickEnable":"mnuSaveRecord"``` |
| onClickDisable | Disables a component, when the widget is clicked.<br><br>Example: ```"onClickDisable":"mnuSaveRecord"``` |
| actionConfirm | Shows a prompt requiring user to confirm action. Click action are executed only if user selects "OK" from prompt.<br><br>Example: ```"actionConfirm":"Do you want to save this record?"``` |
| autoDisable | A component is disable when clicked. Actions for the click are executed before button is disabled. <br><br>Example: ```"autoDisable":true``` |
| onBlurPublish | Publishes a message, when the widget looses keyboard focus.<br><br>Example: ```"onBlurPublish":"checkEmpType"``` |
| onFocusPublish | Publishes a message, when the widget gains keyboard focus.<br><br>Example: ```"onFocusPublish":"checkEmpType"``` |
| onChangePublish | Publishes a message, when the widget is changed.<br><br>Example: ```"onChangePublish":"checkEmpType"``` |

# OLD stuf..... #


## Value Providers ##


## Value Modifiers ##

Use value modifiers to control the value returned by a widget.

| Name | Type | Description |
| -- | -- | -- |
| .value-use-suffix | class | The value returned by this widget will have its suffix appended. (See: attribute ```suffix```) |
| value-max | attribute | Max numeric value that can be returned. Ex: ```value-max="5000"``` |
| value-min | attribute | Min numeric value that can be returned. Ex: ```value-min="1000"``` |
| value-strip | attribute | Strips the string matched in the RegEx given. Ex: ```value-strip="(\@.*)$"``` |
| value-with-suffix | attribute | Appends the string given to the value returned. |
| value-without-suffix | attribute | Removes the string given from the end of the value returned. |
