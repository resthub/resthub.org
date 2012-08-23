=================
Backbone.js Stack
=================

The Backbone.js stack includes the following librairies :
    * **jQuery 1.7** (`documentation <http://docs.jquery.com/Main_Page>`_)
    * **Backbone.js 0.9.2** (`documentation <http://documentcloud.github.com/backbone/>`_) and its `localstorage adapter <http://documentcloud.github.com/backbone/docs/backbone-localstorage.html>`_
    * **Underscore.js 1.3.3** (`documentation <http://documentcloud.github.com/underscore/>`_)
    * **Underscore.String 2.0.0** (`documentation <https://github.com/epeli/underscore.string#readme>`_)
    * **Require 2.0** with `i18n <http://requirejs.org/docs/api.html#i18n>`_ and `text <http://requirejs.org/docs/api.html#text>`_ plugins (`documentation <http://requirejs.org/docs/api.html>`_)
    * **Handlebars 1.0** (`documentation <http://handlebarsjs.com>`_)
    * A **console shim** for browsers that don't support it
    * A RESThub **PubSub implementation**
    * **Twitter Bootstrap 2.0** (`documentation <http://twitter.github.com/bootstrap/>`_) with Require.js compatible JS files

How should I use it in my project
=================================

There are 3 ways to use it in your project :
    * If you are starting a new Java project, the better way to use RESThub Backbone.js stack is to use one of the Backbone.js webappp Maven Archetypes described on the `Getting Started page <getting-started.html>`_
    * You can simply go to `RESThub Backbone.js stack GitHub repository <https://github.com/resthub/resthub-backbone-stack>`_, download the repository content and copy it at the root of your webapp
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

`Todo RESThub 2.0 example <https://github.com/resthub/todo-example>`_ will also be useful to see a real project working with this stack.

Project structure
=================

You should read carefully the awesome blog post `Organizing your application using Require.js Modules <http://backbonetutorials.com/organizing-backbone-using-modules/>`_ since it describes the project structure and principles recommended in RESThub Backbone stack based projects.

Bootstrapping
=============

Please find below the default files needed to bootstrap your webapp (An easier and error proof method is to use RESThub archetypes in order to bootstrap your project).

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


index.html is provided by Backbone stack, so you don't have to create it. Your application bootstrap file is the main.js located at your webapp root (usually src/main/webapp). Please find below a sample :

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
               ],
               exports:'_s'
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
           }
       },

       // Libraries
       paths:{
           jquery:"libs/jquery",
           underscore:"libs/underscore",
           'underscore.string':"libs/underscore.string",
           backbone:"libs/backbone",
           localstorage:"libs/localstorage",
           use:"libs/use",
           text:"libs/text",
           i18n:"libs/i18n",
           pubsub:"libs/resthub/pubsub",
           handlebars:"libs/handlebars",
           'handlebars-helpers':"libs/handlebars-helpers"
       }
   });

   // Preload main libs
   require(['app', 'backbone', 'handlebars', 'handlebars-helpers'], function (App) {

       App.initialize();
   });
   
- **shim** config is part of `Require 2.0`_ and allows to `Configure the dependencies and exports for older, traditional "browser globals" 
  scripts that do not use define() to declare the dependencies and set a module value`. See `<http://requirejs.org/docs/api.html#config-shim>`_ for details.
- **path** config is also part of Require_ and allows to define paths for libs not found durectly under baseUrl. See `<http://requirejs.org/docs/api.html#config-paths>`_ for details.
- resthub suggests to **preload some libs** that will be used surely as soon the app start (typically backbone itself and our template engine). This mechanism also
  allows us to load other linked libs transparently without having to define it repeatedly (e.g. ``underscore.string`` loading - this libs is strongly correlated
  to ``underscore`` - and merged with it and thus should not have to be defined anymore)

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

Templates are injected into Views thanks to RequireJS text plugin. So it should be defined in your main.js :

.. code-block:: javascript

    require.config({
        paths: {
            // ...
            text: "libs/text"
        }
    });

Sample usage in a Backbone.js View :

.. code-block:: javascript

    define(['jquery', 'backbone', 'handlebars', 'text!templates/todo.html'],function($, Backbone, Handlebars, todoTemplate) {
        var TodoView = Backbone.View.extend({

        //... is a list tag.
        tagName:  "li",

        // Compile and cache the template function for a single item.
        template: Handlebars.compile(todoTemplate),

        render: function() {
            // todoTemplate a function that take context (labels, model) and return the dynamized output.
            var result = this.template(this.model.toJSON());
            $(this.el).html(result);
            return this;
        }
    });
    
Helpers
-------

Resthub provide some usefull **Handlebars helpers** included by default in ``main.js`` :

ifinline
++++++++

This helper provides a more fluent syntax for inline ifs. i.e. if embedded in quoted strings.

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

.. todo:: document sprintf helper

Avoid caching issues
====================

In order to avoid caching issues when, for example, you update your JS or HTML files, you should use the `urlArgs RequireJS attribute <http://requirejs.org/docs/api.html#config>`_. You can filter the ${buildNumber} with your build tool at each build.


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

You should never use directly labels or texts in your source files. All labels should be externalized in order to prepare your application internationalization. Doing such thing is pretty simple with RESThub Backbone.js stack thanks to `requireJS i18n plugin <http://requirejs.org/docs/api.html#i18n>`_.

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

Create a labels.js file in the js/nls directory, it will contain labels in the default locale used by your application. You can change labels.js to another name (messages.js or functionality related name like user.js or product.js) but js/nls is the default location. Specify at the same level than the root node the available translations.

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

Add a dependency in the js, typically a View, where you'll need labels. You'll absolutely need to give a scoped variable to the result (in this example ``labels``, but you can choose the one you want). 

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

.. todo:: rewrite this paragraph with a sprintf handlebars helper

.. code-block:: html

    <%= done == 1 ? messages.clearitem : _.sprintf(messages.clearitems, done) %>

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