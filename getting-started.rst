===============
Getting Started
===============

Requirements
============

Before coding, you should download and install the following development tools if needed : 
 * `Java 7 <http://java.sun.com/javase/downloads/index.jsp>`_
 * `Maven 3 <http://maven.apache.org/>`_
 * A Java and Javascript IDE. If you don't know which what IDE to chose, `Netbeans <http://netbeans.org/>`_ is recommended since it is free and have great Java and Javascript capabilities.

Project templates 
=================

The easiest way to start is to use RESThub archetypes to create your first HTML5 application. Just open a command line terminal, and type :

.. code-block:: bash

	mvn archetype:generate -DarchetypeCatalog=http://nexus.pullrequest.org/content/repositories/releases/

Or if you want to use bleeding edge branch :

.. code-block:: bash

	mvn archetype:generate -DarchetypeCatalog=http://nexus.pullrequest.org/content/repositories/snapshots/

You will have to choose between the following RESThub archetypes :
	* **resthub-archetype-spring-jpa-webservice** : simple webservice project with JPA persistence
	* **resthub-archetype-spring-mongodb-webservice** : simple webservice project with MongoDB persistence
	* **resthub-archetype-spring-jpa-backbonejs** : simple HTML5 web application with JPA persistence
	* **resthub-archetype-spring-mongodb-webservice** : simple HTML5 web application with MongoDB persistence
 
After choosing the right archetype and answering a few questions, your project is generated and ready to use.
You can run it thanks to built-in Jetty support :

.. code-block:: bash

	mvn jetty:run

Example application
===================

Check the `Todo RESThub 2.0 example <https://github.com/resthub/todo-example>`_  in order to learn how to design your RESThub based web application.
 
In order to test and run it :
 * Download the `zip file <https://github.com/resthub/todo-example/zipball/master>`_ and extract it
 * Run mvn jetty:run
 * Open your browser and browse http://localhost:8080/index.html

