# API Helper functions
## Overview
Once your framework has been instantiated with descriptors there are a number of additional helpers and constructors made available through the API. Depending on the use-case only some of these may be needed. The most germane of these are listed below, with a short description of use case, arguments list, and example.  
### pinegrow.getCurrentProject()
This helper function returns an array of key:value pairs with information about the current project, including project folder URL. If the current page is not part of a project it returns 'null'. This helper is useful to test whether the current page is part of a project before trying to add resources.
### pinegrow.getSelectedPage()
This helper function returns the CrsaPage variable for the current page, which contains an array of key:value pairs with information such as breakpoints and page file URL.
### pinegrow.showQuickMessage( HTML, duration, single, error, don't_repeat_delay)
This helper allows the display of a message to the user. It is usually triggered by the```on_inserted``` or ```on_changed``` keys. If the user has turned off quick notifications in the settings, the message will not be displayed unless ```error``` is set to true. If you need to display a message to the user whether or not quick message has been turned off, use ```pinegrow.showAlert()``` instead.
  * HTML
  This argument receives an HTML or plain text string to display to the user
  * duration
  This argument receives the number of ms to display the message. Note: this will block any interaction with the Pinegrow App for the duration of the message. Defaults to 2500ms.
  * single
  This argument receives a boolean value and is optional. Generally only passed in if also passing a value to context. Has no impact on message display.
  * error
  This argument receives a boolean value and is optional. If 'true' then the message modal will receive an orange border. To directly display an error message it is preferential to use ```pinegrow.showQuickError()```.
  * don't_repeat_delay
  This argument receives the number of ms to delay before allowing message to display again. For example, if you alert the user to refresh the page on element insertion and they are placing that same element on the page three times in succession, you would only want to display the message once, so the delay could be set to 60000ms (1 minute).
### pinegrow.showQuickError( HTML, single)  
This helper allows for the display of an error message to the user, which is a quick message box with orange background or border. It can be triggered from within a component or any time some check fails. For example, a particular element might only be used in the context of a project and give an error if added to a single page.
```javascript
var isProject = pinegrow.getCurrentProject();
if (!isProject) {
	pinegrow.showQuickError('Save as a project first!');
} else {}
}
```
  * HTML
  This argument receives an HTML or plain text string to display to the user.
  * single
  This argument receives a boolean value and is optional. Generally only passed in if also passing a value to context. Has no impact on message display.  

### pinegrow.showAlert(HTML, title, button_one_msg, button_two_msg, button_one_func, button_two_func)
This helper function receives up to 6 arguments (all technically optional) and opens a modal that collects user input. It can display up to two buttons, each with a different function.
  * HTML
  This argument receives an HTML or plain text string to display as the body text of the modal. Optional and defaults to an empty string if null is passed.
  * title
  This argument receives an HTML or plain text string to display as the title for the modal. Optional and defaults to 'Notice' if null is passed.
  * button_one_msg
  This argument receives an HTML or plain text string to display as the first button text. Optional and defaults to 'OK' if null is passed.
  * button_two_msg
  This argument receives an HTML or plain text string to display as the second button text. Optional with no button displayed if passed null.
  * button_one_func
  This argument receives a function that is fired if the user clicks button number one. Optional and defaults to null if not passed. If the button is used to cancel any changes and close the modal and a second button with function is being passed, this argument should be passed 'null'.
  * button_two_func
  This argument receives a function that is fired if the user clicks button two. Optional and defaults to null if not passed.
  ```javascript
  var $html = $('<h3>Do you wish to add the slider element script to your page?</h3>\
  <p>This is required for the slider element to function.</p>');
  pinegrow.showAlert ($html, 'Add slider JavaScript?', 'Cancel', 'Yes, please!', null, function(){
	  addScript();
	});
  ```
  ![Example showAlert Output](Images/showAlert.png)

### pinegrow.getPlaceholderImage()
This helper will return a random image from the pinegrow server to serve as a placeholder in any image element. Note, this will only work if the user is connected to the internet.

<a name="mc"></a>
## Menu Helpers 
___ 
There are two major ways to add new menu items through the Pinegrow API. The first allows for addition of new menu items to the "Page" main menu using the framework key ```on_page_menu``` and passing in the menu items. The second allows for the addition of an entirely new menu using the ```addPluginControlToTopbar()``` helper function.

### .on_page_menu(page, items)  
As described above, this key is used for inserting additional items into the "Page" menu. The additional items will be added after the "Manage Google Fonts..." menu item and will only be visible when a page is opened in the editor. This key receives a function that is passed two arguments, ```page``` and ```items```.  
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

### framework
The first argument is simply the framework object that we instantiated in our plugin. It defaults to "f" and is required.  

### $control  
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
### show_always  
This argument takes a boolean value. If true, then the menu will be displayed whenever the framework is loaded, irrespective of page or project. Note, this is accomplished through appending the ```$control``` to the editor through jQuery manipulation. If this function is called to early in the load there will be an error due to the targeted on page element not existing. 

### func
This argument takes a function that evaluates whether the menu should be loaded and should return a boolean value. For example, it can be used to determine if there is a page currently open, or if it is part of a project.

## Framework Hooks

### on_project_loaded()
This key takes a function as value and calls the supplied function when a project with the plugin activated is loaded.
### on_page_loaded
This key takes a function as value and calls the supplied function when a page with the plugin activated is loaded.
### on_page_saved
This key takes a function as value and calls the supplied function when a page with the plugin activated is saved.
### on_page_changed
This key takes a function as value and calls the supplied function when a page with the plugin activated is changed.
### on_page_closed
This key takes a function as value and calls the supplied function when a page with the plugin activated is closed.
### ?on_build_actions_menu
### ?on_element_inserted
?
  