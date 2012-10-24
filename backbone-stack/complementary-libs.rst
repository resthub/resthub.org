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
- **Get and set relations (one-to-one, one-to-many, many-to-one) for Backbone models:** `Backbone Relational`_
- **Parsing, validating, manipulating, and formatting dates:** `Moment`_

Resthub only provide require config shims and paths for these libs in ``main.js`` and you are totaly free to use these libs or not:

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
------------------------------------

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

Get and set relations for Backbone models
-----------------------------------------

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

Parsing, validating, manipulating, and formatting dates
-------------------------------------------------------

`Moment`_ is a date library for parsing, validating, manipulating, and formatting dates.

Moment.js features:
 * Parse and format date with custom pattern and internationalization
 * Date manipulation (add, substract)
 * Durations (eg: 2 hours)


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
.. _Backbone Relational: https://github.com/PaulUithol/Backbone-relational
.. _Moment: http://momentjs.com/