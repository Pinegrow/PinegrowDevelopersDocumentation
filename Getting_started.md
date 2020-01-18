# Getting Started
All Pinegrow plugins are instantiated by calling a function, either anonymous or named, once the Pinegrow App signals it is ready. This function gets passed the event and the 'pinegrow' window variable as arguments.
```javascript
$(function() {
    $('body').one('pinegrow-ready', function(e, pinegrow) {
        //plugin body
    });
});
```
or
```javascript
$(function() {
    $('body').one('pinegrow-ready', function(e, pinegrow) {
        myFunctionName(pinegrow);
    });
});
function myFunctionName(pinegrow){
    //plugin body
};
```
Within the body of the plugin, a new framework object is created using the ```PgFramework``` constructor and passing in a unique type that will be prefixed internally, along with a name for the framework.
```javascript
var framework = new PgFramework(type_prefix, name);
``` 
This framework variable can then be populated with a variety of key:value pairs. Some of these pairs are informational, like ```framework.author```, which will be displayed to the end user, or give parameters to the Pinegrow App about how the plugin is supposed to be managed or displayed. The most important of these are listed below in the section [**framework descriptive elements**](#fde). Other pairs add the individual library components, items to the property panel, or actions panel. The most important of these are listed below in the section [**framework helpers and constructors**](#fhc). A third category of key:value pairs control the actions of the CMS and are listed below in the section [**framework CMS helpers**](#fch).  
Typically, the descriptive key:value pairs for the framework are defined prior to returning the new object to the Pinegrow App using the 'addFramework()' function.
```javascript
pinegrow.addFramework(framework);
```

<a name="fde"></a>
## Framework Descriptive Elements  
### type
This key takes a unique value identifying the framework - usually the same as the type_prefix passed into the framework. Defaults to the passed in type_prefix.
```javascript
framework.type = type_prefix;
```
### allow_single_type
This key takes a boolean value, usually "true", that prevents activation of multiple frameworks of the same type. Defaults to "false".
```javascript
framework.allow_single_type = true;
```
### description
This key takes an HTML string describing the plugin and can contain a link that is displayed when creating a new page using a template from the plugin.
```javascript
framework.description = '<a href="http://my.website.com/">Custom plugin</a> that adds a really neat framework';
```
### author
This key takes an HTML string with the author's name and is displayed when creating a new page using a template from the plugin.
```javascript
framework.author = '<em>Pinegrow</em>';
```
### author_link
This key takes a URL link that will be opened if the author name is clicked in the new page pop-up.
```javascript
framework.author_description = 'https://pinegrow.com';
```
### info_badge
This key takes a short string that will be displayed when creating a new page using a template from the plugin. This is useful for displaying version numbers, for example.
```javascript
framework.info_badge = 'v1.0';
```
As shown below, this descriptor information will only be displayed when creating a plugin that adds a template for the user to select when starting a new page or project. It will not be displayed if the plugin only adds HTML snippets, actions, modifies workflow, or adds to the CMS. 
![Descriptors](./Images/Descriptors.png)

<a name="fhc"></a>
## Framework Helpers and Constructors  
Once your framework has been instantiated with descriptors there are a number of additional helpers and constructors made available through the API. Depending on the use-case only some of these may be needed. The most germane of these are listed below, with a short description of use case, arguments list, and example.  

### addTemplateProjectFromResourceFolder ( template_folder, done, index, filter_func, skip_add_to_templates, absolute_folder )
This function is used when constructing a plugin that provides one or more templates as a base for the user. For example, the stock Bootstrap 4 framework provides 19 starter templates when the user clicks on "New page or project".  
To utilize this in a plugin, create a folder that contains each template file, potentially a CSS file with the same name as the accompanying template file, plus an additional subfolder that must be named screenshots and contain an image to display to the user. Each image must have the same name as the template file.  
The folder can also contain a subfolder of resources to be used with the templates.  
![template folder example](./Images/folder_structure.png)  
Arguments
- template_folder - the source of the template folder relative to the main plugin file
- done - typically passed null, primarily for internal use, can take boolean
- index - determines the order of framework display - typically set to 100 or higher to add the plugin framework at the end
- filter_func - receives a function that can edit the files added to the template - usually not used
- skip_add_to_template - internal development use
- absolute_folder - internal development use
 ```javascript
framework.addTemplateProjectFromResourceFolder ('./template', null, 100);
```
  
### ignore_css_files
This key is used if the plugin is adding a template including customized CSS files that shouldn't be altered. It receives an object containing a single [regex string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) or multiple comma-separated regex strings. Any CSS file in the page resources that is matched by these string will be locked for editing.
Single string
```javascript
framework.ignore_css_files = [/my_plugin_style\.css/i];
```
multiple regex strings
```javascript
framework.ignore_css_files = [/my_plugin_style\.css/i, /my_other_styling\.css/i];
```
### PgComponentTypeResource ( resource_url, code )
The PGComponentTypeResource constructor is used to add javascript and CSS files to a project. In addition to the two arguments that can be passed at instantiation, it can accept a number of additional key:value pairs.  

| key | value |
| ----| ---- |
| type | mime type - this allows the Pinegrow app to determine the correct way to embed the file on the HTML page, \<script> and src for JavaScript, or \<link> and href for CSS. Automatically set to correct mime type by lookup.
| detect | regex string - determine if another file (like jQuery) exists on the page -- typically not used |
| footer | boolean - determines whether the item should be added to the head or the bottom of the body -- default: false |
| project | internal use only |
| isFolder | boolean - used to indicate if the resource is a folder or not -- default: false|
| source | url - typically the same as the resource_url, in some cases need to be converted to system path seperators using the Node.js [path.sep](https://nodejs.org/api/path.html#path_path_sep) -- default: null|
| relative_url | resource file location relative to the template -- default: null |
|replace| boolean (or function returning boolean) If .detect is used and a match is found then this indicates if the found resource should be replaced with the file at resource_url -- default: false -- typically not used|

Following creation and addition of key:value pairs, the new resource object is returned as a value to the framework in the "resources" key.
```javascript
framework.resources.add(new_resource)
```

Typical example code using file structure from above.
```javascript
var path = require( 'path' );
var toLocalPath = function(file_path) {
    return file_path.replace(/\//g, path.sep)
};
var resource_files = [
		'css/my-style.css',
		'js/my-js.js'
	];
resource_files.forEach(function (resource_file) {
		var file = framework.getResourceFile('template/resources/' + resource_file);
		var resource = new PgComponentTypeResource(file);
		resource.relative_url = resource_file;
		resource.source = toLocalPath(file);
		resource.footer = resource_file.indexOf('.js') >= 0;
		resource.type = resource_file.indexOf('.js') >= 0 ? 'application/javascript' : 'text/css';
		framework.resources.add(resource);
	});
```
## Creating Components  
### Overview  
The ``` PgComponentType ``` constructor is the main way to add new snippets, property controls, and actions to the Pinegrow App. These objects can be broken down into three sections.
  1) A main body that contains key:value pairs that contain the optional HTML snippet, give information on how to identify the component for property controls or actions to target on the page, and data on how to display the element on the tree.  
  2) Sections identify one or more groups of property controls or actions
  3) Fields are individual property controls or actions
  
  How these components are added to the Pinegrow App depend on component type. For components that add property controls only, the component is added to the framework object using ```addComponentType```.
  ```javascript
  var my_property_control = new PgComponentType( 'pg_my_control', 'My Control, {...} );
  framework.addComponentType(my_property_control);
  ```
  
  For components that add reusable HTML snippets or actions require an additional constructor, ```PgFrameworkLibSection```. This constructor creates a new panel that can be displayed in the Library or Actions Tab. Components for each panel are identified by passing an array of components using ```setComponentTypes```. This object is then added to the framework object using either ```addLibSection``` to pass it to the Library tab, or ```addActionsSection``` to display it on the Actions Tab.
  ```javascript
  var pg_custom_lib_section = new PgFrameworkLibSection( 'pg_my_custom_section', 'My Custom Section');
  pg_custom_lib_section.setComponentTypes([my_custom_component_one, my_custom_component_two]);
  framework.addLibSection(pg_custom_lib_section);
  ```
  or
  ```javascript
  var pg_custom_lib_section = new PgFrameworkLibSection( 'pg_my_custom_section', 'My Custom Section');
  pg_custom_lib_section.setComponentTypes([my_custom_component_one, my_custom_component_two]);
  framework.addActionsSection(pg_custom_lib_section);
  ```

### PgComponentType(unique_id, display_name, {options} )
This constructor is passed three arguments.

 1) A unique id. It is best practice to add a plugin specific prefix to the id to minimize potential conflict with other plugins. 
 2) A name that is displayed in the library or actions tab.
 3) An object that contains the HTML, controls, and or actions.

The options object can be further split into key:value pairs that provide the main body of the component, a section object that organizes all of the controls or actions, a set of field objects within the sections object that contain the key:value pairs that describe each control or action.  
 #### Main Body Key:Value Pairs  

**selector**  
This key receives either a CSS selector or function that uniquely identifies the element being created or controlled. This key is required.  
An example of a simple selector to target any page element with an attribute of 'pg-table'
```javascript
selector: '[pg-table]',
```
A example of a more complex selector using a javascript function. Note: the function can be passed a single argument that is conventionally named ```pgel```. This argument contains the source-code representation of the current DOM node (pgParserNode). This function checks to see if the tag of the node is NOT 'html', 'body','head', or 'script'. As a side note, ```pgel.tagName``` will always return lowercase, irrespective of the case in the Document.
```javascript
selector: function(pgel) {
	if (['html', 'body', 'head', 'script'].includes(pgel.tagName)) {
		return false;
	} else {
		return true;
	}
}
```
**parent_selector**  
## I think I need some help on this one. ?deprecated? only for certain elements?  

**priority**  
Determines the order in which an action or property control component will display in the panel. The default is 1000. This key is optional.  

**code**  
This key receives any reusable snippet - HTML, Javascript, PHP. This key is optional. If you are putting together a component consisting of controls or actions only it doesn't need to be added.  
```javascript
code: '<h2>The title of this article is <?php the_title(); ?>.</h2>',
```
**preview**  
This key receives a function that returns HTML representing what will be shown if the user hovers over the snippet in the library. This code is automatically generated from the ```code``` key:value pair. However, in the case of elements such as containers or rows it is useful to have a visual representation. This key is optional.
```javascript
Other component code...
    preview: getGridPreview('container'),
remainder of component code...

var getGridPreview = function(type) {
	var white = 'height: 20px;';
	var blue = 'height: 20px; background-color: #D8E5F2;';
		return '<div class="container-fluid" style="border:2px solid #0098cc; height: 120px;">\
			<div class="row">\
				<div style="' + white + '"></div>\
				<div style="' + blue + '"></div>\
				<div style="' + white + '"></div>\
			</div>\
			<div class="row">\
				<div style="' + white + '"></div>\
				<div style="' + blue + '"></div>\
				<div style="' + white + '"></div>\
			</div>\
		</div>';
	}
```
This code results in the following being displayed when the user hovers over the element in the Library panel.  
![Image displayed on element hover](Images/Preview.png)  
Note: If the ```code``` key has a value it will be appended to the preview HTML.  

#### Section Set-up and Key:Value Pairs  
The ```sections``` key receives an object of objects. Each object that it receives is a key:value pair with a unique name for key and an object for value. This object in turn has two required and one optional key:value pairs that define a set of controls. It is best practice to prefix the unique name of each section to insure it doesn't conflict with another plugin.  

Basic ```section``` structure
```javascript
sections: {
	pg_unique_name_one: {
		name: 'Displayed Control Name One',
		default_closed: true, \\optional
		fields:{...},
	},
	pg_unique_name_two: {
		name: 'Displayed Control Name Two',
		default_closed: false, \\optional
		fields: {...},
	}
}
```  
Properties Panel from the above code when element is selected
![Output for sections example.](Images/Section_example.png)  

 **name**  
 This key takes a name that will be displayed in the properties or actions panel. Note that alongside this name the Pinegrow App will list the source of the control or action. 

 **default_closed**  
 This key receives a boolean. If set to true, the resulting section will be closed by default on first display. This key is optional.  

 **fields**  
 This key is again an object of objects. Each individual object is a control or action.

 #### Fields Key:Value Pairs  

<a name="fch"></a>
## Framework CMS Helpers  

