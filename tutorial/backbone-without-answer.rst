RESThub Backbone stack tutorial
===============================

This tutorial will help you to get an overview of resthub-backbone-stack.

Step 1 : Model and View
-----------------------

Find:
+++++

1. **Backbone documentation and description of Backbone.Events, Model, Collection, Router, Sync, View**
  
2. **RequireJS documentation:** 
    
    - requirejs usage
    - how to define a module as function
    - how to use it
    - usage for config options shims and path

3. **Description of resthub-backbone standard project layout based on requireJS**

Do:
+++

1. Get an empty resthub-backbone project via an `archetype <../spring-stack.html#bootstrap-your-project>`_ and discover the base application layout 
          
2. **Create a Task model**

3. **Instantiate a task in main.js with attributes title and description**

4. **Try to see your task object in the console. Make it work**

5. **Try to access to title and description. Is task.title working ?**

6. **Inspect task and find where attributes are stored**

7. **Access title attribute value**

8. **Change description attribute. What operation does backbone perform whena a model attrbute is modified ?** 
    
9. **Create a TaskView and implement render with a function that simply logs "rendered"**

10. **Instantiate view in main and render it. Verify that "rendered" is logged. Try to render view multiple times in console**

11. **Instantiate the view with a task model in main. Modify TaskView render to log the title of the task. No other modification should be made on TaskView**


Write in DOM
++++++++++++

View rendering is done in view relative el element that could be attached anywhere in DOM with jQuery DOM insertion API

Find:
##### 

1. **backbone view's DOM element documentation**

2. **jquery documentation and search for $(), html(), append() methods**
    
Do:
###
            
1. **Modify render to display a task inside a div with class='task' containing title in a h1 and description in a p**

2. **render the view and attach $el to the DOM 'tasks' element (in main.js)**


Templating
++++++++++
        
Let's render our task in DOM with a template engine: Handlebars

Find :
######

1. **Handlebars documentation**
    
2. **How to pass a full model instance as render context in backbone**
    
Do :
####

1. **Create Task handlebars template to display task. Template should start with a div with class='task'**

2. **Load (with requirejs text plugin), compile template in view and render it (pass all model to template)**
    
3. Resthub comes with a `hbs RequireJS extension <../backbone-stack.html#templating>`_ to replace Handlebars.compile. **Change TaskView to use this extension. Remove Handlebars requirement**
   

Model events
++++++++++++

Find :
######

1. **Backbone events documentation and model events catalog**


Do :
####
        
1. **Update task in the console -> does not update the HTML**

2. **Bind model's change event in the view to render. Update task in console: HTML is magically updated!**


Step 2: Collections
-------------------

1. **Create a Tasks collection in** ``collection`` **directory**

2. **Create a TasksView** in ``views`` **and a tasks template in** ``templates``.
3. **Implement rendering in TasksView**
4. **Pass the collection as context**
5. **Iterate through the items in the collection in the template**. **Template should start with an** ``ul``
   **element with class='task-list'**
 
6. **In main: instanciate two task and add them into a new tasks collections. Instantiate View and render it and attach $el to '#tasks' div**

7. **try adding an item to the collection in the console**
        
8. **Bind collection's add event in the view to render**
  
9. **Add a nice fade effect**

10. **Add a task to the collection in the console** -> the *whole* collection in rerendered.


Step 3: Nested Views
--------------------

1. Remove the each block in template.
       
2. Use TaskView in TasksView to render each tasks.

3. Update a task in the console -> the HTML for the task is automatically updated.

4. Add tasks to the collection in the console -> the *whole* list is still rerendered.

5. Update TasksView to only append one task when added to the collection instead of rendering the whole list again.

6. Add a nice fade effect to TaskView.
        
7. Test in the console.
8. Remove automatic generated divs and replace them with lis
   
   goal is to have :
   
   .. code-block:: html
   
        <ul>
            <li class='task'></li>
            <li class='task'></li>
        </ul>
        
   instead of :
   
   .. code-block:: html
   
        <ul>
            <div><li class='task'></li></div>
            <div><li class='task'></li></div>
        </ul>

9. Manage click in TaskView to toggle task's details visibility.


Step 4: Rendering strategy
--------------------------

Find: 
+++++

1. **Resthub documentation for default rendering strategy**
    
Do:
+++

1. **Use Backbone.ResthubView for managing rendering in TaskView. Remove render method in TaskView and modify add method in TasksView to set root element**
        
2. **Re-implement render to get back the fade effect by extending it calling parent function**

3. **Use Backbone.ResthubView for managing rendering in TasksView. Call the parent render function.**

4. **In the console try adding a Task: thanks to the effect we can see that only one more Task is rendered and not the entirely list**

5. **In the console, update an existing Task: thanks to the effect we can see that just this task is updated**


Step 5: Forms
-------------

Do:
+++

1. **Create TaskFormView which is rendered in place when double clicking on a TaskView. Wrap your each form field in a div with** ``class='control-group'`` **. Add**
   ``class='btn btn-success'`` **on your input submit**

2. **When the form is submitted, update the task with the changes and display it
   again -> note that the change event is not triggered when there was no
   changes at all.**
  
3. **Force change event to be raised once and only once**
  
4. **Add a button to create a new empty task. In TasksView, bind its click event
   to a create method which instantiate a new empty task with a TaskView which
   is directly editable. Add** ``class="btn btn-primary"`` **to this button**
  
5. **Note that you have to add the task to the collection otherwise when you
   render the whole collection again, the created tasks disappear. Try by attach
   tasksView to windows and call render() from console**

6. **Add a cancel button in TaskFormView to cancel task edition. Add a**
   ``class="btn cancel"`` **to this button**
        
7. **Add a delete button which delete a task. Add** ``class="btn btn-danger delete"`` 
   **to this button. Remove the view associated to this task on delete click and remove 
   the task from the collection**
    
   Note that we can't directly remove it from the collection cause the
   TaskFormView is not responsible for the collection management and does not
   have access to this one.
   
   **Then use the model's destroy method and note that Backbone will automatically
   remove the destroyed object from the collection on a destroy event**
   
 
8. **Note in the console that when removing a task manually in the collection, it
   does not disappear**
    
9. **Bind remove event on the collection to call** ``task.destroy()`` **in TasksView**

10. **Test again in the console**


Step 6: Validation
------------------

Find:
+++++

1. **Backbone documentation about model validation**

2. **Resthub documentation for populateModel**


Do:
+++

1. **Implement validate function in Task model: make sure that the title is not
   blank**
        
2. **In TaskFormView, on save method, get the result of set method call on attributes and 
   trigger "change" event only if validation passes**
   
3. **Update TaskForm template to add a span with class** ``help-inline`` **immediately after title input**
        
4. **In TaskFormView bind model's error event on a function which renders
   validation errors. On error, add class "error" on title input and display error in span "help-inline"**  
        
5. **Use Backbone.Validation for easy validation management**

6. **Note that Backbone.Validation can handle for you error displaying in your
   views: remove error bindings and method and ensure that you form input have
   a name attribute equals to the model attribute name**
   
7. **Rewrite save method using resthub** ``populateModel`` and backbone ``isValid``


Step 7: Persist & Sync
----------------------

* Our data are not persisted, after a refresh, our task collection will be
  reinitialized.
* Use Backbone local storage extension to persist our tasks into the local
  storage.
* Bind the collection's reset event on TasksView.render to render the
  collection once synced with the local storage.
* Warning: you need to specify the model attribute in the Tasks collection to
  tell the collection which model object is gonna be used internally.
  Otherwise, when fetching, the returned JSON object will be added directly to
  the collection without instantiating a Task. As a consequence every specific
  attributes (like validation hash), would be unavailable in the model. At this
  step, if validation does not work anymore after fetching the tasks through
  Backbone.sync, check that the model attribute is correctly set in the
  collection.
  

Step 8 : server backend
-----------------------

* Download `RESThub Spring training sample project <https://github.com/resthub/resthub-spring-training/zipball/step5-solution>`_ and extract it
* Create jpa-webservice/src/main/webapp directory, and move your JS application into it
* Run the jpa-webservice webapp thanks to Maven Jetty plugin
* Remove backbone-localstorage.js file and usage in JS application
* Make your application retreiving tasks from api/task?page=no URL
* Validate that retreive, delete, create and update actions work as expected with this whole new jpa-webservice backend

