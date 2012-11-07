:tocdepth: 2

=================
Backbone.js Stack
=================

RESThub 2 backbone stack provides a client-side full stack and guidelines for building enterprise grade HTML5 applications. It could be used with any server backend : Ruby, PHP, NodeJS, J2EE, Spring, Grail ...

.. contents::
   :depth: 3
   
The Backbone.js stack includes the following main librairies :
    * **jQuery 1.7** (`documentation <http://docs.jquery.com/Main_Page>`_)
    * **Backbone.js 0.9.2** (`documentation <http://documentcloud.github.com/backbone/>`_) and its `localstorage adapter 
      <http://documentcloud.github.com/backbone/docs/backbone-localstorage.html>`_
    * **Underscore.js 1.3.3** (`documentation <http://documentcloud.github.com/underscore/>`_)
    * **Underscore.String 2.0.0** (`documentation <https://github.com/epeli/underscore.string#readme>`_)
    * **Require.js 2.0** with `i18n <http://requirejs.org/docs/api.html#i18n>`_ and `text <http://requirejs.org/docs/api.html#text>`_ plugins 
      (`documentation <http://requirejs.org/docs/api.html>`_)
    * **Handlebars 1.0** (`documentation <http://handlebarsjs.com>`_)
    * A **console shim** for browsers that don't support it
    * **Twitter Bootstrap 2.1** (`documentation <http://twitter.github.com/bootstrap/>`_) JS plugins
    
And some additional libraries documented here: :ref:`complementary-libs`

Bootstrap your project
======================

There are 2 ways to use it in your project :
    * If you are starting a new RESThub Spring + Backbone stack project, the better way to use it is to use one of the Backbone.js webappp Maven Archetypes described `here <spring-stack.html#bootstrap-your-project>`_
    * You can simply go to the `RESThub Backbone.js stack GitHub repository <https://github.com/resthub/resthub-backbone-stack>`_, and download the repository content and copy it at the root of your webapp

The `Todo RESThub 2.0 example <http://github.com/resthub/todo-example>`_ project is the reference example project using this stack.

Tutorial
========

You should follow `RESThub Backbone Stack tutorial <tutorial/backbone.html>`_ (solution provided at the end) in order to learn step by step how to use it.

Project layout
==============

Directories and filename conventions
------------------------------------

.. code-block:: text

    /
    ├── img
    ├── css
    │   ├── style.css
    │   ├── bootstrap.css
    │   ├── bootstrap-responsive.css
    ├── template
    │   ├── project
    │   │   ├── projects.html
    │   │   └── project-edit.html
    │   └── user
    │       ├── users.html
    │       └── user-edit.html
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
    │   │   ├── user.js
    │   │   └── project.js
    │   ├── collection
    │   │   ├── users.js
    │   │   └── projects.js
    │   ├── view
    │   │   ├── project
    │   │   │   ├── projects-view.js
    │   │   │   └── project-edit-view.js
    │   │   └── user
    │   │       ├── users-view.js
    │   │       └── user-edit-view.js
    │   ├── router
    │   │   └── app-router.js
    │   └── main.js
    └── index.html

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
        <script data-main="js/main" src="js/lib/require.js"></script>
      </body>
    </html>


index.html is provided by RESThub Backbone stack, so you don't have to create it. Your application bootstrap file is the main.js located at your webapp root (usually src/main/webapp). Here's the default main.js file :

main.js
-------

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
            'backbone-orig': {
                deps: [
                'underscore',
                'underscore-string',
                'jquery'
                ],
                exports: 'Backbone'
            },
            'backbone-queryparams': {
                deps: [
                'backbone-orig',
                'underscore'
                ]
            },
            'backbone-paginator': {
                deps: [
                'backbone-orig',
                'underscore',
                'jquery'
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
                'backbone-orig',  
                'underscore'  
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
            'backbone-paginator': 'lib/backbone-paginator',
            'backbone-relational': 'lib/backbone-relational',
            async: 'lib/async',
            keymaster: 'lib/keymaster',
            hbs: 'lib/resthub/require-handlebars',
            'moment': 'lib/moment',
            template: '../template'
        }
    });

  require(['router/app-router'], function(AppRouter) {
      new AppRouter();
  });
   
- **shim** config is part of `Require 2.0`_ and allows to `Configure the dependencies and exports for older, traditional "browser globals" scripts that do not use define() to declare the dependencies and set a module value`. See `<http://requirejs.org/docs/api.html#config-shim>`_ for more details.
- **path** config is also part of Require_ and allows to define paths for libs not found directly under baseUrl. 
  See `<http://requirejs.org/docs/api.html#config-paths>`_ for details.
- RESThub suggests to **preload some libs** that will be used for sure as soon the app starts (dependencies required by Backbone itself and our template engine). This mechanism also allows us to load other linked libs transparently without having to define it repeatedly (e.g. ``underscore.string`` loading - this libs is strongly correlated to ``underscore`` - and merged with it and thus should not have to be defined anymore)

Backbone.ResthubView
====================

RESThub Backbone stack provides an enhanced Backbone View named Backbone.ResthubView with the following functionalities :
 * Default rendering implementation
 * $root attribute used to specify the container root element where the view should be attached (since $el is the view itself)
 * Default template attribute with context management

$root attribute
---------------

Backbone views contain an $el attribute that represents the element (a div by default) where the template will be rendered, but it does not provide an attribute that represents the DOM element in which the view will be attached.

In order to follow separation of concerns and encapsulation principles, RESThub Backbone stack manages a $root element in which the view will be attached. You should always pass it as constructor parameter, so as to avoid hardcoding view root elements. Like el, model or collection, it will be automatically as view attributes.

.. code-block:: javascript

    new MyView({root: this.$('.container'), collection: myCollection});

In this example, we create the MyView view and attach it to the .container DOM element of the parent view. You can also pass a String selector parameter.

.. code-block:: javascript

    new MyView({root: '#container', collection: myCollection});

RESThub provides a default implementation that will render your template with model, collection and labels in context if these properties are defined.

.. code-block:: javascript

    define(['underscore', 'backbone', 'hbs!template/my'], function(_, Backbone, myTemplate){
        var MyView = Backbone.ResthubView.extend({
            
            template: myTemplate,
            
            initialize: function() {
                _.bind(this.render, this);
                this.collection.on('reset', this.render, this);
            }

        });
    });

A sample template with automatic collection provisionning :

.. code-block:: html

    <ul>
      {{#each collection}}
      <li>{{this.firstname}} {{this.name}}</li>
      {{/each}}
    </ul>

Or with automatic model and labels provisionning :

.. code-block:: html

    <p>{{labels.user.identity}} : {{model.firstname}} {{model.name}}</li>    

After instantiation, ``this.$root`` contains a cached jQuery element and ``this.root`` the DOM element. By default, when render() is called, Backbone stack empties the root element, and adds el to the root as a child element. You can change this behaviour with the strategy parameter that could have following values :
 * replace : replace the content of $root with $el view content
 * append : append the content of $el at the end of $root
 * prepend : prepend the content of $el at the beginning of $root

.. code-block:: javascript

    var MyView = Backbone.ResthubView.extend({
            
        template: myTemplate,
        tagName:  'li',
        strategy: 'append'
        
    });

You can customize the rendering context by defining a context property :

.. code-block:: javascript

    var MyView = Backbone.ResthubView.extend({
            
        template: myTemplate,

        context: {
            messages: messages,
            collection: this.collection
        }
       
    });

Or by passing a function if you need dynamic context :

.. code-block:: javascript

    var MyView = Backbone.ResthubView.extend({
            
        template: myTemplate,
        
        context: function() {
            var done = this.collection.done().length;
            var remaining = this.collection.remaining().length;
            return {
                total:      this.collection.length,
                done:       done,
                remaining:  remaining,
                labels:   labels
            };
    });

Or by passing the context as a render parameter when you call it explicitely :

.. code-block:: javascript

    this.render({messages: messages, collection: this.collection});

If you need to customize the render() function, you can replace or extend it. Here is an example about how to extend it. This sample calls the default render method and adds children elements:

.. code-block:: javascript

    var MyView = Backbone.ResthubView.extend({

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
    
Backbone dispose extension and automatic binding removal
--------------------------------------------------------

``Backone.ResthubView`` now includes a ``dispose`` method that cleans all view, model and collection bindings to properly clean up a view.
This method is called by another View method ``remove`` that also performs a jquery ``view.el`` DOM remove.

RESThub provides three extensions related to this workflow:

1. ``dispose`` extension to automatically unbind ``Backbone.Validation``:

   When removing a view and, if ``Backbone.Validation`` is defined, you also have to unbind validation events that call ``validate``, ``preValidate`` and ``isValid`` methods.
   
   **This is now automatically done for you by RESThub** in ``dispose``.
   
2. Addition of an ``onDispose()`` method called on top of ``dispose``:

   This method is empty by default but can be implemented to perform some additional actions (unbind, etc.) immediately
   before the effective view disposal. You simply have to define such a method in your views:

   .. code-block:: javascript

      onDispose: function() {
         // do something
      }


3. Automatic bind ``dispose`` call on element remove event:

   The ``dispose`` method previously described is called by the ``remove`` Backbone_ view method. But this method still has to be manually called by users (for instance in your router).
   
   RESThub offers an extension to this mechanism that listens on any removal in the ``view.el`` DOM element and **automatically calls dispose on remove**. This means that you don't have to manage this workflow anymore and any replacement done in el parent will trigger a dispose call.
   
   i.e. : each time a jQuery ``.html(something)``, ``.remove()`` or ``.empty()`` is performed on view el parent or each time a ``remove()`` is done on the el itself, **the view will be properly destroyed**.

Automatic population of view model from a form
----------------------------------------------

`Backbone Validation`_ provides some helpers to validate a model against constraints. Backbone_ defines some methods (such as ``save``) to validate a model and then save it on the server. But neither `Backbone Validation`_ nor Backbone_ allow to fill a model stored in a view with form values. 

RESThub comes with a really simple (naive ?) ``Backbone.View`` extension that copies each input field of a given form in a model. This helper is a new View method called ``populateModel()``. This function has to be explicitely called (e.g. before a ``save()``):

.. code-block:: javascript

   Backbone.ResthubView.extend({

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

Client-side templating capabilities are based by default on Handlebars_.

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

Templates are injected into Views by the RequireJS Handlebars plugin, based on RequireJS text plugin. This hbs plugin will automatically **retrieve and compile** your template. So it should be defined in your main.js :

.. code-block:: javascript

    require.config({
        paths: {
            // ...
            text: 'lib/text',
            hbs: 'resthub/handlebars-require'
        }
    });

Sample usage in a Backbone.js View :

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

**Handlebars Helpers** provided by RESThub are documented here: :ref:`handlebars-helpers`

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


    
Avoid caching issues
====================

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
        urlArgs: 'appversion=${738792920293847}'
    });

Internationalization
====================

You should never use directly labels or texts in your source files. All labels should be externalized in order to prepare your 
application for internationalization. Doing such thing is pretty simple with RESThub Backbone.js stack thanks to `requireJS i18n 
plugin <http://requirejs.org/docs/api.html#i18n>`_.

Please find below the steps needed to internationalize your application.

1. **Configure i18n plugin**

In your main.js file you should define a shortcut path for i18n plugin and the default language for your application :

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

Prepending 'i18n!' before the file path in the dependency indicates RequireJS to get the file related to the current locale :

.. code-block:: javascript

    define(['i18n!nls/labels'], function(myLabels) {
        // ...

        labels: myLabels,

        // ...
    });

In your html template :

.. code-block:: html

    <div class="title">
        <h1><%= labels.titles.login %></h1>
    </div>

4. **Change locale**

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

5. **sprintf to the rescue**

Internalionalization can sometimes be tricky since words are not always in the same order depending on the language. To make your life easier, RESThub backbone stack includes Underscore.String. It contains a sprintf function that you can use for your translations.

You can use the ``_.sprintf()`` function and the ``sprintf`` helper to have substitutions in your labels.

labels.js

.. code-block:: javascript

    'root': {
        'clearitem'    : "Clear the completed item",
        'clearitems' : 'Clear %s completed items',
    }

RESThub also provides a ``sprintf`` handlebars helper to use directly in your templates (cf. :ref:`sprintf-helper`):

.. code-block:: html

    {{#ifequals done 1}} {{messages.clearitem}} {{else}} {{sprintf messages.clearitems done}} {{/ifequals}}

Inheritance
===========

As described by `k33g <https://twitter.com/#!/k33g_org>`_ on his `Gist Use Object Model of BackBone <https://gist.github.com/2287018>`_, it is possible to reuse Backbone.js extend() function in order to get simple inheritance in Javascript.

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

.. _pubsub:
    
Publish Subscribe
=================

RESThub provides a publish / subscribe mechanism in your application with a tiny native ``Backbone.Events`` extension.
Publishing and subscribing are globally scoped and allow to communicate between views within your app.

API
---

``Backbone.Events`` API was not modified : `documentation <http://backbonejs.org/#Events>`_

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

   Not following this convention does not have any impact on PubSub behaviour but prevents usage of integrated Backbone.ResthubView PubSub events declaration (see below)

.. _pubsub-in-views:
   
PubSub and Backbone Views integration
-------------------------------------

In order to facilitate global PubSub events in Backbone Views, RESThub provides some syntactic sugar in ``Backbone.ResthubView``.
You'll get to use this extension as soon as you include the RESThub Backbone extension instead of the original Backbone lib (cf. :ref:`resthub-extensions`).

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

With this mechanism, PubSub subscribings are automatically declared on View construction, as DOM Events : **You don't have to call PubSub.on on these declared events**.
In the same way, PubSub subscribings for this View are automatically removed during a Backbone ``dispose()`` method call : **You don't have either to call PubSub.off on these declared events**.

Obviously, it is still possible for you to explicitely call ``on`` and ``off`` in your view on other global events that you don't want to or you can't declare on events hash (e.g. for more dynamic needs). But don't forget to bind ``this` when declaring subscription:

.. code-block:: javascript

   PubSub.on("!event", function () {...}, this);

Guidelines
==========

Collection View
---------------

If you need to render a simple list of elements, just make a single view with an each loop in the template :

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

In order to allow automatic cleanup when the View is removed, you should always specify the context when binding models or collection events :

.. code-block:: javascript
    
    // BAD : no context specified - event bindings won't be cleaned when the view is removed
    Todos.on('all', this.render);

    // GOOD : context will allow automatic cleanup when the view is removed
    Todos.on('all', this.render, this);

You should also specify the model or collection attribute of your View to make it work.

Static versus instance variables
-------------------------------

If you want to create different View instances, you have to manage properly the DOM element where the view will be attached as described previously. You also have to use instance variables.

Backbone way of declaring a static color variable :

.. code-block:: javascript

    var MyView = Backbone.ResthubView.extend({

        color : '#FF0000',

        initialize: function(options) {
            this.$root = options.root;
            this.$root.html(this.$el);
        }
           
    });
    return MyView;

Backbone way of declaring an instance color variable :

.. code-block:: javascript

    var MyView = Backbone.ResthubView.extend({

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