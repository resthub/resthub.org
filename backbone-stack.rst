=================
Backbone.js Stack
=================

.. contents::
   :depth: 3

The Backbone.js stack includes the following main librairies :
    * **jQuery 1.7** (`documentation <http://docs.jquery.com/Main_Page>`_)
    * **Backbone.js 0.9.2** (`documentation <http://documentcloud.github.com/backbone/>`_) and its `localstorage adapter 
      <http://documentcloud.github.com/backbone/docs/backbone-localstorage.html>`_
    * **Underscore.js 1.3.3** (`documentation <http://documentcloud.github.com/underscore/>`_)
    * **Underscore.String 2.0.0** (`documentation <https://github.com/epeli/underscore.string#readme>`_)
    * **Require 2.0** with `i18n <http://requirejs.org/docs/api.html#i18n>`_ and `text <http://requirejs.org/docs/api.html#text>`_ plugins 
      (`documentation <http://requirejs.org/docs/api.html>`_)
    * **Handlebars 1.0** (`documentation <http://handlebarsjs.com>`_)
    * A **console shim** for browsers that don't support it
    * **Twitter Bootstrap 2.1** (`documentation <http://twitter.github.com/bootstrap/>`_) JS plugins
    
And some other complementary librairies : cf. :ref:`complementary-libs`

How should I use it in my project
=================================

There are 3 ways to use it in your project :
    * If you are starting a new Java project, the better way to use RESThub Backbone.js stack is to use one of the Backbone.js webappp 
      Maven Archetypes described on the `Getting Started page <getting-started.html>`_
    * You can simply go to `RESThub Backbone.js stack GitHub repository <https://github.com/resthub/resthub-backbone-stack>`_, download the 
      repository content and copy it at the root of your webapp
    * Last option (but deprecated since you don't see the stack files in your project), you can add the following code snippet to your pom.xml :

.. code-block:: xml

    <dependencies>
    
        <dependency>
            <groupId>org.resthub</groupId>
            <artifactId>resthub-backbone-stack</artifactId>
            <version>2.0-SNAPSHOT</version>
            <type>war</type>
        </dependency>

    </dependencies>

The following examples could also be useful to see real projects working with this stack:

- `Todo RESThub 2.0 example <http://github.com/resthub/todo-example>`_.
- `Tournament sample app <http://github.com/bmeurant/tournament-front>`_.

Please don't hesitate to send feedbacks `here <https://github.com/resthub/resthub-backbone-stack/issues>`_.

Project structure
=================

You should read carefully the awesome blog post `Organizing your application using Require.js Modules 
<http://backbonetutorials.com/organizing-backbone-using-modules/>`_ since it describes the project structure and principles recommended in RESThub Backbone stack based projects.

Bootstrapping
=============

Please find below the default files needed to bootstrap your webapp (An easier and error proof method is to use RESThub archetypes in 
order to bootstrap your project).

index.html
----------

.. code-block:: html

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="utf-8">
        <title>RESThub Backbone.js Bootstrap</title>
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
        <meta name="description" content="">
        <meta name="author" content="">

        <link href="css/bootstrap.css" rel="stylesheet">

        <!--[if lt IE 9]>
          <script src="http://html5shim.googlecode.com/svn/trunk/html5.js"></script>
        <![endif]-->

      </head>

      <body>
        
        <div id="main"> </div>
        
        <!-- Placed at the end of the document so the pages would load faster -->
        <script data-main="js/main" src="js/libs/require.js"></script>
      </body>
    </html>


index.html is provided by Backbone stack, so you don't have to create it. Your application bootstrap file is the main.js located 
at your webapp root (usually src/main/webapp). Please find below a sample :

.. code-block:: javascript

   // Set the require.js configuration for your application.
   require.config({

       shim:{
           'underscore':{
               exports:'_'
           },
           'underscore.string':{
               deps:[
                   'underscore'
               ]
           },
           'handlebars':{
               exports:'Handlebars'
           },
           'backbone':{
               deps:[
                   'underscore',
                   'underscore.string',
                   'jquery'
               ],
               exports:'Backbone'
           },
           'backbone-queryparams':{
               deps:[
                   'backbone',
                   'underscore'
               ]
           },
           'backbone-paginator':{
               deps:[
                   'backbone',
                   'underscore',
                   'jquery'
               ],
               exports:'Backbone.Paginator'
           },
           async:{
               deps:[
                   'underscore'
               ]
           }
       },

       // Libraries
       paths:{
           jquery:'libs/jquery',
           underscore:'libs/underscore',
           'underscore.string':'libs/underscore.string',
           backbone:'libs/backbone',
           localstorage:'libs/localstorage',
           text:'libs/text',
           i18n:'libs/i18n',
           pubsub:'resthub/pubsub',
           'bootstrap':'libs/bootstrap',
           'backbone-validation':'libs/backbone-validation',
           'resthub-backbone-validation':'resthub/backbone-validation.ext',
           handlebars:'libs/handlebars',
           'resthub-handlebars':'resthub/handlebars-helpers',
           hbs: 'resthub/handlebars-require',
           'backbone-queryparams':'libs/backbone.queryparams',
           'backbone-paginator':'libs/backbone.paginator',
           async:'libs/async.js',
           keymaster:'libs/keymaster'
       }
   });

   // Preload main libs
   require(['router'], function (Router) {

       Router.initialize();
   });
   
- **shim** config is part of `Require 2.0`_ and allows to `Configure the dependencies and exports for older, traditional "browser globals" 
  scripts that do not use define() to declare the dependencies and set a module value`. See `<http://requirejs.org/docs/api.html#config-shim>`_ for details.
- **path** config is also part of Require_ and allows to define paths for libs not found durectly under baseUrl. 
  See `<http://requirejs.org/docs/api.html#config-paths>`_ for details.
- resthub suggests to **preload some libs** that will be used surely as soon the app start (typically backbone itself and our template engine). This mechanism also
  allows us to load other linked libs transparently without having to define it repeatedly (e.g. ``underscore.string`` loading - this libs is strongly correlated
  to ``underscore`` - and merged with it and thus should not have to be defined anymore)

Guidelines
==========

View instantiation
------------------

Backbone views contains an $el attribute that represent the element (a div by default) where the template will be rendered, but it does not provide an attribute that represent the DOM element where the view will be attached.

In order to follow separation of concerns and encapsulation principles, RESThub Backbone stack guideline is to use a this.$root instance variabvle in order to specify where the View will be attached. This will also allow to manage in a nice way View lifecycle (multiple View creation/removal).

You should use the following pattern in all your views :

.. code-block:: javascript

    var MyView = Backbone.View.extend({

        initialize: function(options) {
            this.$root = options.root;
            this.$root.html(this.$el);
            
            /// ...
        },

        // For example
        render: function() {
            this.$el.html(template({
                messages:   messages
            }));
        }
    });
    return MyView;

This allows to avoid to hardcode the root element in the View, since the root is passed as parameter at instantiation :

.. code-block:: javascript
    
    var myView = new MyView({root: $('#content')});

2 remarks :
 * It is important to do this.$root.html(this.$el) in initialize, if you do it in render it will broke delegate events.
 * You can use append() or prepend() instead of html() in the this.$root.html(this.$el) line in collection views for example.

Collection View
---------------

In order to follow with separation of concerns and encapsulation principles, if you need to render a collection with its child elements, you should create a view for the collection and view for the model. The model view should be able to render itself.

You can see more details on the `Todo example <http://github.com/resthub/todo-example>`_ (have a look to TodosView and TodoView).

Always specify the context for event binding
--------------------------------------------

In order to allow automatic cleanup when the View is removed, you should always specfy the context when binding model or collection events :

.. code-block:: javascript
    
    // BAD : no context specify to the handler won't be cleanup when the view will be removed
    Todos.on('all', this.render);

    // GOOD : context will allow automatic cleanup of the handler when the view will be removed
    Todos.on('all', this.render, this);

You should also specify the model or collection attribute of your View in order to make it works.

Static versus instance varables
-------------------------------

If you want to be able to create different View instances, your have to manage properly the DOM element where the view will be attched as described previously. You also have to use instance variable.

Backbone way of declaring a static color variable :

.. code-block:: javascript

    var MyView = Backbone.View.extend({

        color : '#FF0000',

        initialize: function(options) {
            this.$root = options.root;
            this.$root.html(this.$el);
        }
           
    });
    return MyView;

Backbone way of declaring an instance color variable :

.. code-block:: javascript

    var MyView = Backbone.View.extend({

        initialize: function(options) {
            this.$root = options.root;
            this.$root.html(this.$el);

            this.color = '#FF0000';
        }
           
    });
    return MyView;

Events
------

Backbone default event list is available `here <http://backbonejs.org/#FAQ-events>`_.

Templating
==========

Client side templating capabilities are based by default on Handlebars_.

Templates are HTML fragments, without the <html>, <header> or <body> tag :

.. code-block:: html

    <div class="todo {{#if done}}done{{/if}}">
        <div class="display">
            <input class="check" type="checkbox" {{#if done}}checked="checked"{{/if}}/>
            <div class="todo-content">{{content}}</div>
            <span class="todo-destroy"></span>
        </div>
        <div class="edit">
            <input class="todo-input" type="text" value="{{content}}" />
        </div>
    </div>

Templates are injected into Views thanks to RequireJS Handlebars plugin, based on RequireJS text plugin. This hbs plugin will automatically retreive and compile your template. So it should be defined in your main.js :

.. code-block:: javascript

    require.config({
        paths: {
            // ...
            text: 'libs/text',
            hbs: 'resthub/handlebars-require'
        }
    });

Sample usage in a Backbone.js View :

.. code-block:: javascript

    define(['jquery', 'backbone', 'handlebars', 'hbs!templates/todo.html'],function($, Backbone, Handlebars, todoTmpl) {
        var TodoView = Backbone.View.extend({

        //... is a list tag.
        tagName:  'li',

        render: function() {
            // todoTmpl a function that take context (labels, model) and return the dynamized output.
            var result = todoTmpl(this.model.toJSON());
            $(this.el).html(result);
            return this;
        }
    });
    
.. _handlebars-helpers:
    
Helpers
-------

Resthub provide some usefull **Handlebars helpers** included by default in ``main.js`` :

ifinline
++++++++

This helper provides a more fluent syntax for inline ifs. i.e. if embedded in quoted strings.

As with Handlebars ``#if``, if its first argument returns ``false``, ``undefined``, ``null``
or ``[]`` (a "falsy" value), ``''`` is returned, otherwise ``returnVal`` argument is rendered.

e.g:

.. code-block:: html

   <div class='{{ifinline done "done"}}'>Issue number 1</div>

with the following context:

.. code-block:: javascript

   {done:true}
   
will produce:

.. code-block:: html

   <div class='done'>Issue number 1</div>

unlessinline
++++++++++++

Opposite of ifinline helper.

As with Handlebars ``#unless``, if its first argument returns ``false``, ``undefined``, ``null``
or ``[]`` (a "falsy" value), ``returnVal`` is returned, otherwise ``''`` argument is rendered.

e.g:

.. code-block:: html

   <div class='{{unlessinline done "todo"}}'>Issue number 1</div>

with the following context:

.. code-block:: javascript

   {done:false}
   
will produce:

.. code-block:: html

   <div class='todo'>Issue number 1</div>

ifequalsinline
++++++++++++++

This helper provides a if inline comparing two values.

If the two values are strictly equals (``===``) return the returnValue argument, ``''`` otherwise.

e.g:

.. code-block:: html

   <div class='{{ifequalsinline type "details" "active"}}'>Details</div>

with the following context:

.. code-block:: javascript

   {type:"details"}
   
will produce:

.. code-block:: html

   <div class='active'>Details</div>

unlessequalsinline
++++++++++++++++++

Opposite of ifequalsinline helper.

If the two values are not strictly equals (``!==``) return the returnValue  argument, ``''`` otherwise.

e.g:

.. code-block:: html

   <div class='{{unlessequalsinline type "details" "active"}}'>Edit</div>

with the following context:

.. code-block:: javascript

   {type:"edit"}
   
will produce:

.. code-block:: html

   <div class='active'>Edit</div>

ifequals
++++++++

This helper provides a if comparing two values.

If only the two values are strictly equals (``===``) display the block

e.g:

.. code-block:: html

   {{#ifequals type "details"}}
      <span>This is details page</span>
   {{/ifequals}}

with the following context:

.. code-block:: javascript

   {type:"details"}
   
will produce:

.. code-block:: html

   <span>This is details page</span>

unlessequals
++++++++++++

Opposite of ifequals helper.

If only the two values are not strictly equals (``!==``) display the block

e.g:

.. code-block:: html

   {{#unlessequals type "details"}}
      <span>This is not details page</span>
   {{/unlessequals}}

with the following context:

.. code-block:: javascript

   {type:"edit"}
   
will produce:

.. code-block:: html

   <span>This is not details page</span>

for
+++

This helper provides a for i in range loop.

start and end parameters have to be integers >= 0 or their string representation. start should be <= end.
In all other cases, the block is not rendered.

e.g:

.. code-block:: html

   <ul>
      {{#for 1 5}}
         <li><a href='?page={{this}}'>{{this}}</a></li>
      {{/for}}
   </ul>
   
will produce:

.. code-block:: html

   <ul>
      <li><a href='?page=1'>1</a></li>
      <li><a href='?page=2'>2</a></li>
      <li><a href='?page=3'>3</a></li>
      <li><a href='?page=4'>4</a></li>
      <li><a href='?page=5'>5</a></li>
   </ul>
   
sprintf
+++++++

This helper allows to use sprintf C like string formatting in your templates. It is based on `Underscore String <https://github.com/epeli/underscore.string>`_ implementation. A detailed documentation is available `here <http://www.diveintojavascript.com/projects/javascript-sprintf>`_.

e.g:

.. code-block:: html

   <span>{{sprintf "This is a %s" "test"}}</span>

will produce:

.. code-block:: html

   <span>This is a test</span>

This helper is very usefull for Internationalization_, and can take any number of parameters.

.. _complementary-libs:

Complementary libs
==================

Resthub selected and embed some complementary utility libs provinding advanced functionalities for a complete and real backbone (and resthub)
based application. These libs are : 

- **Form Validation:** `Backbone Validation`_
- **Parameters support on view routing:** `Backbone Query Parameters`_
- **Paginated lists:** `Backbone Paginator`_
- **Asynchronous calls:** Async_
- **Dispatching keyboard shortcuts:** Keymaster_

Resthub only provide require config shims and paths for these libs in ``main.js`` and you are totaly free to use these libs or not:

.. code-block:: javascript

   require.config({

      shim:{
        ...
        'backbone-queryparams':{
            deps:[
                'backbone',
                'underscore'
            ]
        },
        'backbone-paginator':{
            deps:[
                'backbone',
                'underscore',
                'jquery'
            ],
            exports:'Backbone.Paginator'
        },
        async:{
            deps:[
                'underscore'
            ]
        }
      },

      // Libraries
      paths:{
        'backbone-validation':'libs/backbone-validation',
        'resthub-backbone-validation':'resthub/backbone-validation.ext',
        'backbone-queryparams':'libs/backbone.queryparams',
        'backbone-paginator':'libs/backbone.paginator',
        async:'libs/async.js',
        keymaster:'libs/keymaster'
      }
   });

Form Validation : Backbone Validation
-------------------------------------

Backbone_ does not provide natively **any tool for form or validation management**. It is not necessary
to specify model attributes or related constraints.

In terms of validation, Backbone_ provides only empty methods ``validate`` and ``isValid`` that have to be implemented by each developer. 
The only guarantee that the ``validate`` method is called before a ``save`` (canceled on error). But a complete form validation is 
not obvious (custom error array management ... ) and the errors are not distinguishable from inherent ``save`` errors (server communication and so on).

`Backbone Validation`_ **only focus on validation aspects** and leaves us free to write our form. The lib has **a very large number of built-in 
validators** and **provides effective validators customization and extension mechanisms**.

`Backbone Validation`_ does not neither propose automatic linking between form and model and leaves us the choice to use a dedicated lib or 
to implement custom behaviour (before the validation, process all form values to set to model). The behaviour of `Backbone Validation`_ perfectly matches standard
Backbone_ workflow through ``validate`` and ``isValid`` methods.

**Model** : constraints definition:

.. code-block:: javascript

   define([
       'underscore',
       'backbone',
       'resthub-backbone-validation'
   ], function (_, Backbone) {

       /**
        * Definition of a Participant model object
        */
       var ParticipantModel = Backbone.Model.extend({
           urlRoot:App.Config.serverRootURL + "/participant",
           defaults:{

           },

           // Defines validation options (see Backbone-Validation)
           validation:{
               firstname:{
                   required:true
               },
               lastname:{
                   required:true
               },
               email:{
                   required:false,
                   pattern:'email'
               }
           },

           initialize:function () {
           }

       });
       return ParticipantModel;

   });

**HTML5 Form** :

.. code-block:: html

   {{#with participant}}
       <form class="form-horizontal">
           <fieldset>
               <div class="row">
                   <div class="span8">
                       <div class="control-group">
                           {{#if id}}
                               <label for="participantId" class="control-label">Id:</label>
                               <div class="controls">
                                   <input id="participantId" name="id" type="text" value="{{id}}" disabled/>
                               </div>
                           {{/if}}
                       </div>

                       <div class="control-group">
                           <label for="firstname" class="control-label">First name:</label>
                           <div class="controls">
                               <input type="text" id="firstname" name="firstname" required="true" value="{{firstname}}" tabindex="1" autofocus="autofocus"/>
                               <span class="help-inline"></span>
                           </div>
                       </div>

                       <div class="control-group">
                           <label for="lastname" class="control-label">Last name:</label>
                           <div class="controls">
                               <input type="text" id="lastname" name="lastname" required="true" value="{{lastname}}" tabindex="2"/>
                               <span class="help-inline"></span>
                           </div>
                       </div>

                       <div class="control-group">
                           <label for="email" class="control-label">email address:</label>
                           <div class="controls">
                               <input type="email" id="email" name="email" value="{{email}}" tabindex="3"/>
                               <span class="help-inline"></span>
                           </div>
                       </div>

                   </div>
           </fieldset>
       </form>
   {{/with}}


**View** : initialization and usage:

.. code-block:: javascript

   initialize:function () {

       ...

       // allow backbone-validation view callbacks (for error display)
       Backbone.Validation.bind(this);

       ...
   },

   ...

   /**
    * Save the current participant (update or create depending of the existence of a valid model.id)
    */
   saveParticipant:function () {

       // build array of form attributes to refresh model
       var attributes = {};
       this.$el.find("form input[type!='submit']").each(function (index, value) {
           attributes[value.name] = value.value;
           this.model.set(value.name, value.value);
       }.bind(this));

       // save model if it's valid, display alert otherwise
       if (this.model.isValid()) {
           this.model.save(null, {
               success:this.onSaveSuccess.bind(this),
               error:this.onSaveError.bind(this)
           });
       }
       else {
           ...
       }

If you're using ``resthub-backbone-validation`` instead of the original lib (cf. :ref:`resthub-extensions`), you also natively
beneficate of custom validation callbacks allowing to render validation errors in a form structured with `Twitter Bootstrap`_.


Parameters support on view routing : Backbone Query Parameters
--------------------------------------------------------------

Backbone_ routes management allows to define permet such routes :
``"participants":"listParticipants"`` and ``"participants?:param":"listParticipantsParameters"``. But the native 
behaviour seems not sufficient:

- **management of an unknown number of parameters** (ex ``?page=2&filter=filter``) is not obvious
- we have to define (at least) **two routes to handle calls with or without parameters** without duplication
and without too much technical code

Expected behaviour was that the **map a single route to a method with an array of request parameter as optional parameter.**

`Backbone Query Parameters`_ provides this functionality.

With this lib, included once and for all in the main router, You 'll get the following:

**router.js** :

.. code-block:: javascript

   define(['backbone', 'backbone-queryparams'], function (Backbone) {
       var AppRouter = Backbone.Router.extend({
         routes:{
             // Define some URL routes
             ...

             "participants":"listParticipants",

             ...
         },

         ...

         listParticipants:function (params) {
             // params contains the list of all query params of is empty if no param
         }
      });
   });

Query parameters array is automatically recovered **without any further operation** and **whatever the number
of these parameters**. It can then be passed to the view constructor for initialization:

**list.js** :

.. code-block:: javascript

   askedPage:1,

   initialize:function (params) {

       ...

       if (params) {
           if (params.page && this.isValidPageNumber(params.page)) this.askedPage = parseInt(params.page);
       }

       ..
   },

Paginated lists : Backbone Paginator
-----------------------------------

`Backbone Paginator`_ offers both client side pagination (``Paginator.clientPager``) and integration with server side pagination
(``Paginator.requestPager``). It includes management of filters, sorting, etc.

Client side pagination
++++++++++++++++++++++

This lib extends Backbone_ collections. So adding options to collections is necessary:

.. code-block:: javascript

   var participantsCollection = Backbone.Paginator.clientPager.extend({
       model:participantModel,
       paginator_core:{
           // the type of the request (GET by default)
           type:'GET',

           // the type of reply (jsonp by default)
           dataType:'json',

           // the URL (or base URL) for the service
           url:App.Config.serverRootURL + '/participants'
       },
       paginator_ui:{
           // the lowest page index your API allows to be accessed
           firstPage:1,

           // which page should the paginator start from
           // (also, the actual page the paginator is on)
           currentPage:1,

           // how many items per page should be shown
           perPage:12,

           // a default number of total pages to query in case the API or
           // service you are using does not support providing the total
           // number of pages for us.
           // 10 as a default in case your service doesn't return the total
           totalPages:10
       },
       parse:function (response) {
           return response;
       }
   });

Then we ``fetch`` the collection and then ask for the right page:

.. code-block:: javascript

    this.collection = new ParticipantsCollection();

    // get the participants collection from server
    this.collection.fetch(
     {
         success:function () {
             this.collection.goTo(this.askedPage);
         }.bind(this),
         error:function (collection, response) {
             ...
         }
     });

Once the collection retrieved, ``collection.info()`` allows to get information about current state:

.. code-block:: javascript

   totalUnfilteredRecords
   totalRecords
   currentPage
   perPage
   totalPages
   lastPage
   previous
   next
   startRecord
   endRecord

Server side pagination
++++++++++++++++++++++

Once client side pagination implemented, server adaptation is very easy:

We set **parameters to send to server** in ``collections/participants.js``:

.. code-block:: javascript

   server_api:{
       'page':function () {
           return this.currentPage;
       },

       'size':function () {
           return this.perPage;
       }
   },

Then, in the same file, we provide a parser to get the response back and initialize collection and pager:

.. code-block:: javascript

   parse:function (response) {
       var participants = response.content;
       this.totalPages = response.totalPages;
       this.totalRecords = response.totalElements;
       this.lastPage = this.totalPages;
       return participants;
   }

Finally, we change server call : this time the ``goTo`` method extend ``fetch`` and should be called instead
(``views/participants/list.js``) :

.. code-block:: javascript

   // get the participants collection from server
   this.collection.goTo(this.askedPage,
       {
           success:function () {
               ...
           }.bind(this),
           error:function () {
               ...
           }
       });

All other code stay inchanged but the ``collection.info()`` is a little bit thinner:

.. code-block:: javascript

   totalRecords
   currentPage
   perPage
   totalPages
   lastPage


Asynchronous calls : Async
--------------------------

Other recurrent problem: parallel asynchronous calls for which we want to have a
final processing in order to display the results of the entire process: number of errors, successes,
etc.

Basically, each asynchronous call define a callback invoked at the end of his own treatment (success or error).
Without tools, we are thus obliged to implement a **manual count of called functions and a count
of callbacks called to compare**. The final callback is then called at the end of each call unit
but executed only if there is no more callback to call. This gives:

.. code-block:: javascript

   /**
    * Effective deletion of all element ids stored in the collection
    */
   deleteElements:function () {

       var self = this;
       var nbWaitingCallbacks = 0;

       $.each(this.collection, function (type, idArray) {
           $.each(idArray, function (index, currentId) {
               nbWaitingCallbacks += 1;

               $.ajax({
                   url:App.Config.serverRootURL + '/participant/' + currentId,
                   type:'DELETE'
               })
                   .done(function () {
                       nbWaitingCallbacks -= 1;
                       self.afterRemove(nbWaitingCallbacks);
                   })
                   .fail(function (jqXHR) {
                       if (jqXHR.status != 404) {
                           self.recordError(type, currentId);
                       }
                       nbWaitingCallbacks -= 1;
                       self.afterRemove(nbWaitingCallbacks);
                   });
           });
       });
   },

   /**
    * Callback called after an ajax deletion request
    *
    * @param nbWaitingCallbacks number of callbacks that we have still to wait before close request
    */
   afterRemove:function (nbWaitingCallbacks) {

       // if there is still callbacks waiting, do nothing. Otherwise it means that all request have
       // been performed : we can manage global behaviours
       if (nbWaitingCallbacks == 0) {
           // do something
       }
   },


This code work but there is **too much technical code** !

Async_ provides a set of helpers to perform **asynchronous parallel processing** and synchronize the end of 
these treatments through a final callback called once.

This lib is initially developed for nodeJS server but has been **implemented on browser side**.

Theoretically, the method we currently need is ``forEach``. However, we faced the following problem: all of these helpers
are designed to stop everything (and call the final callback) when the first error occurs.
But if we need to perform all server calls and only then, whether successful or fail, return global results
to the user, there is unfortunately no appropriate option (despite similar requests on mailing lists) ...

You can twick a little and, instead of ``forEach``, use the ``map`` function that returns a result array
in which you can register successes and errors. error parameter of the final callback cannot be used without
stopping everything. So, the callback should always be called with a ``null`` err parameter and a custom wrapper containing the
returned object and the type of the result: ``success`` or ``error``. You can then globally count errors without
interrupting your calls:

.. code-block:: javascript

   /**
    * Effective deletion of all element ids stored in the collection
    */
   deleteElements:function () {

       ...

       async.map(elements, this.deleteFromServer.bind(this), this.afterRemove.bind(this));
   },

   deleteFromServer:function (elem, deleteCallback) {
       $.ajax({
           url:App.Config.serverRootURL +'/' + elem.type + '/' + elem.id,
           type:'DELETE'
       })
       .done(function () {
           deleteCallback(null, {type:"success", elem:elem});
       })
       .fail(function (jqXHR) {
           ...

           // callback is called with null error parameter because otherwise it breaks the
           // loop and top on first error :-(
           deleteCallback(null, {type:"error", elem:elem});
       }.bind(this));
   },

   /**
    * Callback called after all ajax deletion requests
    *
    * @param err always null because default behaviour break map on first error
    * @param results array of fetched models : contain null value in cas of error
    */
   afterRemove:function (err, results) {

       // no more test
       ...
   },

Dispatching keyboard shortcuts : Keymaster
------------------------------------------

Keymaster_ is a micro library allowing to define listeners on keyboard shortcuts and propagate them. 
The syntax is elegant, it is very simple while very complete:

- Management of multiple hotkeys
- Chaining through an important number of "modifiers"
- Source DOM element type filtering
- ...

It is so simple that the doc is self sufficient - see `here <http://github.com/madrobby/keymaster>`_

.. _resthub-extensions:

Resthub extensions
==================

For some of suggested embedded libs, resthub provides extensions.

These extensions can be found, as any other custom resthub lib, in ``js/resthub`` directory.

Resthub provides currently these extensions : 

- Handlebars_ helpers extension : Addition of some usefull Handlebars helpers. cf :ref:`handlebars-helpers` and `Github source <http://github.com/resthub/resthub-backbone-stack/blob/master/js/resthub/handlebars-helpers.js>`_.
- Handlebars_ RequireJS plugin in order to retreive and compile automatically Handlebars templates
- `Backbone Validation`_ extension : Validation callbacks (``valid`` and ``invalid``) extension to provide a native integration 
  with `Twitter Bootstrap`_ form structure (``controls`` and ``control-group``). cf. `Github source <http://github.com/resthub/resthub-backbone-stack/blob/master/js/resthub/backbone-validation.ext.js>`_

To beneficate of these extensions, we suggest you to replace standard lib inclusion in your require define by the inclusion
of these libs.

e.g.

.. code-block:: javascript

   define([
       'resthub-handlebars',
       'resthub-backbone-validation'
   ], function (Handlebars, BackboneValidation) {
      ...
   });
   
By default, resthub archetype generates view including these extensions instead of original libs. Each extension depends on the original lib.

If you don't want to use these extensions, you only have to use the original lib alone : 

.. code-block:: javascript

   define([
       'handlebars',
       'backbone-validation'
   ], function (Handlebars, BackboneValidation) {
      ...
   });
   
All extensions paths and shims are defined in ``main.js`` :

.. code-block:: javascript

   paths:{
      ...
      'backbone-validation':'libs/backbone-validation',
      'resthub-backbone-validation':'resthub/backbone-validation.ext',
      handlebars:'libs/handlebars',
      'resthub-handlebars':'resthub/handlebars-helpers',
      ...
    }

.. todo:: add backbone extension

Avoid caching issues
====================

In order to avoid caching issues when, for example, you update your JS or HTML files, you should use the 
`urlArgs RequireJS attribute <http://requirejs.org/docs/api.html#config>`_. You can filter the ${buildNumber} with your build 
tool at each build.


main.js:

.. code-block:: javascript

    require.config({
        paths: {
            // ...
        },
        urlArgs: 'appversion=${buildNumber}''
    });

main.js after filtering:

.. code-block:: javascript

    require.config({
        paths: {
            // ...
        },
        urlArgs: 'appversion=${738792920293847}'
    });

Internationalization
====================

You should never use directly labels or texts in your source files. All labels should be externalized in order to prepare your 
application internationalization. Doing such thing is pretty simple with RESThub Backbone.js stack thanks to `requireJS i18n 
plugin <http://requirejs.org/docs/api.html#i18n>`_.

Please find below the steps needed to internationalize your application.

Configure i18n plugin
---------------------

In your main.js file you should define a shortcut path for i18n plugin and the default language for your application :

.. code-block:: javascript

    require.config({
        paths: {
            // ...
            i18n: "libs/i18n"
        },
        locale: localStorage.getItem('locale') || 'en-us'
    });


Define labels
-------------

Create a labels.js file in the js/nls directory, it will contain labels in the default locale used by your application. You 
can change labels.js to another name (messages.js or functionality related name like user.js or product.js) but js/nls is the 
default location. Specify at the same level than the root node the available translations.

Sample js/nls/labels.js file:

.. code-block:: javascript

    define({
        // root is mandatory.
        'root': {
            'titles': {
                'login': 'Login'
            }
        },
        "fr-fr": true
    });

Add translations in subfolders named with the locale, for example js/nls/fr-fr ...
You should always keep the same file name, and the file located at the root will be used by default.

Sample js/nls/fr-fr/labels.js file:

.. code-block:: javascript

    define({
        // root is mandatory.
        'root': {
            'titles': {
                'login': 'Connexion'
            }
        }
    });

Use it
------

Add a dependency in the js, typically a View, where you'll need labels. You'll absolutely need to give a scoped variable to 
the result (in this example ``labels``, but you can choose the one you want). 

Prepending 'i18n!' before the file path in the dependency indicates RequireJS to get the file related to the current locale :

.. code-block:: javascript

    define(['i18n!nls/labels'], function(labels) {
        // ...

        render: function() {
            $(this.el).html(this.template(labels));
            return this;
        },

        // ...
    });

In in your html template :

.. code-block:: html

    <div class="title">
        <h1><%= labels.titles.login %></h1>
    </div>

Change locale
-------------

Changing locale require a page reloading, so it is usually implemented with a Backbone.js router configuration like the following one :

.. code-block:: javascript

    define(['backbone'], function(Backbone){
        var AppRouter = Backbone.Router.extend({
            routes: {
                'fr': 'fr',
                'en': 'en'
            },
            fr: function( ){
                var locale = localStorage.getItem('locale');
                if(locale != 'fr-fr') {
                    localStorage.setItem('locale', 'fr-fr'); 
                    location.reload(); 
                }
            },
            en: function( ){
                var locale = localStorage.getItem('locale');
                if(locale != 'en-us') {
                    localStorage.setItem('locale', 'en-us'); 
                    location.reload();
                }
            }
        });

        return AppRouter;
    });

sprintf to the rescue
---------------------

Internalionalization can sometimes be tricky since words are not always at the same position depending on the language. In order to make it easier to use, 
RESThub backbone stack include Underscore.String. It contains a sprintf function that you can use for your translations.

RESThub also provide a ``sprintf`` handlebars helper to use directly in your templates.

You can use the ``_.sprintf()`` function and the ``sprintf`` helper in order to have some replacement in your labels.

labels.js

.. code-block:: javascript

    'root': {
        'clearitem'    : "Clear the completed item",
        'clearitems' : 'Clear %s completed items',
    }

And in your template

.. code-block:: html

    {{#ifequals done 1}} {{messages.clearitem}} {{else}} {{sprintf messages.clearitems done}} {{/ifequals}}

Inheritance
===========

As described by `k33g <https://twitter.com/#!/k33g_org>`_ on his `Gist Use Object Model of BackBone <https://gist.github.com/2287018>`_, 
it is possible to reuse Backbone.js extend() function in order to get simple inheritance in Javascript.

.. code-block:: javascript

    // Define an example Kind class
    var Kind = function() {
        this.initialize && this.initialize.apply(this, arguments);
    };
    Kind.extend = Backbone.Model.extend;

    // Create a Human class by extending Kind
    var Human = Kind.extend({
        toString : function() { console.log("hello : ", this); },
        initialize : function (name) {
            console.log("human constructor");
            this.name = name
        }
    });

    // Call parent constructor
    var SomeOne = Human.extend({
        initialize : function(name){
            SomeOne.__super__.initialize.call(this, name);
        }
    });

    // Create an instance of Human class
    var Bob = new Human("Bob");
    Bob.toString();

    // Create an instance of SomeOne class
    var Sam = new SomeOne("Sam");
    Sam.toString();

    // Static members
    var Human = Kind.extend({
        toString : function() { console.log("hello : ", this); },
        initialize : function (name) {
            console.log("human constructor");
            this.name = name
        }
    },{ //Static
        counter : 0,
        getCounter : function() { return this.counter; }
    });

Publish Subscribe
=================

pubsub.js implements a simple event bus, allowing loosely coupled software design in your application.
It is an elegant way to enable communication between Views without introducing strong coupling between them.

API
---

.. code-block:: javascript
 
        /**
         * Define an event handler for this eventType listening on the event bus
         *
         * subscribe( type, callback )
         * @param {String} type A string that identifies your custom javaScript event type
         * @param {function} callback(args) function to execute each time the event is triggered
         * 
         * @return Handle used to unsubscribe.
         */
        Pubsub.subscribe(eventType, handler(args));
      
        /**
         * Remove a previously-defined event handler for the matching eventType
         * 
         * @param {String} handle The handle returned by the $.subscribe() function
         */
        Pubsub.unsubscribe(handle);
      
        /**
         * Publish an event in the event bus
         * 
         * @param {String} type A string that identifies your custom javaScript event type
         * @param {Array} data  Parameters to pass along to the event handler
         */
        Pubsub.publish(eventType, [extraParameters]);

Usage
-----

.. code-block:: javascript

    define(['pubsub'], function(Pubsub) {
        // TODO
    }        
    
    
.. _Require 2.0: http://requirejs.org
.. _Require: http://requirejs.org
.. _Handlebars: http://handlebarsjs.com
.. _Backbone Validation: http://github.com/thedersen/backbone.validation
.. _Twitter Bootstrap: http://twitter.github.com/bootstrap/
.. _Backbone Paginator: http://addyosmani.github.com/backbone.paginator/
.. _Backbone Query Parameters: http://github.com/jhudson8/backbone-query-parameters
.. _Async: http://github.com/caolan/async/
.. _Keymaster: http://github.com/madrobby/keymaster
.. _Backbone: http://backbonejs.org/
