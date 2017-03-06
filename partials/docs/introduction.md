---
layout: doc
---
Introduction - What is JSON Forms?
==================================

JSON Forms is a framework to efficiently build form-based web UIs. 
These UIs are targeted at entering, modifying, and viewing data and are usually embedded within an application.

JSON Forms eliminates the need to write HTML templates and Javascript for data binding by hand in order to create 
customizable forms.
It does so by leveraging the capabilities of JSON and [JSON schema](http://json-schema.org) and providing a simple and 
declarative way of describing forms. Forms are then rendered with a UI framework, currently one that is 
based on AngularJS.

The following diagram shows all main components of JSON Forms. Any UI is defined by using two schemata:

* The *data schema* defines the underlying data to be shown in the UI (objects, properties, and their types).
* The *UI schema* defines how this data is rendered as a form, e.g. the order of controls, their visibility, and the layout.

Both artefacts are interpreted during runtime by the rendering component. 
It translates the schemata into an AngularJS based UI,
which is already fully functional including data binding, validation, etc. 
The following diagram show a functional overview. On the left, you see a simple JSON schema 
defining a data object "Task" with three properties. 
On the right, you see a UI schema, which defines those three properties as controls in the UI. 
It therefore references the data schema. 
Additionally, it adds UI-specific concerns, e.g. that the "description" control is multi-line and that the "done" 
control uses an adapted label ("Done?"). 
All other information, e.g. the property type, or mandatory properties (in the example "name"), are derived 
from the data schema.
Based on those two schemata, the rendering component of JSON Forms produces the fully functional UI as shown below.

{% 
   include image.html url="images/docs/introduction.overview.png" 
   description="The schema and the UI schema act as inputs for JSON Forms" 
%}   

Please follow the article [Getting started with your first form](#/docs/firstform) to learn the simple steps 
necessary for producing such a form.

For the declarative description of a form, JSON forms uses a simple JSON format, which can be created using 
any editor of choice. However, JSON Forms also provides advanced tooling for creating and modifying UI schemata 
including a tree-based editor and a live preview. Please refer to 
the [JSON Forms Editor article](#/docs/jsonformseditor) to learn more about the editor.

{% 
   include image.html url="images/docs/introduction.jsonformseditor.png" 
   description="The JSON Forms Editor in action" 
%}   

JSON Forms provides default renderers for all data types, however, you can change the way things are 
rendered by providing [custom renderers](#/docs/customrenderer). 
Consider for instance, you want to add a new property of type integer called "rating" with a maximum value of 5 to 
the data schema of our example above. Additionally you wish to add a control for it below the description control 
in the UI schema. The default renderer will produce a form that looks as follows:

{% 
   include image.html url="images/docs/introduction.jsonforms.png" 
   description="A form renderer by JSON Forms" 
%}   

A plain textbox for entering a rating may not be ideal for all scenarios. Therefore JSON Forms allows you to 
create a custom renderer, which can be plugged into the framework. With such a custom renderer, 
the integer value is not displayed as a text box anymore, but rather, using a more appealing control that 
allows users to set the rating by selecting the number of stars they want to assign (see screenshot below). 
By plugging in custom renderers, you can adapt the UI created by JSON Forms to your requirements and also 
support domain specific data types and visualizations.

{% 
   include image.html url="images/docs/introduction.jsonformscustom.png" 
   description="The previously rendered form with a custom renderer" 
%}   
