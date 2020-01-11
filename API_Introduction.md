# API Introduction
The Pinegrow Web Editor is a tool for building responsive websites faster, with clean, non-opinionated code. The Pinegrow App can also be used as a CMS, allowing rapid templating of web pages that allow the end user to modify only selected content. Under the hood, Pinegrow is built with an SDK version of the NW.js App builder, around Node.js and the Chromium engine. This makes the Pinegrow App highly customizable and developer friendly. Plugins are created in JavaScript and programmers can take advantage of access to a number of Node.js modules and the Chromium Developer's Tools.

Extensions of the Pinegrow Web App are delivered through a main framework function. This framework exposes event hooks and functions that allow for manipulation of App content and detection of App state.  

Content is primarily added to three areas:  

1) The Library panel- this section of the App typically contains reusable HTML snippets that can be dragged to the main DOM page to visually build web pages. This library can also contain other items, such as scripts.  
2) The Properties panel - this section of the App contains controls that modify the on page code, such as adding a class or changing the src for an \<image>.  
3) The Actions panel - this section of the App is used both for modifying the on page code, but also for adding Pinegrow-specific code, such as defining locked areas on a CMS page.

In addition to adding content, the plugin API also allows for adding additional controls to Pinegrow, for example, a utility to minimize the active stylesheet on page save.

## Common Use Cases
1) **Component and Block Addition**  
   A common plugin for Pinegrow is a framework that adds new components to the library without accompanying properties or actions. Note: Pinegrow already contains an extensive set of property controls for most common HTML elements.
2) **Framework Addition**  
   This type of plugin adds not only components, but accompanying properties or actions that modify those components. Pinegrow ships with several Framework systems, including Bootstrap, that add both HTML snippets and extensive properties.
3) **Workflow Customization**  
   ?????Have to work on this - still not sure what people are doing as far as workflow.
4) **Streamline CMS template production**  
   This type of plugin can automate definition of editable areas, or populate new template pages with common code.

   While these are common use cases, there is no reason that a single plugin can't incorporate multiple functions.
