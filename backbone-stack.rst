:tocdepth: 2

===============================
Backbone.js Stack documentation
===============================

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


View instantiation
==================

RESThub Backbone stack provides a default rendering strategy with root element, template and context management.

Backbone views contains an $el attribute that represent the element (a div by default) where the template will be rendered, but it does not provide an attribute that represent the DOM element where the view will be attached.

In order to follow separation of concerns and encapsulation principles, RESThub Backbone stack manage a root element that respresent where the view will be attached. In order to avoid hardcoding view root element, you sould always pass it as constructor parameter. Like el, model or collection, it will be automatically added to the view.

.. code-block:: javascript

    new MyView({root: this.$('.container'), collection: myCollection});

In this example, we create the MyView view and attach it to the .container DOM element of the parent view. You can also pass a String selector parameter.

.. code-block:: javascript

    new MyView({root: '#container', collection: myCollection});

RESThub provides a default render implementation that will render your template with model or collection in the context if these properties are defined.

.. code-block:: javascript

    define(['underscore', 'backbone', 'hbs!templates/my'], function(_, Backbone, myTmpl){
        var MyView = Backbone.View.extend({
            
            template: myTemplate,
            
            initialize: function() {
                _.bind(this.render, this);
                this.collection.on('reset', this.render);
            }

        });
    });

After instantiation, this.$root contains a cached jQuery element and this.root the DOM element. By default, when render() is called, Backbone stack empty root element, and add el to root as a child element. You can change this behaviour thanks to the strategy parameter (could be 'replace', 'append' or 'prepend') :

.. code-block:: javascript

    var MyView = Backbone.View.extend({
            
        template: myTemplate,
        tagName:  'li',
        strategy: 'append'
        
    });

You can customize the rendering context by defining a context property :

.. code-block:: javascript

    var MyView = Backbone.View.extend({
            
        template: myTemplate,
        context: {
            messages: messages,
            collection: this.collection
        }
       
    });

Or by passing the context to the render function :

.. code-block:: javascript

    this.render({messages: messages, collection: this.collection});

If you need to customize render() function, you can replace or extend it. Here is an example about how to extend it. This sample call default render and add some child element:

.. code-block:: javascript

    var MyView = Backbone.View.extend({

        render: function() {
            MyView.__super__.render.apply(this, arguments);
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

Guidelines
==========

Collection View
---------------

If you need to render a simple list of elements, just make a single view with a each loop in the template :

.. code-block:: html

    <h1>My TodoList</h1>
    <ul>
      {{#each this}}
        <li>{{title}}</li>
      {{/each}}
    </ul>

But if each element of your collection is a real view (usually when you listen some event on it or if it is a form), in order to comply with separation of concerns and encapsulation principles, you should create a view for the collection and view for the model. The model view should be able to render itself.

You can see more details on the `Todo example <http://github.com/resthub/todo-example>`_ (have a look to TodosView and TodoView).

Always specify the context for event binding
--------------------------------------------

In order to allow automatic cleanup when the View is removed, you should always specify the context when binding model or collection events :

.. code-block:: javascript
    
    // BAD : no context specified won't clean the view on remove
    Todos.on('all', this.render);

    // GOOD : context will allow automatic cleanup of the handler on remove
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

Use this.$() selector
---------------------

this.$() is a shortcut for this.$el.find(). You should use it for all your view DOM selector code in order to find elements only in your view content, not in the wall page. It allows your to follow encapsulation pattern, and will make it possible to have several instance of your view on the same page. Even with singleton view, it is a good idea to use this pattern.

Events
------

Backbone default event list is available `here <http://backbonejs.org/#FAQ-events>`_.

.. _templating:

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

    define(['jquery', 'backbone', 'hbs!templates/todo'],function($, Backbone, todoTmpl) {
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
        
Helpers
-------

**Handlebars Helpers** provided by resthub are documented here: :ref:`handlebars-helpers`

Complementary libs
==================

**Complementary libs** provided by resthub are documented here: :ref:`complementary-libs`

.. _resthub-extensions:

Resthub extensions
==================

For some of suggested embedded libs, resthub provides extensions.

These extensions can be found, as any other custom resthub lib, in ``js/resthub`` directory.

Resthub provides currently these extensions : 

- Backbone extensions :
   - PubSub events declaration integration mechanism in ``Backbone.Views``: cf. :ref:`pubsub-in-views`.
   - Backbone ``dispose`` method extension and automatic el DOM removing binding: cf. :ref:`backbone-dispose`.
   - Backbone effective pushState extension: cf. :ref:`backbone-pushstate`.
   - Basic view extension to automatically populate model from a form :ref:`backbone-form-helper`.
- Handlebars_ helpers extension : Addition of some usefull Handlebars helpers. cf :ref:`handlebars-helpers` and 
 `Github source <http://github.com/resthub/resthub-backbone-stack/blob/master/js/resthub/handlebars-helpers.js>`_.
- Handlebars_ RequireJS plugin in order to retreive and compile automatically Handlebars templates: cf. :ref:`templating`
- `Backbone Validation`_ extensions : Validation callbacks (``valid`` and ``invalid``) extension to provide a native integration 
  with `Twitter Bootstrap`_ form structure (``controls`` and ``control-group``). cf. 
  `Github source <http://github.com/resthub/resthub-backbone-stack/blob/master/js/resthub/backbone-validation.ext.js>`_

To beneficate of these extensions, we suggest you to replace standard lib inclusion in your require define by the explicit inclusion
of these libs.

Backbone extension is an exception because, to facilitate integration, we override standard ``backbone`` to map it to extented backbone file.

e.g.

.. code-block:: javascript

   define([
       'backbone', 'resthub-handlebars', 'resthub-backbone-validation'
   ], function (Backbone, Handlebars, BackboneValidation) {
      ...
   });
   
By default, resthub archetype generates view including these extensions instead of original libs. Each extension depends on the original lib.

If you don't want to use these extensions, you only have to use the original lib alone : 

.. code-block:: javascript

   define([
       'backbone-orig' 'handlebars', 'backbone-validation'
   ], function (Backbone, Handlebars, BackboneValidation) {
      ...
   });
   
Please note that, as explained before, original backbone distribution is accessible with ``backbone-orig`` path.
   
All extensions paths and shims are defined in ``main.js`` :

.. code-block:: javascript

   paths:{
      ...
      'backbone':'resthub/backbone',
      'backbone-orig':'lib/backbone.ext',
      'backbone-validation':'libs/backbone-validation',
      'resthub-backbone-validation':'resthub/backbone-validation.ext',
      handlebars:'libs/handlebars',
      'resthub-handlebars':'resthub/handlebars-helpers',
      ...
    }

.. _backbone-dispose:
    
Backbone dispose extension and automatic remove binding
-------------------------------------------------------

``Backone.View`` includes now a ``dispose`` method that clean all view, model and collection bindings to properly clean up a view.
This method is called by another View method ``remove`` that also perform a jquery ``view.el`` DOM remove.

Resthub provides three extensions related to this workflow:

1. ``dispose`` extension to add ``Backbone.Validation`` unbind:

   When removing a view and, if ``Backbone.Validation`` is defined, you have also to unbind validation events that call ``validate``,
   ``preValidate`` and ``isValid`` methods.
   
   **This is now automatically done for you by resthub** in ``dispose``.
   
2. Addition of an ``onDispose()`` method called on top of ``dispose``:

   This method is empty by default but can be implemented to perform some complementary actions (unbind, etc.) immediately
   before effective view disposing. You simply have to define such a method in your views:

   .. code-block:: javascript

      onDispose: function() {
         // do something
      }


3. Automatic bind ``dispose`` call on element remove event:

   ``dispose`` method described beside is called by ``remove`` Backbone_ view method. But this method still have to be manually called
   by users (for instance in your router).
   
   Resthub offers an extension to this mechanism that listen any removing on the ``view.el`` DOM element and **automatically call dispose
   on remove**. This means that you don't have to manage this workflow anymore and any replacement done in el parent will trigger a dispose call.
   
   i.e. : each time jQuery ``.html(something)``, ``.remove()`` or ``.empty()`` is performed on view el parent or each time a ``remove()`` is done
   on the el itself, **the view will be properly destroyed**.

.. _backbone-pushstate:
   
Backbone effective pushState extension
--------------------------------------

Backbone_ allows ``pushState`` activation that permits usage of real links instead of simple anchors `#`.
PushState offers better navigation experience and better indexation and search engine ranking:

.. code-block:: javascript

   Backbone.history.start({pushState:true, root:"/"});


`root` option allows to ask Backbone_ to define this path as application context;

However, Backbone_ stops here. Direct access to views by url works fine but, each link leads to
**a full reload**! Backbone_ does not intercept html links and it is necessary to implement it ourselves.

Branyen Tim, the creator of `Backbone boilerplate <http://github.com/tbranyen/backbone-boilerplate>`_ proposes the following solution that
resthub integrates in its extensions with a complementary a test to check pushState activation.

If ``Backbone.history`` is started with the ``pushState`` option, **any click on a link will be intercepted and bound to a Backbone navigation instead**. I you want to
provide **external links**, you only have to use the ``data-bypass`` attribute:

.. code-block:: html

   <a data-bypass href="http://github.com/bmeurant/tournament-front" target="_blank">

.. _backbone-form-helper:

Automatically population of view model from a form
--------------------------------------------------

`Backbone Validation`_ provides some helpers to validate a model against constraints and Backbone_ defines some methods (such as ``save``) to valid
a model and then save it on server. But neither `Backbone Validation`_ nor Backbone_ allow to fill a model stored in a view with form values. 

Resthub comes with a really simple (naive ?) ``Backbone.View`` extension that copy each input field of a given form in a model. This helper is
a new View method called ``populateModel()``. This function has to be explicitely called (e.g. before a ``save()``):

.. code-block:: javascript

   Backbone.View.extend({

      ...
   
      saveUser:function () {
         this.populateModel();

          // save model if its valid, display alert otherwise
          if (this.model.isValid()) {
              this.model.save(null, {
                  success:this.onSaveSuccess.bind(this),
                  error:this.onSaveError.bind(this)
              });
          }   
       }
   });
   
``populateModel`` search for the form element provided and copy each form input value into the provided model attribute that match the
copied form input name. API is: 

.. code-block:: javascript

   /** utility method providing a default and basic handler that
    * populate model from a form input
    *
    * @param form form element to 'parse'. form parameter could be a css selector or a
    * jQuery element. if undefined, the first form of this view el is used.
    * @param model model instance to populate. if no model instance is provided,
    * search for 'this.model'
   **/
   populateModel:function (form, model);
   
So you can use it in multiple ways from your view: 

.. code-block:: javascript

   // take the first el form element and copy values into 'this.model' instance
   this.populateModel();
   
   // get the form element maching provided selector (form with id "myForm") and copy values into 'this.model' instance
   this.populateModel("#myForm");
   
   // get the provided jquery form element and copy values into 'this.model' instance
   this.populateModel($("#myForm");
   
   // take the first el form element and copy values into provided myModel instance
   this.populateModel(null, myModel);
   
   // get the form element maching provided selector (form with id "myForm") and copy values into provided myModel instance
   this.populateModel("#myForm", myModel);
   
   // get the provided jquery form element and copy values into provided myModel instance
   this.populateModel($("#myForm", myModel);

As said before, this approach could appear naive but will probably fit your needs in most of cases. If not, you are free to not use this helper,
to extend this method, globally or locally with your own logic or to use a third party lib to bind model and form (see 
`Backbone.ModelBinder <http://github.com/theironcook/Backbone.ModelBinder>`_ or `Rivets.js <http://rivetsjs.com/>`_ for instance).
    
Avoid caching issues
====================

In order to avoid caching issues when, for instance, you update your JS or HTML files, you should use the 
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

1. **Configure i18n plugin**

In your main.js file you should define a shortcut path for i18n plugin and the default language for your application :

.. code-block:: javascript

    require.config({
        paths: {
            // ...
            i18n: "libs/i18n"
        },
        locale: localStorage.getItem('locale') || 'en-us'
    });


2. **Define labels**

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

Internalionalization can sometimes be tricky since words are not always at the same position depending on the language. In order to make it easier to use, 
RESThub backbone stack include Underscore.String. It contains a sprintf function that you can use for your translations.

You can use the ``_.sprintf()`` function and the ``sprintf`` helper in order to have some replacement in your labels.

labels.js

.. code-block:: javascript

    'root': {
        'clearitem'    : "Clear the completed item",
        'clearitems' : 'Clear %s completed items',
    }

RESThub also provide a ``sprintf`` handlebars helper to use directly in your templates (cf. :ref:`sprintf-helper`), so you can use it easily in your templates:

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

.. _pubsub:
    
Publish Subscribe
=================

Resthub provides publish / subscribe mechanisms over your application with a tiny native ``Backbone.Events`` extension.
Publishing and subscribing are global scopped and allow to communicate between view all over your app.

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
        
      // subscribe to one event (do not forget this)
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

Because of ``Bacbone.View`` and PubSub integration mechanisms (see below) the prefix ``!`` on first index of any global PubSub event
is **strongly recommended**. 

.. warning::

   Do not follow this convention does not have any impact on PubSub behaviour but prevents usage of integrated Backbone.View
   PubSub events declaration (see below)

.. _pubsub-in-views:
   
PubSub and Backbone Views integration
-------------------------------------

In order to facilitate global PubSub events in Backbone Views, Resthub provides some syntaxic sugar with a ``Backbone.View`` extension.
You will able to beneficiate of this extension as soon as you included Restbu Backbone extension instead of original Backbone lib (cf. :ref:`resthub-extensions`).

Backbone Views events hash parsing has been extended to be capable of declaring global PubSub events as it is already done for DOM events binding. To declare such
global events in your Backbone View, you only have to add it in events hash:

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
    
Please not that it is mandatory to prefix your global events with ``!`` to differenciate them from DOM events. You will always have to use the ``!`` prefix
to reference events later (see :ref:`pubsub-usage` for samples).

With this mechanism, PubSub subscribings are automatically declared on View construction, as DOM Events : **You don't have to call PubSub.on on these declared events**.
In the same way, PubSub subscribings for this View are automatically removed during a Backbone ``dispose()`` method call : **You don't have either to call PubSub.off 
on these declared events**.

Obviously, this is still possible for you to explicitely call ``on`` and ``off`` in your view on other global events that you don't want to or you can't declare on 
events hash (e.g. for more dynamic needs). But don't forget to bind this when declaring subscription:

.. code-block:: javascript

   PubSub.on("!event", function () {...}, this);

    
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
