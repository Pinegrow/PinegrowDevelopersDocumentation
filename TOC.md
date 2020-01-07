# API Introduction

# Plugin Framework Overview

# The Basics

## Framework elements
  * PgFramework()
  * key  
  * name  
  * show_in_action_tab  
  * detect  
  * component_types  
  * default  
  * lib_sections  
  * actions_section  
  * ignore_css_files  
  * type  
  * allow_single_type  
  * pluginUrl  
  * on_get_source  
  * user_lib  
  * changed  
  * not_main_types  
  * product  
  * trial  
  * trial_start_message  
  * trial_expired_message  
  * common_sections  
  * script_url  
  * description  
  * author  
  * author_link  
  * has_action  
  * info_badge  
  * auto_updatable  
  * order  
  * page_libs  
  * quick_actions  
  * show_in_manager  
  * preview_images_base  
  * resources  
  * resources_name  
  * customResourceNamespace  
  * ordered_list  
  * ordered  
  * types_map  
  * defs_that_filter_tree  
  * has_types_that_filter_tree  


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
  * PgResourcesList()  
  * CrsaProject()  
  * setScriptFileByScriptTagId()  
  * showAlert()  
  * getCurrentProject()  
  * getAllCrsaStylesheets()
  * getStylesheetByUrl()
  * getStylesheetByFileName()
  * addTemplateProjectFromResourceFile()
  * setUrlStylesheetForOverrides()
  * getResourceFile()
  * toLocalPath()
  * getSelectedPage()
  * getMimeType()
  * getSimpleMimeType()
  * PgEditableAreaExtension()

### Component Helpers
  * pgCreateNodeFromHtml
# Advanced Topics

## Conditionals
## Custom Controls  
  * onDefine()
  * onShow()
  * registerInputField()
  * showInputField()
  * addActionsSection()
## Menu additions and hooks
  * addPluginControlToTopbar()
  * on_page_menu
  * 
## Stylesheet manipulation  

### List of important windows variables?  
### List of available Node modules?
### List of NW.js hooks
  * nw.gui
    * showDevTools
