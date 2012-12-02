:tocdepth: 2

=================
Backbone.js Stack
=================

RESThub Backbone stack provides a client-side full stack and guidelines for building enterprise grade HTML5 applications. It could be used with any server backend: Ruby, PHP, NodeJS, JEE, Spring, Grails ...

In addition to the existing librairies included in the stack, it provides additional functionalities (mainly Backbone.js addons) designed to allow you to build a real enterprise grade application, and described in this documentation.

.. contents::
   :depth: 3
   
The Backbone.js stack includes the following librairies:
    * jQuery 1.7 (`documentation <http://docs.jquery.com/Main_Page>`_)
    * Backbone.js 0.9.2 (`documentation <http://documentcloud.github.com/backbone/>`_) and its `localstorage adapter 
      <http://documentcloud.github.com/backbone/docs/backbone-localstorage.html>`_
    * Underscore.js 1.3.3 (`documentation <http://documentcloud.github.com/underscore/>`_)
    * Underscore.String 2.0.0 (`documentation <https://github.com/epeli/underscore.string#readme>`_)
    * Require.js 2.0 with `i18n <http://requirejs.org/docs/api.html#i18n>`_ and `text <http://requirejs.org/docs/api.html#text>`_ plugins 
      (`documentation <http://requirejs.org/docs/api.html>`_)
    * Handlebars 1.0 (`documentation <http://handlebarsjs.com>`_)
    * A console shim + client logging to server mechanism
    * Twitter Bootstrap 2.1 (`documentation <http://twitter.github.com/bootstrap/>`_) and its JS plugins
    * Form Validation: `Backbone Validation`_
    * Parameters support on view routing: `Backbone Query Parameters`_
    * Datagrid: `Backbone Datagrid`_
    * Paginated lists: `Backbone Paginator`_
    * Asynchronous calls: Async_
    * Dispatching keyboard shortcuts: Keymaster_
    * Get and set relations (one-to-one, one-to-many, many-to-one) for Backbone models: `Backbone Relational`_
    * Parsing, validating, manipulating, and formatting dates: `Moment`_

Before going deeper in the RESThub Backbone stack, you should read the great documentation `Developing Backbone.js Applications <http://addyosmani.github.com/backbone-fundamentals/>`_ by Addy Osmani, it is a great introduction to pure Backbone.js.

Changelog
=========

 * 2012-11-28: RESThub Backbone.js stack 2.0.0 GA has been released!
 * 2012-11-13: RESThub Backbone.js stack 2.0-rc4 has been released
 * 2012-10-24: RESThub Backbone.js stack 2.0-rc3 has been released
 * 2012-10-22: `RESThub Backbone.js stack 2.0-rc2 <https://github.com/resthub/resthub-backbone-stack/issues?milestone=4&state=closed>`_ has been released
 * 2012-10-01: `RESThub 2.0-rc1 <https://github.com/resthub/resthub-backbone-stack/issues?milestone=3&state=closed>`_ has been released
 * 2012-08-29: `RESThub 2.0-beta2 <https://github.com/resthub/resthub-backbone-stack/issues?milestone=1&state=closed>`_ has been released

Bootstrap your project
======================

There are 2 ways to use it in your project:
    * If you are starting a new RESThub Spring + Backbone stack project, the better way to use it is to use one of the Backbone.js webappp Maven Archetypes described `here <spring-stack.html#bootstrap-your-project>`_
    * You can simply download `latest RESThub Backbone.js stack <https://github.com/resthub/resthub-backbone-stack/downloads>`_, and extract it at the root of your webapp

The `Todo RESThub 2.0 example <https://github.com/resthub/todo-backbone-example>`_ project is the reference example project using this stack.

Tutorial
========

You should follow `RESThub Backbone Stack tutorial <tutorial/backbone.html>`_  in order to learn step by step how to use it.

Project layout
==============

Directories and filename conventions
------------------------------------

Here is the typical RESThub Backbone.js stack based application directories and filename layout:

.. code-block:: text

    /
    ├── img
    ├── css
    │   ├── style.css
    │   ├── bootstrap.css
    │   ├── bootstrap-responsive.css
    ├── template
    │   ├── project
    │   │   ├── projects.hbs
    │   │   └── project-edit.hbs
    │   └── user
    │       ├── users.hbs
    │       └── user-edit.hbs
    ├── js
    │   ├── lib
    │   │   ├── async.js
    │   │   ├── backbone.js
    │   │   ├── ...
    │   │   └── resthub
    │   │       ├── backbone-resthub.js
    │   │       ├── backbone-validation-ext.js
    │   │       └── ...
    │   ├── model
    │   │   ├── user.js 				var User = Backbone.Model.extend(...); return User;
    │   │   └── project.js 				var Project = Backbone.Model.extend(...); return Project;
    │   ├── collection
    │   │   ├── users.js 				var Users = Backbone.Collection.extend(...); return Users;
    │   │   └── projects.js 				var Projects = Backbone.Collection.extend(...); return Projects;
    │   ├── view
    │   │   ├── project
    │   │   │   ├── projects-view.js 			var ProjectsView = Resthub.View.extend(...); return ProjectsView;
    │   │   │   └── project-edit-view.js 		var ProjectEditView = Resthub.View.extend(...); return ProjectEditView;
    │   │   └── user
    │   │       ├── users-view.js 			var UsersView = Resthub.View.extend(...); return UsersView;
    │   │       └── user-edit-view.js 			var UserEditView = Resthub.View.extend(...); return UserEditView;
    │   ├── router
    │   │   └── app-router.js 				var AppRouter = Backbone.Router.extend(...); return AppRouter;               
    │   ├── app.js
    │   └── main.js
    └── index.html

index.html
----------

index.html is provided by RESThub Backbone stack, so you don't have to create it.

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
        <script data-main="js/main" src="js/lib/require.js"></script>
      </body>
    </html>

main.js
-------

This application bootstrap file is main.js located at your webapp root (usually src/main/webapp). The goal of this file is mainly to intialize require.js configuration. Your application code should not be here but in app.js (automatically loaded by main.js) in order to allow easy Backbone stack updates.

Here's the default main.js file:

.. code-block:: javascript

    require.config({

        shim: {
            'underscore': {
                exports: '_'
            },
            'underscore-string': {
                deps: [
                    'underscore'
                ]
            },
            'handlebars-orig': {
                exports: 'Handlebars'
            },
            'backbone': {
                deps: [
                    'underscore',
                    'underscore-string',
                    'jquery'
                ],
                exports: 'Backbone'
            },
            'backbone-queryparams': {
                deps: [
                    'backbone'
                ]
            },
            'backbone-datagrid': {
                deps: [
                    'backbone'
                ],
                exports: 'Backbone.Datagrid'
            },
            'backbone-paginator': {
                deps: [
                    'backbone'
                ],
                exports: 'Backbone.Paginator'
            },
            'bootstrap': {
                deps: [
                    'jquery'
                ]
            },
            'backbone-relational': {
                deps: [
                    'backbone'
                ]
            },
            'keymaster': {
                exports: 'key'
            },
            'async': {
                exports: 'async'
            }
        },

        // Libraries
        paths: {
            jquery: 'lib/jquery',
            underscore: 'lib/underscore',
            'underscore-string': 'lib/underscore-string',
            'backbone-orig': 'lib/backbone',
            backbone: 'lib/resthub/backbone-resthub',
            localstorage: 'lib/localstorage',
            text: 'lib/text',
            i18n: 'lib/i18n',
            pubsub: 'lib/resthub/pubsub',
            'bootstrap': 'lib/bootstrap',
            'backbone-validation-orig': 'lib/backbone-validation',
            'backbone-validation': 'lib/resthub/backbone-validation-ext',
            'handlebars-orig': 'lib/handlebars',
            'handlebars': 'lib/resthub/handlebars-helpers',
            'backbone-queryparams': 'lib/backbone-queryparams',
            'backbone-datagrid': 'lib/backbone-datagrid',
            'backbone-paginator': 'lib/backbone-paginator',
            'backbone-relational': 'lib/backbone-relational',
            async: 'lib/async',
            keymaster: 'lib/keymaster',
            hbs: 'lib/resthub/require-handlebars',
            'moment': 'lib/moment',
            template: '../template'
        }
    });

    // Load our app module and pass it to our definition function
    require(['app']);

**shim** config is part of `Require 2.0`_ and allows to `Configure the dependencies and exports for older, traditional "browser globals" scripts that do not use define() to declare the dependencies and set a module value`. See `<http://requirejs.org/docs/api.html#config-shim>`_ for more details.

**path** config is also part of Require_ and allows to define paths for libs not found directly under baseUrl. 
  See `<http://requirejs.org/docs/api.html#config-paths>`_ for details.

RESThub suggests to **preload some libs** that will be used for sure as soon the app starts (dependencies required by Backbone itself and our template engine). This mechanism also allows us to load other linked libs transparently without having to define it repeatedly (e.g. ``underscore.string`` loading - this libs is strongly correlated to ``underscore`` - and merged with it and thus should not have to be defined anymore)

app.js
-------

app.js is where your application begins. You should customize it in order to initialize your routers and/or views.

Here's the default app.js file:

.. code-block:: javascript

    define(['router/app-router'], function(AppRouter) {
        new AppRouter();
        // ...
    });

Resthub.View
============

RESThub Backbone stack provides an enhanced Backbone View named Resthub.View with the following functionalities:
 * Default render() with root and context attributes
 * Automatic view dispose + callbacks unbind when a view is removed from DOM
 * View model population from a form

Default render() with root and context attributes
-------------------------------------------------

Backbone views contain an $el attribute that represents the element (a div by default) where the template will be rendered, but it does not provide an attribute that represents the DOM element in which the view will be attached.

In order to follow separation of concerns and encapsulation principles, RESThub Backbone stack manages a $root element in which the view will be attached. You should always pass it as constructor parameter, so as to avoid hardcoding view root elements. Like el, model or collection, it will be automatically as view attributes.

.. code-block:: javascript

    new MyView({root: this.$('.container'), collection: myCollection});

In this example, we create the MyView view and attach it to the .container DOM element of the parent view. You can also pass a String selector parameter.

.. code-block:: javascript

    new MyView({root: '#container', collection: myCollection});

RESThub provides a default implementation that will render your template with **model**, **collection** and **labels** as template attributes context if these properties are defined.

.. code-block:: javascript

    define(['underscore', 'backbone', 'hbs!template/my'], function(_, Backbone, myTemplate){
        var MyView = Resthub.View.extend({
            
            template: myTemplate,
            
            initialize: function() {
                _.bind(this.render, this);
                this.collection.on('reset', this.render, this);
            }

        });
    });

A sample template with automatic collection provisionning:

.. code-block:: html

    <ul>
      {{#each collection}}
      <li>{{this.firstname}} {{this.name}}</li>
      {{/each}}
    </ul>

Or with automatic model and labels provisionning:

.. code-block:: html

    <p>{{labels.user.identity}}: {{model.firstname}} {{model.name}}</li>    

After instantiation, ``this.$root`` contains a cached jQuery element and ``this.root`` the DOM element. By default, when render() is called, Backbone stack empties the root element, and adds el to the root as a child element. You can change this behaviour with the strategy parameter that could have following values:
 * replace: replace the content of $root with $el view content
 * append: append the content of $el at the end of $root
 * prepend: prepend the content of $el at the beginning of $root

.. code-block:: javascript

    var MyView = Resthub.View.extend({
            
        template: myTemplate,
        tagName:  'li',
        strategy: 'append'
        
    });

You can customize the rendering context by defining a context property:

.. code-block:: javascript

    var MyView = Resthub.View.extend({
            
        template: myTemplate,

        context: {
            numberOfElemnts: 42,
            collection: this.collection
        }
       
    });

Or by passing a function if you need dynamic context:

.. code-block:: javascript

    var MyView = Resthub.View.extend({
            
        template: myTemplate,
        labels: myLabels,
        
        context: function() {
            var done = this.collection.done().length;
            var remaining = this.collection.remaining().length;
            return {
                total:      this.collection.length,
                done:       done,
                remaining:  remaining,
                labels:   this.labels
            };
    });

Or by passing the context as a render parameter when you call it explicitely:

.. code-block:: javascript

    this.render({messages: messages, collection: this.collection});

If you need to customize the render() function, you can replace or extend it. Here is an example about how to extend it. This sample calls the default render method and adds children elements:

.. code-block:: javascript

    var MyView = Resthub.View.extend({

        render: function() {
            // Call super render function with the same arguments
            MyView.__super__.render.apply(this, arguments);
            // Add child views
            this.collection.each(function(child) {
                this.add(child);
            }, this);
        },
        add: function(todo) {
            var childView = new ChildView({
                model: child,
                root: this.$('.childcontainer')
            });
        }

    });

.. _backbone-dispose:
    
Automatic view dispose + callbacks unbind
-----------------------------------------

``Resthub.View`` now includes a ``dispose`` function (antipated from Backbone.js master) that cleans all view, model and collection bindings to properly clean up a view. This method is called by another View method ``remove`` that also performs a jquery ``view.el`` DOM remove.

RESThub provides three extensions related to this functionnality:

1- ``dispose`` extension to automatically unbind ``Backbone.Validation``: when removing a view and, if ``Backbone.Validation`` is defined, you also had to unbind validation events that call ``validate``, ``preValidate`` and ``isValid`` methods. **This is now automatically done for you by RESThub** in ``dispose``.
   
2- Addition of an ``onDispose()`` method called on top of ``dispose``: this method is empty by default but can be implemented to perform some additional actions (unbind, etc.) immediately before the effective view disposal. You simply have to define such a method in your views:

.. code-block:: javascript

	onDispose: function() {
		// do something
	}

3- Automatic bind ``dispose`` call on element remove event: the ``dispose`` method previously described is called by the ``remove`` Backbone_ view method. But this method still has to be manually called by users (for instance in your router).
   
RESThub offers an extension to this mechanism that listens on any removal in the ``view.el`` DOM element and **automatically calls dispose on remove**. This means that you don't have to manage this workflow anymore and any replacement done in el parent will trigger a dispose call.
   
i.e.: each time a jQuery ``.html(something)``, ``.remove()`` or ``.empty()`` is performed on view el parent or each time a ``remove()`` is done on the el itself, **the view will be properly destroyed**.

View model population from a form
---------------------------------

`Backbone Validation`_ provides some helpers to validate a model against constraints. Backbone_ defines some methods (such as ``save``) to validate a model and then save it on the server. But neither `Backbone Validation`_ nor Backbone_ allow to fill a model stored in a view with form values. 

RESThub comes with a really simple ``Backbone.View`` extension that copies each input field of a given form in a model. This helper is a new View method called ``populateModel()``. This function has to be explicitely called (e.g. before a ``save()``):

.. code-block:: javascript

   Resthub.View.extend({

      ...
   
      saveUser:function () {
         this.populateModel();

          // save model if it's valid, display alert otherwise
          if (this.model.isValid()) {
              this.model.save(null, {
                  success:this.onSaveSuccess.bind(this),
                  error:this.onSaveError.bind(this)
              });
          }   
       }
   });
   
``populateModel`` searches for the form element provided and copies each form input value into the given model (matching the form input name to an model attribute name). API is: 

.. code-block:: javascript

   /** utility method providing a default and basic handler that
    * populates model from a form input
    *
    * @param form form element to 'parse'. Form parameter could be a css selector or a
    * jQuery element. If undefined, the first form of this view el is used.
    * @param model model instance to populate. If no model instance is provided,
    * search for 'this.model'
   **/
   populateModel:function (form, model);
   
So you can use it in multiple ways from your view: 

.. code-block:: javascript

   // take the first el form element and copy values into 'this.model' instance
   this.populateModel();
   
   // get the form element matching the provided selector (form with id "myForm") and copy values into 'this.model' instance
   this.populateModel("#myForm");
   
   // get the provided jquery form element and copy values into 'this.model' instance
   this.populateModel(this.$("#myForm");
   
   // take the first el form element and copy values into provided myModel instance
   this.populateModel(null, myModel);
   
   // get the form element matching the provided selector (form with id "myForm") and copy values into provided myModel instance
   this.populateModel("#myForm", myModel);
   
   // get the provided jquery form element and copy values into provided myModel instance
   this.populateModel(this.$("#myForm"), myModel);

As said before, this approach could appear naive but will probably fit your needs in most cases. If not, you are free not to use this helper, to extend this method, globally or locally with your own logic or to use a third party lib to bind model and form (see `Backbone.ModelBinder <http://github.com/theironcook/Backbone.ModelBinder>`_ or `Rivets.js <http://rivetsjs.com/>`_ for instance).

.. _templating:

Templating
==========

Handlebars
----------

Client-side templating capabilities are based by default on Handlebars_.

Templates are HTML fragments, without the <html>, <header> or <body> tag:

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

RequireJS Handlebars plugin
---------------------------

Templates are injected into Views by the RequireJS Handlebars plugin, based on RequireJS text plugin. This hbs plugin will automatically **retrieve and compile** your template. So it should be defined in your main.js:

.. code-block:: javascript

    require.config({
        paths: {
            // ...
            text: 'lib/text',
            hbs: 'resthub/handlebars-require'
        }
    });

Sample usage in a Backbone.js View:

.. code-block:: javascript

    define(['jquery', 'backbone', 'hbs!template/todo'],function($, Backbone, todoTmpl) {
        var TodoView = Resthub.View.extend({

        //... is a list tag.
        tagName:  'li',
        
        // Resthub.View will automtically Handlebars template with model or collection set in the context
        template: todoTmpl;

    });
        
Helpers
-------

Resthub provide some usefull **Handlebars helpers** included by default:

ifinline
++++++++

This helper provides a more fluent syntax for inline ifs, i.e. if embedded in quoted strings.

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

.. _sprintf-helper:
   
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

modulo
++++++++

This helper provides a modulo function.

If (n % m) equals 0 then the block is rendered, and if not, the else block is rendered if provided.

e.g:

.. code-block:: html

   {{#modulo index 2}}
      <span>{{index}} is even</span>
   {{else}}
      <span>{{index}} is odd</span>
   {{/modulo}}

with the following context:

.. code-block:: javascript

   {index:10}
   
will produce:

.. code-block:: html

   <span>10 is even</span>

formatDate
++++++++++

This helper provides a date formatting tool.
The date will be parsed with the inputPattern and then formatted with the outputPattern.

Parameters are:

 - date: the date to parse and format
 - outputPattern: the pattern used to display the date (optional)
 - inputPattern: the pattern used to parse the date (optional)

inputPattern and outputPattern are optionals: the default pattern is 'YYYY-MM-DD HH:mm:ss'

Full documentation about date format can be found `here <http://momentjs.com/docs/#/displaying/format/>`_.

e.g:

.. code-block:: html

   <span>{{formatDate myDate pattern}}</span>

with the following context:

.. code-block:: javascript

   { myDate: new Date(), pattern: '[today] MM/DD/YYYY' }
   
will produce:

.. code-block:: html

   <span>today 10/24/2012</span>

and:

.. code-block:: html

   <span>{{formatDate myDate outputPattern inputPattern}}</span>

with the following context:

.. code-block:: javascript

   { myDate: '2012/17/02 11h32', inputPattern: 'YYYY/DD/MM HH\\hmm', outputPattern: 'HH:mm, MM-DD-YYYY' }
   
will produce:

.. code-block:: html

   <span>11:32, 02-17-2012</span>


.. _backbone-pushstate:
   
Backbone effective pushState extension
======================================

Backbone_ allows ``pushState`` activation that permits usage of real URLs instead of `#` anchors.
PushState offers a better navigation experience, better indexation and search engine ranking:

.. code-block:: javascript

   Backbone.history.start({pushState:true, root:"/"});


The `root` option defines the path context of our Backbone_ application;

However, Backbone_ stops here. Direct access to views by URL works fine but, each link leads to
**a full reload**! Backbone_ does not intercept html links events and it is necessary to implement it ourselves.

Branyen Tim, the creator of `Backbone boilerplate <http://github.com/tbranyen/backbone-boilerplate>`_ shares the following solution that RESThub integrates in its extensions with an additional test to check pushState activation.

If ``Backbone.history`` is started with the ``pushState`` option, **any click on a link will be intercepted and bound to a Backbone navigation instead**. If you want to provide **external links**, you only have to use the ``data-bypass`` attribute:

.. code-block:: html

   <a data-bypass href="http://github.com/bmeurant/tournament-front" target="_blank">

.. _backbone-form-helper:
    
Internationalization
====================

You should never use directly labels or texts in your source files. All labels should be externalized in order to prepare your 
application for internationalization. Doing such thing is pretty simple with RESThub Backbone.js stack thanks to `requireJS i18n plugin <http://requirejs.org/docs/api.html#i18n>`_.

Please find below the steps needed to internationalize your application.

1. **Configure i18n plugin**

In your main.js file you should define a shortcut path for i18n plugin and the default language for your application:

.. code-block:: javascript

    require.config({
        paths: {
            // ...
            i18n: "lib/i18n"
        },
        locale: localStorage.getItem('locale') || 'en-us'
    });


2. **Define labels**

Create a labels.js file in the js/nls directory, it will contain labels in the default locale used by your application. You can change labels.js to another name (messages.js or functionality related name like user.js or product.js), but js/nls is the default location.

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

Add translations in subfolders named with the locale, for instance js/nls/fr-fr ...
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

3. **Use it**

Add a dependency in the js, typically a View, where you'll need labels. You'll absolutely need to give a scoped variable to the result (in this example ``myLabels``, but you can choose the one you want). 

Prepending 'i18n!' before the file path in the dependency indicates RequireJS to get the file related to the current locale:

.. code-block:: javascript

    define(['i18n!nls/labels'], function(myLabels) {
        // ...

        labels: myLabels,

        // ...
    });

In your html template:

.. code-block:: html

    <div class="title">
        <h1><%= labels.titles.login %></h1>
    </div>

4. **Change locale**

Changing locale require a page reloading, so it is usually implemented with a Backbone.js router configuration like the following one:

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

5. **sprintf to the rescue**

Internalionalization can sometimes be tricky since words are not always in the same order depending on the language. To make your life easier, RESThub backbone stack includes Underscore.String. It contains a sprintf function that you can use for your translations.

You can use the ``_.sprintf()`` function and the ``sprintf`` helper to have substitutions in your labels.

labels.js

.. code-block:: javascript

    'root': {
        'clearitem': "Clear the completed item",
        'clearitems': 'Clear %s completed items',
    }

RESThub also provides a ``sprintf`` handlebars helper to use directly in your templates (cf. :ref:`sprintf-helper`):

.. code-block:: html

    {{#ifequals done 1}} {{messages.clearitem}} {{else}} {{sprintf messages.clearitems done}} {{/ifequals}}

Logging
=======

RESThub Backbone stack include a console.js implementation responsible for 
 * Creating console.* functions if they do not exists (old IE versions)
 * Optionnaly sending logs to the server, in order to make JS error tracking and debugging easier

 In order to send logs to the server, import console.js in your main.js (already done by default):

.. code-block:: javascript

    // Load our app module and pass it to our definition function
    require(['console', 'app']);

In your app.js, you can define different console.level values, which define what log level will be sent to the server:

.. code-block:: javascript

    console.level = 'off'; // Default, no log are sent to the server
    console.level = 'debug'; // debug, info, warn and error logs are sent to the server
    console.level = 'info'; // info, warn and error logs are sent to the server
    console.level = 'warn'; // warn and error logs are sent to the server
    console.level = 'error'; // error logs are sent to the server

Javascript syntax error are also sent to the server with an error log level.

You can customize the log server url:

.. code-block:: javascript
    
    console.serverUrl = 'api/log'; // Default value

Log are sent thanks a POST request with the following JSON body:

.. code-block:: javascript
    
    {"level":"warn","message":"log message","time":"2012-11-13T08:18:52.972Z"}

RESThub web server provide a builtin implementation of the serverside logging webservice, see the `related documentation <spring-stack.html#client-logging>`_ for more details.

.. _pubsub:
    
Publish Subscribe
=================

RESThub provides a publish / subscribe mechanism in your application with a tiny native ``Backbone.Events`` extension.
Publishing and subscribing are globally scoped and allow to communicate between views within your app.

API
---

``Backbone.Events`` API was not modified: `documentation <http://backbonejs.org/#Events>`_

.. code-block:: javascript
 
   // Bind one or more space separated events, `events`, to a `callback`
   // function. Passing `"all"` will bind the callback to all events fired.
   on: function(events, callback, context);

   // Remove one or many callbacks. If `context` is null, removes all callbacks
   // with that function. If `callback` is null, removes all callbacks for the
   // event. If `events` is null, removes all bound callbacks for all events.
   off: function(events, callback, context);

   // Trigger one or many events, firing all bound callbacks. Callbacks are
   // passed the same arguments as `trigger` is, apart from the event name
   // (unless you're listening on `"all"`, which will cause your callback to
   // receive the true name of the event as the first argument).
   trigger: function(events);

.. _pubsub-usage:   
   
Usage
-----

PubSub component can be accessed globally but we strongly recommend to import it with Require_.

.. code-block:: javascript

   define(['pubsub'], function(Pubsub) {
        
      ...
        
      // subscribe to one event (do not forget the context:this)
      Pubsub.on("!test-event", function () { ... }, this);

      // subscribe to multiple events
      Pubsub.on("!test-event !test-event2", function () { ... }, this);

      // trigger one event
      Pubsub.trigger("!test-event");

      // trigger multiple events
      Pubsub.trigger("!test-event !test-event2");

      // unsubscribe from one event
      Pubsub.off("!test-event");

      // unsubscribe from multiple events
      Pubsub.off("!test-event !test-event2");

      // unsubscribe from all
      Pubsub.off();
        
      ...
        
   }

Because of ``Bacbone.ResthubView`` and PubSub integration mechanisms (see below), the ``!`` prefix for any global PubSub event is **strongly recommended**. 

.. warning::

   Not following this convention does not have any impact on PubSub behaviour but prevents usage of integrated Resthub.View PubSub events declaration (see below)

.. _pubsub-in-views:
   
PubSub and Backbone Views integration
-------------------------------------

In order to facilitate global PubSub events in Backbone Views, RESThub provides some syntactic sugar in ``Resthub.View``.

Backbone Views events hash parsing has been extended to be capable of declaring global PubSub events as it is already done for DOM events binding. To declare such global events in your Backbone View, you only have to add it in events hash:

.. code-block:: javascript

   events:{
       // regular DOM event bindings
       "click #btn1":"buttonClicked",
       "click #btn2":"buttonClicked",
       // global PubSub events
       "!global":"globalFired",
       "!global1":"globalFired",
       "!globalParams":"globalFiredParams"
   },
    
Please note that it is mandatory to prefix your global events with ``!`` to differenciate them from DOM events. You will always have to use the ``!`` prefix to reference events later (see :ref:`pubsub-usage` for samples).

With this mechanism, PubSub subscribings are automatically declared on View construction, as DOM Events: **You don't have to call PubSub.on on these declared events**.
In the same way, PubSub subscribings for this View are automatically removed during a Backbone ``dispose()`` method call: **You don't have either to call PubSub.off on these declared events**.

Obviously, it is still possible for you to explicitely call ``on`` and ``off`` in your view on other global events that you don't want to or you can't declare on events hash (e.g. for more dynamic needs). But don't forget to bind ``this` when declaring subscription:

.. code-block:: javascript

   PubSub.on("!event", function () {...}, this);

Other librairies included in the stack
======================================

Backbone Validation
-------------------

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

**Model**: constraints definition:

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

**HTML5 Form**:

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


**View**: initialization and usage:

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

You also natively beneficate of custom validation callbacks allowing to render validation errors in a form structured with `Twitter Bootstrap`_.

Backbone Query Parameters
-------------------------

Backbone_ routes management allows to define permet such routes:
``"participants":"listParticipants"`` and ``"participants?:param":"listParticipantsParameters"``. But the native 
behaviour seems not sufficient:

- **management of an unknown number of parameters** (ex ``?page=2&filter=filter``) is not obvious
- we have to define (at least) **two routes to handle calls with or without parameters** without duplication
and without too much technical code

Expected behaviour was that the **map a single route to a method with an array of request parameter as optional parameter.**

`Backbone Query Parameters`_ provides this functionality.

With this lib, included once and for all in the main router, You 'll get the following:

**router.js**:

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

**list.js**:

.. code-block:: javascript

   askedPage:1,

   initialize:function (params) {

       ...

       if (params) {
           if (params.page && this.isValidPageNumber(params.page)) this.askedPage = parseInt(params.page);
       }

       ..
   },

Backbone Datagrid
-----------------

`Backbone Datagrid`_ is a powerful component, based on Backbone.View, that
displays your Bakbone collections in a dynamic datagrid table. It is highly
customizable and configurable with sensible defaults.

You will find the full documentation on its `dedicated website
<http://loicfrering.github.com/backbone.datagrid/>`_. Do not miss the examples
listed on `this page <http://loicfrering.github.com/backbone.datagrid/examples/>`_. Their sources are
available in the `examples <https://github.com/loicfrering/backbone.datagrid/tree/master/examples/>`_
directory of the repository.

* Solar: a simple and complete example with an in memory collection of planets from the
  Solar System.

  * `Live version <http://loicfrering.github.com/backbone.datagrid/examples/solar.html>`_
  * `Sources <https://github.com/loicfrering/backbone.datagrid/tree/master/examples/js/solar.js>`_

* GitHub: an example with a collection connected to GitHub's REST API.

  * `Live version <http://loicfrering.github.com/backbone.datagrid/examples/github.html>`_
  * `Sources <https://github.com/loicfrering/backbone.datagrid/tree/master/examples/js/github.js>`_

Note that the Backbone Datagrid handles pagination by itself and does not rely
on Backbone Paginator which is described below and should only be used to
paginate collections which are not displayed in a datagrid.

Backbone Paginator
------------------

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

Finally, we change server call: this time the ``goTo`` method extend ``fetch`` and should be called instead
(``views/participants/list.js``):

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


Async
-----

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
       // been performed: we can manage global behaviours
       if (nbWaitingCallbacks == 0) {
           // do something
       }
   },


This code works but there is **too much technical code**!

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
    * @param results array of fetched models: contain null value in cas of error
    */
   afterRemove:function (err, results) {

       // no more test
       ...
   },

Keymaster
---------

Keymaster_ is a micro library allowing to define listeners on keyboard shortcuts and propagate them. 
The syntax is elegant, it is very simple while very complete:

- Management of multiple hotkeys
- Chaining through an important number of "modifiers"
- Source DOM element type filtering
- ...

It is so simple that the doc is self sufficient - see `here <http://github.com/madrobby/keymaster>`_

Backbone Relational
-------------------

`Backbone Relational`_ provides one-to-one, one-to-many and many-to-one relations between models for Backbone. To use relations, extend Backbone.RelationalModel (instead of the regular Backbone.Model) and define a property relations, containing an array of option objects. Each relation must define (as a minimum) the type, key and relatedModel. Available relation types are Backbone.HasOne and Backbone.HasMany.

Backbone-relational features:
 * Bidirectional relations, which notify related models of changes through events.
 * Control how relations are serialized using the includeInJSON option.
 * Automatically convert nested objects in a model's attributes into Model instances using the createModels option.
 * Lazily retrieve (a set of) related models through the fetchRelated(key<string>, [options<object>], update<bool>) method.
 * Determine the type of HasMany collections with collectionType.
 * Bind new events to a Backbone.RelationalModel for:
 * addition to a HasMany relation (bind to add:<key>; arguments: (addedModel, relatedCollection)),
 * removal from a HasMany relation (bind to remove:<key>; arguments: (removedModel, relatedCollection)),
 * reset of a HasMany relation (bind to reset:<key>; arguments: (relatedCollection)),
 * changes to the key itself on HasMany and HasOne relations (bind to update:<key>; arguments=(model, relatedModel/relatedCollection)). 

Moment
------

`Moment`_ is a date library for parsing, validating, manipulating, and formatting dates.

Moment.js features:
 * Parse and format date with custom pattern and internationalization
 * Date manipulation (add, substract)
 * Durations (eg: 2 hours)

Guidelines
==========

Collection View
---------------

If you need to render a simple list of elements, just make a single view with an each loop in the template:

.. code-block:: html

    <h1>My TodoList</h1>
    <ul>
      {{#each this}}
        <li>{{title}}</li>
      {{/each}}
    </ul>

But if each element of your collection requires a separate view (typically when you listen on some events on it or if it contains a form), in order to comply with separation of concerns and encapsulation principles, you should create separate views for the collection and the model. The model view should be able to render itself.

You can see more details on the `Todo example <http://github.com/resthub/todo-example>`_ (have a look to TodosView and TodoView).

Always specify the context for event binding
--------------------------------------------

In order to allow automatic cleanup when the View is removed, you should always specify the context when binding models or collection events:

.. code-block:: javascript
    
    // BAD: no context specified - event bindings won't be cleaned when the view is removed
    Todos.on('all', this.render);

    // GOOD: context will allow automatic cleanup when the view is removed
    Todos.on('all', this.render, this);

You should also specify the model or collection attribute of your View to make it work.

Static versus instance variables
-------------------------------

If you want to create different View instances, you have to manage properly the DOM element where the view will be attached as described previously. You also have to use instance variables.

Backbone way of declaring a static color variable:

.. code-block:: javascript

    var MyView = Resthub.View.extend({

        color: '#FF0000',

        initialize: function(options) {
            this.$root = options.root;
            this.$root.html(this.$el);
        }
           
    });
    return MyView;

Backbone way of declaring an instance color variable:

.. code-block:: javascript

    var MyView = Resthub.View.extend({

        initialize: function(options) {
            this.$root = options.root;
            this.$root.html(this.$el);

            this.color = '#FF0000';
        }
           
    });
    return MyView;

Use this.$() selector
---------------------

this.$() is a shortcut for this.$el.find(). You should use it for all your view DOM selector code in order to find elements within your view (i.e. not in the whole page). It follows the encapsulation pattern, and will make it possible to have several instances of your view on the same page. Even with a singleton view, it is a good practice to use this pattern.

Events
------

Backbone default event list is available `here <http://backbonejs.org/#FAQ-events>`_.

Inheritance
-----------

As described by `k33g <https://twitter.com/#!/k33g_org>`_ on his `Gist Use Object Model of BackBone <https://gist.github.com/2287018>`_, it is possible to reuse Backbone.js extend() function in order to get simple inheritance in Javascript.

.. code-block:: javascript

    // Define an example Kind class
    var Kind = function() {
        this.initialize && this.initialize.apply(this, arguments);
    };
    Kind.extend = Backbone.Model.extend;

    // Create a Human class by extending Kind
    var Human = Kind.extend({
        toString: function() { console.log("hello: ", this); },
        initialize: function (name) {
            console.log("human constructor");
            this.name = name
        }
    });

    // Call parent constructor
    var SomeOne = Human.extend({
        initialize: function(name){
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
        toString: function() { console.log("hello: ", this); },
        initialize: function (name) {
            console.log("human constructor");
            this.name = name
        }
    },{ //Static
        counter: 0,
        getCounter: function() { return this.counter; }
    });

Cache buster
------------

In order to avoid caching issues when updating your JS or HTML files, you should use the `urlArgs RequireJS attribute <http://requirejs.org/docs/api.html#config>`_. You can filter the ${buildNumber} with your build tool at each build.


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
        urlArgs: 'appversion=738792920293847'
    });


    
.. _Require 2.0: http://requirejs.org
.. _Require: http://requirejs.org
.. _Handlebars: http://handlebarsjs.com
.. _Backbone Validation: http://github.com/thedersen/backbone.validation
.. _Twitter Bootstrap: http://twitter.github.com/bootstrap/
.. _Backbone Datagrid: http://loicfrering.github.com/backbone.datagrid/
.. _Backbone Paginator: http://addyosmani.github.com/backbone.paginator/
.. _Backbone Query Parameters: http://github.com/jhudson8/backbone-query-parameters
.. _Async: http://github.com/caolan/async/
.. _Keymaster: http://github.com/madrobby/keymaster
.. _Backbone: http://backbonejs.org/
.. _Backbone Relational: https://github.com/PaulUithol/Backbone-relational
.. _Moment: http://momentjs.com/
