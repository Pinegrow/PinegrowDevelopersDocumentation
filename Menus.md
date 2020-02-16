## Menu Helpers 
___ 
There are two major ways to add new menu items through the Pinegrow API. The first allows for addition of new menu items to the "Page" main menu using the framework key ```on_page_menu``` and passing in the menu items. The second allows for the addition of an entirely new menu using the ```addPluginControlToTopbar()``` helper function.

### .on_page_menu(page, items)  
This framework key is used for inserting additional items into the "Page" menu. The additional items will be added after the "Manage Google Fonts..." menu item and will only be visible when a page is opened in the editor. This key receives a function that is passed two arguments, ```page``` and ```items```.  
The ```items``` argument is an array of objects containing the items to be added to the menu. There are three different objects that ```items``` accepts.  

 1) An object with a single key:value pair  
 ```type: 'divider'```   
 This object adds a horizontal divider in the dropdown menu.
 
 2) An object that adds a header with two key value pairs:  
 ```type: 'header',```  
 ```label: 'Any string'```  
 This object will cause Pinegrow to add a non-clickable header to  the dropdown menu using the string passed as the ```label``` value.
 3) An object with up two key:value pairs:  
 ```label: 'Any string',```  
 ```action: function(){/*action to be performed when item is selected}```  
 This object will display the ```label``` key value in the menu and execute the function passed in the ```action``` key value when the user clicks the menu item. 
 Simple menu example
 ```javascript
 framework.on_page_menu = function (page, items) {
	 items.push({
		 type: 'divider'
	 });

	 items.push({
		 type: 'header',
		 label: 'New Menu'
	 });

	 items.push({
		 label: 'Menu Item One',
		 action: function() {
			 customFunction()
		 }
	 })
 }
 ```

## addPluginControlToTopbar( framework, $control, show_always, func)
This helper will create a new menu and insert it prior to the "Support" menu in the main header. It accepts four arguments, with the first two being required and the second two optional.  

**framework**  
The first argument is simply the framework object that we instantiated in our plugin. It defaults to "f" and is required.  

**$control**    
The second argument is the actual menu HTML and is required. It should be passed as a jQuery element. Each of the dropdown menus are individual ```<li>``` items. The menu is styled and enabled using Bootstrap 4 classes and toggles.  
Simple menu example  
```javascript
var $control = $('
    <li id="example-menu" class="dropdown">
        <a href="#" class="dropdown-toggle" data-toggle="dropdown"><span>New Menu Title</span></a>
        <ul class="dropdown-menu">
            <li><a href="#" id="item-one">Menu Item One</a></li>
            <li><a href="#" id="item-two">Menu Item Two</a></li>
        </ul>
    </li>
');
```
The next two keys work in tandem. One or both arguments must evaluate to true in order to display the new menu.  

**show_always**    
This argument takes a boolean value. If true, then the menu will be displayed whenever the framework is loaded, irrespective of page or project. Note, this is accomplished through appending the ```$control``` to the editor through jQuery manipulation. If this function is called to early in the load there will be an error due to the targeted on page element not existing. 

**func**  
This argument takes a function that evaluates whether the menu should be loaded and should return a boolean value. For example, it can be used to determine if there is a page currently open, or if it is part of a project.