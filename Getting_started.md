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
Within the body of the plugin, a new framework object is created using the 'PgFramework' constructor and passing in a unique type that will be prefixed internally, along with a name for the framework.
```javascript
var framework = new PgFramework(type_prefix, name);
``` 
This framework variable can then be populated with a variety of key:value pairs. Some of these pairs are informational, like 'framework.author', which will be displayed to the end user, or give parameters to the Pinegrow App about how the plugin is supposed to be managed or displayed. The most important of these are listed below in the section [**framework descriptive elements**](#fde). Other pairs add the individual library components, items to the property panel, or actions panel. The most important of these are listed below in the section [**framework helpers and constructors**](#fhc). A third category of key:value pairs control the actions of the CMS and are listed below in the section [**framework CMS helpers**](#fch).  
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
| detect | regex string - determine if  |
| footer | boolean - determines whether the item should be added to the head or the bottom of the body |
| project |  |
| isFolder | |
| source | |
| relative_url | |
|replace| |
<a name="fch"></a>
## Framework CMS Helpers  

