# API Helper functions
## Overview
Once your framework has been instantiated with descriptors there are a number of additional helpers and constructors made available through the API. Depending on the use-case only some of these may be needed. The most germane of these are listed below, with a short description of use case, arguments list, and example.  
### pinegrow.getCurrentProject()
This helper function returns an array of key:value pairs with information about the current project, including project folder URL. If the current page is not part of a project it returns 'null'. This helper is useful to test whether the current page is part of a project before trying to add resources.
### pinegrow.getSelectedPage()
This helper function returns the CrsaPage variable for the current page, which contains an array of key:value pairs with information such as breakpoints and page file URL.
### pinegrow.showQuickMessage( HTML, duration, single, error, don't_repeat_delay)
This helper allows the display of a message to the user. It is usually triggered by the ```on_inserted``` or ```on_changed``` keys. If the user has turned off quick notifications in the settings, the message will not be displayed unless ```error``` is set to true. If you need to display a message to the user whether or not quick message has been turned off, use ```pinegrow.showAlert()``` instead.
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
} else {
  ...
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

Next: [Menus](Menus.md)
  