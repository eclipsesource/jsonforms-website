---
layout: doc
---
# Styling

## Use Bootstrap version

In order to use the Bootstrap version of JSONForms, you'll need to first load the CSS file of Bootstrap, 
e.g. by installing it via npm and referencing it in your index.html. 
Furthermore, you'll also need to load JSONForms specific CSS styling bits which are contained in 
the `jsonforms-bootstrap.css` file:
<pre nag-prism class="language-html" source="
<link rel='stylesheet' href='/node_modules/bootstrap/dist/css/bootstrap.css'/>
<link rel='stylesheet' href='/node_modules/jsonforms/dist/jsonforms-bootstrap.css'/>
"/>  

You'll also need to load an additional JS file `jsonforms-bootstrap.js` in order to fully integrate 
JSONForms with Bootstrap:

<pre nag-prism class="language-html" source="
<script src='node_modules/jsonforms/dist/jsonforms.js'></script>
<script src='node_modules/jsonforms/dist/jsonforms-bootstrap.js'></script>
"/>

Finally, you need to add the jsonforms-bootstrap module as a dependency of your application module:

<pre nag-prism class="language-javascript" source="
angular.module('MyModule', ['jsonforms','jsonforms-bootstrap'])
"/>  

## Use Material Design version

In order to use the Material Design version you additionally need to load JSONForms specific CSS stylings bits 
which are contained in the `jsonforms-material.css` file:
<pre nag-prism class="language-html" source="
<link rel='stylesheet' href='/node_modules/jsonforms/dist/jsonforms.css'/>
<link rel='stylesheet' href='/node_modules/jsonforms/dist/jsonforms-material.css'/>
"/>
You'll also need to load an additional JS file `jsonforms-material.js` in order to fully integrate 
JSONForms with Angular Material Design:

<pre nag-prism class="language-html" source="
<script src='node_modules/jsonforms/dist/jsonforms.js'></script>
<script src='node_modules/jsonforms/dist/jsonforms-material.js'></script>
"/>

Finally, you need to add the jsonforms-material module as a dependency of your application module:
<pre nag-prism class="language-javascript" source="
  angular.module('MyModule', ['jsonforms','jsonforms-material'])
"/>

<br>