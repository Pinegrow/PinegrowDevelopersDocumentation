## Custom Actions
___
### Overview  
Custom actions do more to manipulate the DOM than adding a simple class or attribute. For example they can set multiple attributes and values at the same time, or add a class to a parent element in the DOM at the same time as adding one to the selected elements. They are comprised of two components, the ```get_value``` and ```set_value``` keys, with the ```action``` key set to ```custom```.

### get_value
This key accepts a function as a value. This function receives a single argument, ```pgel```, which is the pgParserNode (the source-code representation of the current DOM node) for the selected element. This function should return the current value that the control should display. This value is based on the current state of the DOM. For example, if the custom control adds an attribute to the parent element, the ```get_value``` key function should fetch and examine the parent element and return "true" if the attribute is present. Depending on the type of control, the value that is returned will need to be different.
|type|returned value|
|---|---|
checkbox | true/1 or null/false/0
select | value of ```name``` key
text | user entered text
image | file name
slider | current slider value
### set_value
This key accepts a function as value. This function receives two arguments, ```pgel```, which is the pgParserNode (the source-code representation of the current DOM node) for the selected element, and ```value```, which is the user selected value from the field. It is the responsability of this function to manipulate the DOM in response to the user selection.  
Simple example of a custom field
```javascript
//other component code
fields: {
	pge_add_tooltip: {
		type: 'checkbox',
		name: 'Add tooltip?',
		action: 'custom',
		value: '1',
		get_value: function (pgel) {
			return pgel.getAttribute('data-toggle') === 'tooltip' ? '1' : null;
		},
		set_value: function (pgel, value) {
			if (value) {
				pgel.setAttribute('data-toggle','tooltip');
				pgel.setAttribute('data-placement', 'top');
			} else {
				pgel.removeAttribute('data-toggle');
				pgel.removeAttribute('data-placement');
			}
			return value;
		}
	}
}

```  
In an actual plugin an additional plugin to select the value of the ```data-placement``` attribute would be conditionally displayed when a tooltip was added.
Note: along with the ```custom``` action, a ```value``` key with a value that is non-zero and defined is required. In this example the ```value``` key is passed a string, ```'1'``` but passing a non-zero integer, or any string is sufficent.  
