---
layout: doc
---
Custom renderers with Javascript ES5
====================================

Even if the controls that are created by the default renderers of JSON Forms are probably a good fit 
for several scenarios, you may still want to customize the rendered forms in certain situations.
This can be done by registering a custom renderer that produces a different UI control.
On this page, you will learn how to create and register a custom renderer in an application that uses *Javascript (ES 5)*.
Please note, there are separate articles for [Javascript ES6](#/docs/customrenderer-es6) and [Typescript](#/docs/customrenderer-ts).

In this article, we will replace the default renderer for integer values (see screenshot below)

{% 
   include image.html url="images/docs/customrenderer.ts.previewbefore.png" 
   description="The default rating control" 
%}   

so that it will use a *rating control* as shown below instead.

{% 
   include image.html url="images/docs/customrenderer.ts.preview.png" 
   description="The custom rating control" 
%}

This article is based on the "Make it happen" application. 
Please refer to the [sample application setup article](#/docs/setup), to learn how to set it up.
In the "Make it happen" example, we provide two states, the *initial* and the *final* state, 
which are located within the directories `app/initial` and `app/final`, respectively. 
Thus, when we refer to files, such as `index.html`, we actually refer to `app/initial/index.html`.

Preparations for creating the custom rating control
---------------------------------------------------

To implement the rating control as seen here

{% 
   include image.html url="images/docs/customrenderer.ts.preview.png" 
   description="The custom control we want to implement" 
%}

we use `ui-bootstrap`. Therefore, we first add the project dependency `"angular-ui-bootstrap": "1.3.3"` 
in `package.json` and add it in `index.html` as follows:

<pre nag-prism class="language-html" source="
<script src='node_modules/angular-ui-bootstrap/dist/ui-bootstrap.js'></script>
<script src='node_modules/angular-ui-bootstrap/dist/ui-bootstrap-tpls.js'></script>
"/>

Moreover, we have to import an additional module `ui.bootstrap` in `task.controller.js` as shown below:

<pre nag-prism class="language-javascript" source="
angular.module('MiHexample', ['jsonforms', 'ui.bootstrap'])
"/>

Now, we can add the rating control to the `index.html` in order to test that we have all of the dependencies. 
Let us put the following tag into `index.html`:

<pre nag-prism class="language-html" source="
  <div class='panel-body jsf'>
     <jsonforms schema='tc.taskSchema' uischema='tc.taskUISchema' data='tc.taskData'></jsonforms>
     <!-- we added this line -->
     <uib-rating ng-model='2' max='5'></uib-rating>
  </div>
"/>

When we run `npm install` and `npm start initial` again, we should see the following:


{% 
   include image.html url="images/docs/customrenderer.ts.formwithuibrating.png" 
   description="The rendered form together with the additional UI Bootstrap control" 
%}

As you can see, we are able to use the ui-bootstrap control in our HTML code. 
So it is now time to use it in a custom JSON forms renderer.
But let us remove the tag `<uib-rating ng-model="2" max="5"></uib-rating>` from `index.html`, 
as we just added it to test `ui-bootstrap`.

Let us now add a new property to the data schema and add a new control for it in the UI schema.
Therefore, we add the property `rating` of type `integer` with a maximum value of `5` to `tasks_schema.js`.

<pre nag-prism class="language-javascript" source="
angular.module('MiHexample').value('Schema', {
    'type': 'object',
    'properties': {
      'name': {
        'type': 'string',
        'minLength': 3
      },
      'description': {
        'type': 'string'
      },
      'rating': {
        'type': 'integer',
        'maximum': 5
      },            
      'done': {
        'type': 'boolean'
      }
    },
    'required': ['name']
});"/>

In `tasks_uischema.js`, we only have to add a new control for the property `rating` to the form.

<pre nag-prism class="language-javascript" source="
angular.module('MiHexample').value('UISchema', {
  'type': 'VerticalLayout',
  'elements': [
    {
      'type': 'Control',
      'label': 'Name',
      'scope': {
        '$ref': '#/properties/name'
      }
    },
    {
      'type': 'Control',
      'label': 'Description',
      'scope': {
        '$ref': '#/properties/description'
      },
      'options': {
        'multi': true
      }
    },
    {
      'type': 'Control',
      'label': 'Rating',
      'scope': {
        '$ref': '#/properties/rating'
      }
    },
    {
      'type': 'Control',
      'label': 'Done?',
      'scope': {
        '$ref': '#/properties/done'
      }
    }
  ]
});"/>

The default renderer will show an integer text field for the property `rating`.

{% 
   include image.html url="images/docs/customrenderer.ts.previewbefore.png" 
   description="The default renderer for an integer property" 
%}

In the next section, we will add a custom renderer to change this text field with a more appropriate control for assigning a rating.

{% 
   include image.html url="images/docs/customrenderer.ts.preview.png" 
   description="The customized renderer for an integer property" 
%}

Defining your custom renderer
-----------------------------

To create a custom renderer, we have to do three steps.

1. Create an Angular directive
2. Provide a controller for the directive
3. Register a new renderer that uses the created directive

Let us create the new Javascript file `rating.control.js` that will contain those three things. 
Also add `rating.controll.js` to the `index.html`:

<pre nag-prism class="language-html" source="
<script src='rating.control.js'></script>
"/>

### Create an Angular directive

In order to create an Angular directive, we add the code below to `rating.control.js`.

<pre nag-prism class="language-javascript" source="
angular.module('MiHexample')
    .directive('ratingControl', function() {
        return {
            restrict: 'E',
            controller: function() { /* TODO */ },
            controllerAs: 'vm',
            templateUrl: 'rating.control.html'
        };
    })
    .run(['RendererService', 'JSONFormsTesters', function(RendererService, Testers) {
        RendererService.register('rating-control', Testers.and(
            Testers.uiTypeIs('Control'),
            Testers.schemaTypeIs('integer')
        ), 10);
    }]);
"/>

As you can see above, we created a new Angular directive, which provides a template and a controller.
We'll look at the controller in a second, but let's first create the template file `rating.control.html`.
The content of `rating.control.html` looks as follows:

<pre nag-prism class="language-javascript" source="
<jsonforms-control>
    <uib-rating
            id='{{vm.id}}'
            readonly='vm.uiSchema.readOnly'
            ng-model='vm.resolvedData[vm.fragment]'
            max='vm.max()'>
    </uib-rating>
</jsonforms-control>
"/>

The template introduces the tag `jsonforms-control`, which contains the `uib-rating` tag from ui-bootstrap.
As parameters of this tag, we use values provided from a controller `RatingControl`, which we still must implement.
However, we can already see that we set the `readonly` flag as specified in the respective UI schema and bind the 
value of this control to a specific model value.
Moreover, we configure the parameter `max`, which indicates the maximum number of stars a user may give in the control, 
to a value that is computed in a function of `RatingControl`.

### Provide a controller for the directive

Let us now implement the `controller` property of the `RatingControl` in `rating.control.js`:

<pre nag-prism class="language-javascript" source="
 controller: ['BaseController', '$scope', function(BaseController, $scope) {
                var vm = this;
                BaseController.call(vm, $scope);
                vm.max = function() {
                    if (vm.resolvedSchema['maximum'] !== undefined) {
                        return vm.resolvedSchema['maximum'];
                    } else {
                        return 5;
                    }
                };
            }]
"/>

We create the controller by calling the `BaseController` factory, which provides common functionality for controls. 
We forward the `$scope` parameter as the second argument while the first one provides the scope of the 
current controller, which we can use to add methods and properties to the current controller.
In our case, we add `max()`, which we want to obtain the maximum value as specified in the data schema.

<pre nag-prism class="language-javascript" source="
'rating': {
  'type': 'integer',
  'maximum': 5 // <- this is the value we want to obtain
}
"/>

Therefore, we first resolve the data schema element for which this renderer should render a UI control.
As we subclassed `BaseController`, we may access the already resolved schema element via `resolvedSchema`.

Now, we can check whether the resolved schema element has a property `maximum`. 
If so, we will return its value; otherwise, we return a default maximum value of `5`.

### Register a new renderer that uses the created directive

Finally, the only thing that is left to do is to register the created directive and specify when we want to use our custom renderer.
Therefore, we need to call the `run` method.

<pre nag-prism class="language-javascript" source="
.run(['RendererService', 'JSONFormsTesters', function(RendererService, Testers) {
        RendererService.register('rating-control', Testers.and(
            Testers.uiTypeIs('Control'),
            Testers.schemaTypeIs('integer')
        ), 10);
    }]);
"/>

With this code, we tell Angular about our new directive `ratingControl` and register it at the `RendererService`.
We specify its name `rating-control` and define when it should be activated.
For defining when the renderer should be activated, JSON Forms provides a service `JSONFormsTesters`.
These testers check whether the schema element is of type `integer` and the property name is `rating`. 
Thus, our new renderer will only be activated for one particular property.

After we registered the new renderer, we can refresh the browser and should see our new renderer in action.

{% 
   include image.html url="images/docs/customrenderer.ts.finalform.png" 
   description="The finished form" 
%}


You can checkout the final state of the code of this article in `app/final` of the "Make it happen" source code.
