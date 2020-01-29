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

### show_always  
This argument takes a boolean value. If true, then the menu will be displayed whenever the framework is loaded 