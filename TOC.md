# API Introduction

# Plugin Framework Overview
# The Basics

## Framework elements
  * PgFramework()
  * addFramework()
  * type
  * allow_single_type
  * description
  * author
  * author_link
  * info_badge
  * ignore_css_files
  * addLibSection()
  * addActionsSection()
  * has_actions?
  * getActionTypes?
  * detect?



## Property Types

  * Property Overview  
  * Property Structure  
    * Control
      * selector
      * display_name
      * tags
      * empty_placeholder
      * code
      * priority
      * on_add
      * on_inserted
      * on_changed
    * Sections
      * name
      * default_closed
    * Fields
      * type
        * Text
        * Select
        * Checkbox
        * Filepicker
        * Custom
        * Colorpicker? 
      * name
      * action
        * element_attribute
        * apply_class
        * custom
      * options
      * value
      * show_empty
      * empty_attribute
      * get_value
      * set_value
      * on_changed
<br />  

## Event Hooks
### Plugin Events
  * on_page_loaded()
  * on_plugin_activated()
  * onFrameworkActivatedOnPage()?
### Component Events
  * on_inserted()
  * on_changed()
  * on_moved()
### Property Events
  * on_changed()
<br />  

## Helper Functions

### Framework Helpers
  * addTemplateProjectFromResourceFile()
  * setScriptFileByScriptTagId()
  * PgComponentTypeResource()
  * getResourceFile()
  * getResourceUrl()
  * getCurrentProject()
  * showAlert()  
  * showQuickMessage()
  * toLocalPath()
  * getMimeType()
  * copyFilesToProject()?
  * refreshCurrentProject()?

### Component Helpers
  * PgComponentType()
  * setComponentTypes()
  * addComponentType()
# Advanced Topics

## Conditionals
## Custom Controls  
  * onDefine()
  * onShow()
  * registerInputField()
  * showInputField()
  * addActionsSection()
  * PgToggleButtonMaker()
## Menu additions and hooks
  * addPluginControlToTopbar()
  * on_page_menu
  * 
## Stylesheet manipulation  
  * getAllCrsaStylesheets()?
  * PgSCSSStyleSheet()
  * PgLESSStylesheet()
  * removeExistingStylesheet()
  * 
### List of important windows variables?  
### List of available Node modules?
  * fs
  * path
### List of NW.js hooks
  * nw.gui
    * showDevTools
