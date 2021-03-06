## Custom Controls
---
### Overview
Custom controls allow for a more customized appearance for a plugin. They also allow for the construction of more complex controls. One example of this would be a set of side-by-side dropdown menus to set column numbers for each breakpoint. At a basic level, Pinegrow provides two methods to create controls with a custom look - the ```toggle_buttons``` key plus ```html``` key with or without the ```PgToggleButtonMaker()``` constructor. At a more advanced level the Pinegrow API a series of keys and helper functions for novel layouts.

**toggle_buttons**  
As briefly mentioned in the documentation of the ```select``` type, this key works along with the ```html``` key. This key takes a boolean value and if set to true it converts the dropdown list to side-by-side buttons. Reguires the use of the ```html``` key in the option list.

**html**  
This key takes an HTML string of function that returns an HTML string as value and should be provided as the third key in an individual option object within an option array. This HTML string can contain inline styling and classes. 
```javascript
options: [
	{
		key: 'push',
		name: 'Push',
		html: '<div class="pg-tb-button pg-tb-button-text" style="font-weight: 700; color: white;" title="Push">Push</div>'
	},
	{
		key: 'slide',
		name: 'Slide',
		html: '<div class="pg-tb-button pg-tb-button-text" style="font-weight: 700; color: white;" title="Slide">Slide</div>'
	}
	}
]
```
For simple buttons the ```pg-tb-button``` and ```pg-tb-button-text``` classes can be added to maintain an overall appearance that mimics the standard interface.


### __PgToggleButtonMaker()__ 
---
This constructor exposes a number of different helper functions that aid in making different types of interface buttons. It doesn't receive any arguments and each of the helper functions are passed to the ```html``` key.  
Basic usage
```javascript
var button_maker = new PgToggleButtonMaker();
```

### helper functions
---
#### makeText( text, options)
This helper function receives two arguments.  
  * **text** This argument accepts a string that will be displayed on the button.

  * **options**  This argument receives an object containing key:value pairs, where each value is an object composed of key:value pairs. The two primary keys are ```styles``` and ```attributes```.  

    * ```styles```  
This key receives an object containing key:value pairs where each key is a valid CSS property, e.g. ```color``` or ```padding```, and the value is what you want the property value set to, e.g. ```red```. These styles will be added inline to the resulting button.  

    * ```attributes```  
This key receives an object containing key:value pairs where each key is an attribute name, e.g. ```data-toggle``` or ```placement```, and the value is the attribute set value, e.g. ```tooltip``` or ```left```.  
Example usage
```javascript
'options' : [
    {
        'key' : 'normal',
        'name' : 'Normal',
        'html' : button_maker.makeText('Abc', {
            styles: {'font-family': 'serif', 'font-size': '15px'},
            attributes: {'title': 'Normal'}
        })
    },
    {
        'key' : 'small-caps',
        'name' : 'Small caps',
        'html' : button_maker.makeText('Abc', {
            styles: {'font-family': 'serif', 'font-size': '15px', 'font-variant': 'small-caps'},
            attributes: {'title': 'Small caps'}
        })
    }
]
```
Note: in this example a ```title``` attribute is being set. This will appear when the user hovers over the button.  

Output of example:  
![makeText button](Images/makeText&#32;Button.png)
#### makeColor(color, text, options)
This helper is of limited use due to the caveats noted below. It is usually better to use the makeText helper with additional styling for any button other than a solid color selector. This helper function returns a small 16px x 18px button with a colored background. It accepts three arguments.  

  * **color**  This argument receives the desired background color of the button.  

  * **text**  This argument receives an HTML string to be displayed on the button - with the caveat that the string must fit on a small button, usually a single character.  

  * **options**  Like ```makeText()```, this argument can receive a ```styles``` object and an ```attribute``` object.  

The one caveat to using this helper is that any objects supplied through the ```options``` argument are merged with the existing key:value pairs set by default in the ```makeColor()``` function. This means that the default styling and passed in color argument will be overridden by any ```style``` object passed in. No default attributes are set in the function, so there is no conflict in passing an ```attribute``` object.  
Typical usage:
```javascript
{
	'key': 'primary',
	'name': 'Primary',
	'html' : button_maker.makeColor('#FF0000')
}
```
Example output from the Pinegrow Bootstrap Framework  
![makeColor button](Images/makeColor&#32;button.png)  


#### makeColorText(color, options, text)  
This helper function is of limited use due to the caveats noted below. It is usually better to use the makeText helper with additional styling for any button other than a solid color selector. It returns a small 16px x 18px button that is populated with an icon representing the letter "A" in the specified color. Formally, the function receives three arguments.  

  * **color** This argument receives the desired color of the icon displayed on the button.  

  * **options** This argument receives ```styles``` and ```options``` objects like the other helpers described above.  

  * **text** This argument can be passed to the function but is not used in the function.  

Overall, this function has the same caveats as the ```makeColor()``` function.  
Example output from Pinegrow Bootstrap Framework  
![makeColorText button](Images/makeColorText&#32;button.png)  


#### makeIcon(icon, otions)  
This helper function creates a small button displaying the passed icon. It receives two arguments. 

  * **icon** This argument is a string representing the name of the glyph to be displayed on the button.  

  * **options** This argument takes ```settings``` and ```attributes``` objects like the ```makeText()``` function.  

Unlike the ```makeText()``` function, the ```makeIcon()``` function has no predefined options, so any that are passed will not impact the default styling. A table of the glyph names can be accessed [here](https://bodonkey.github.io/Pinegrow-Fonts/). Please note that not all names are descriptive, some are simply numeric references. There is also a downloadable PDF file listing all of the glyphs and names that can be accessed <a href="https://raw.githubusercontent.com/BoDonkey/Pinegrow-Fonts/master/PG%20Fonts%20Cheatsheet.pdf">here</a>.  
Example usage:
```javascript
options: [{
		key: 'left',
		name: 'Left',
		html: uikitButton.makeIcon('align-left', {
			attributes: {
				title: 'Left'
			}
		})
	},
	{
		key: 'center',
		name: 'Center',
		html: uikitButton.makeIcon('align-center', {
			attributes: {
				title: 'Center'
			}
		})
	},
	{
		key: 'right',
		name: 'Right',
		html: uikitButton.makeIcon('align-right', {
			attributes: {
				title: 'Right'
			}
		})
	},
	{
		key: 'none',
		name: 'None',
		html: '<div class="pg-tb-button pg-tb-button-text" style="font-weight: 700; color: white;" title="None">X</div>'
	}
],
```
![Icon Button Example](Images/Icon&#32;Button&#32;Example.png "Icon Button example output")

## Advanced Custom Controls
---  
### Overview  
Not only does the Pinegrow API provide standard controls for constructing your plugin UI, it also has several helper functions and constructors to allow the composition of custom controls with unique look. For example, the "Margin & Padding" control within the Pinegrow visual CSS editor is a custom control.  
![Custom Control Example](Images/Custom&#32;control.png "Example of a custom control")

These custom controls are composed as functions that are called from the ```control``` key of one or more fields. The called function instantiates a new control using the ```PgCustomPropertyControl``` constructor. This constructor takes two keys, ```registerInputField``` and ```showInputField```, that register and return return HTML for each of the control elements. 

Example Usage
```javascript
var pge_breakpoints = [ 'small', 'medium', 'large', 'xlarge' ];
var pge_breakpoint_widths = ['1-1', '1-2', '1-3', '2-3', '1-4', '3-4', '1-5', '2-5', '3-5', '4-5', '1-6', '5-6'];

//additional component code
fields: {
	pge_add_breakpoint_widths: {
		type: 'custom',
		name: 'Element Breakpoint Width',
		action: 'none',
		control: widthControl([{
			values: pge_breakpoint_widths,
		}]),
	},
//additional component code

var widthControl = function (settings) {
	
	//create the new control with the PgCustomPropertyControl constructor
	var width_control = new PgCustomPropertyControl('pge_width_control');
	
	width_control.onDefine = function () {
		//loop through each of the breakpoint_sizes
		for (var breakpoints_sizes = 0; breakpoints_sizes < pge_breakpoints.length; breakpoints_sizes++) {
			//generate a field name from each array value
			var field_name = 'pge_sizes_' + pge_breakpoints[breakpoints_sizes];
			//register each field with name and a set of controls
			this.registerInputField(field_name, createFieldDef(settings, pge_breakpoints[breakpoints_sizes]));
		};
	};

	width_control.onShow = function () {
		//create a table for our control
		var $table = $('<table/>'});
		//add a blank cell for row label
		var html = '<td></td>';
		//add name labels for each column
		pge_breakpoints.forEach(function(size) {
			html += '<td><label>' + size + '</label></td>';
		});
		//add row with labels to the table
		var $row = $('<tr/>').html(html).appendTo($table);
		//add next row to table
		$row = $('<tr/>').appendTo($table);
		//add control lable
		var $td = $('<td/>').html('<lable>Breakpoint Widths</label>').appendTo($row);
		//loop through each of the break_point sizes
		for (var breakpoints_sizes = 0; breakpoints_sizes < pge_breakpoints.length; breakpoints_sizes++) {
			//create a data cell to hold each control field
			$td = $('<td/>').appendTo($row);
			//generate a field name from each array value - this should be identical to the registered name
			var field_name = 'pge_sizes_' + pge_breakpoints[breakpoints_sizes];
			//add each registered field to the created $td of the table
			this.showInputField($td, field_name, createFieldDef(settings, pge_breakpoints[breakpoints_sizes]));
		};
		//return the newly created control to the calling field
		return $table
	};
};

var createFieldDef = function (settings, width_name) {
	var span_select = {
		type: 'select',
		name: null,
		action: 'apply_class',
		show_empty: true,
		options: []
	};
	//Generate specific Key and Name values from array passed in the settings argument
	settings.values.forEach(function (value) {
		span_select.options.push({
						key: value + '@' + width_name,
						name: value
		});
	});
	//return the field control to the define method
	return span_select;
};
```

#### Component Field Structure  
A field utilizing a custom control should assign the ```type``` key a value of ```custom``` and the ```action``` key a value of ```none```. As mentioned above, the ```control``` key should receive a function that returns the new control. In order to keep components re-usable, it is often useful to pass an object containing multiple key:values to the custom control. For simplicity, in this example we are passing an object with a single key, ```values```. That key receives an array ```pge_breakpoint_widths```.

```javascript
/additional component code
fields: {
	pge_add_breakpoint_widths: {
		type: 'custom',
		name: 'Element Breakpoint Width',
		action: 'none',
		control: widthControl([{
			values: pge_breakpoint_widths,
		}]),
	},
//additional component code
```

#### PgCustomPropertyControl(control_id)    
This constructor takes a single argument during instantiation, ```control_id```. This ```control_id``` should be unique and best practice is to prefix with a plugin unique value to prevent conflict. It takes two key:value pairs, ```onDefine``` and ```onShow```.   
Example usage:
```javascript
var widthControl = function (settings) {
	
	//create the new control with the PgCustomPropertyControl constructor
	var width_control = new PgCustomPropertyControl('pge_width_control');
	//remainder of widthControl function including .onDefine and .onShow keys
```  
  * #### onDefine  
    This key acts as a hook that fires when the framework is first loaded. It receives a function that reserves the unique field names of each of the custom control elements using the constructor supplied ```registerInputField```function. It typically is a loop that iterates over an array of values.
Example usage:
```javascript
var pge_breakpoints_array = [ 'small', 'medium', 'large', 'xlarge' ];

//additional function code
width_control.onDefine = function () {
		//loop through each of the breakpoint_sizes
		for (var breakpoints_sizes = 0; breakpoints_sizes < pge_breakpoints.length; breakpoints_sizes++) {
			//generate a field name from each array value
			var field_name = 'pge_sizes_' + pge_breakpoints[breakpoints_sizes];
			//register each field with name and a set of controls
			this.registerInputField(field_name, createFieldDef(settings, pge_breakpoints[breakpoints_sizes]));
		};
	};
//additional function code
```
In this example, we are creating a control that will allow the user to select the element width from a set of responsive widths (```pge_responsive_widths```)for four hypothetical breakpoints (```pge_breakpoints_array```). The anonymous function passed to ```onDefine``` will loop over an array named ```pge_breakpoints_array```. For each of the items it will prefix with ```pge_sizes_``` and then pass that unique field name to the ```registerInputField``` function. In this example that will result in the reservation of 4 control slots, one for each item in the ```pge_breakpoints_array```.

  * #### registerInputField(field_name, field_definition) 
      This function takes two arguments:  
    * **field_name**  This argument receives a unique field name that is typically proceeded with a plugin-specific prefix.  

    * **field_definition**  This argument receives a function that returns an object containing the key:value pairs normally found in a component field. Conventionally, this object is returned by a function named ```createFieldDef```, but can be any function that returns a unique field for each component in the custom control. 

  * #### on_show
     This key acts as a hook that fires when the component is selected in the tree and the custom control is shown in the properties tab. As a value it receives a function that returns HTML with each reserved control to display to the user. Each of the unique control ids reserved using the ```registerInputField``` function are passed to the ```showInputField```.
```javascript
width_control.onShow = function () {
		//create a table for our control
		var $table = $('<table/>'});
		//add a blank cell for row label
		var html = '<td></td>';
		//add name labels for each column
		pge_breakpoints.forEach(function(size) {
			html += '<td><label>' + size + '</label></td>';
		});
		//add row with labels to the table
		var $row = $('<tr/>').html(html).appendTo($table);
		//add next row to table
		$row = $('<tr/>').appendTo($table);
		//add control lable
		var $td = $('<td/>').html('<lable>Breakpoint Widths</label>').appendTo($row);
		//loop through each of the break_point sizes
		for (var breakpoints_sizes = 0; breakpoints_sizes < pge_breakpoints.length; breakpoints_sizes++) {
			//create a data cell to hold each control field
			$td = $('<td/>').appendTo($row);
			//generate a field name from each array value - this should be identical to the registered name
			var field_name = 'pge_sizes_' + pge_breakpoints[breakpoints_sizes];
			//add each registered field to the created $td of the table
			this.showInputField($td, field_name, createFieldDef(settings, pge_breakpoints[breakpoints_sizes]));
		};
		//return the newly created control to the calling field
		return $table
	};
```

  * #### showInputField( $el, field_name, field_definition)  
    This function takes three arguments:  

    * **\$el**  This argument passes a jQuery-wrapped DOM element indicating the location where the control should be inserted.  

    * **field_name**  This argument takes a unique field name, identical to the value passed to ```registerInputField```.  

    * **field_definition**  This takes a function that returns the control field key:value pairs.  

  * #### createFieldDef()  
    The function passed to ```showInputField```  conventionally receives this name, but any callback function name can be used. It returns a field array to the ```onDefine``` and ```onShow``` functions, including ```type``` and ```action``` key:value pairs.  
Example usage:  
```javascript
var createFieldDef = function (settings, width_name) {
	var span_select = {
		type: 'select',
		name: null,
		action: 'apply_class',
		show_empty: true,
		options: []
	};
	//Generate specific Key and Name values from array passed in the settings argument
	settings.values.forEach(function (value) {
		span_select.options.push({
						key: value + '@' + width_name,
						name: value
		});
	});
	//return the field control to the define method
	return span_select;
};
```
