# Getting Started
All Pinegrow plugins are instantiated by calling a function, either anonymous or named, once the Pinegrow App signals it is ready. This function gets passed the event and the 'pinegrow' window variable as arguments.
```javascript
$(function(){
    $('body').one('pinegrow-ready', function(e, pinegrow) {
        //plugin body
    })
})
```
or
```javascript
$(function(){
    $('body').one('pinegrow-ready',function(e, pinegrow) {
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
###
<a name="fhc"></a>
## Framework Helpers and Constructors  

<a name="fch"></a>
## Framework CMS Helpers  

