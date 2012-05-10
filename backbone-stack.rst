=================
Backbone.js Stack
=================

The Backbone.js stack includes the following librairies :
	* jQuery 1.7 (`documentation <http://docs.jquery.com/Main_Page>`_)
	* Backbone.js 0.9 (`documentation <http://documentcloud.github.com/backbone/>`_) and its `localstorage adapter <http://documentcloud.github.com/backbone/docs/backbone-localstorage.html>`_
	* Underscore.js 1.3 (`documentation <http://documentcloud.github.com/underscore/>`_)
	* Underscore.String (`documentation <https://github.com/epeli/underscore.string#readme>`_)
	* Require.js 1.0 with `i18n <http://requirejs.org/docs/api.html#i18n>`_ and `text <http://requirejs.org/docs/api.html#text>`_ plugins (`documentation <http://requirejs.org/docs/api.html>`_)
	* A console shim for browser that don't support it
	* A RESThub PubSub implementation

You should read carefully the awesome blog post `Organizing your application using Require.js Modules <http://backbonetutorials.com/organizing-backbone-using-modules/>`_ since it describe exactly the project structure and principles recommanded in RESThub projects.

In order to use it in your project, add the following code snippet to your pom.xml :

.. code-block:: xml

	<dependencies>
	
		<dependency>
			<groupId>org.resthub</groupId>
			<artifactId>resthub-backbone-stack</artifactId>
			<version>2.0-SNAPSHOT</version>
		</dependency>

	</dependencies>

`Todo RESThub 2.0 example <https://github.com/resthub/todo-example>`_ will also be useful to see a real project working with this stack.

