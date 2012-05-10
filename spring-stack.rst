============
Spring Stack
============

Here is the description of each RESThub modules, with related dependency snippets and documentation.

Here is a collection of documentation, tutorial or examples for technologies included in RESThub stack.


Spring stack
============

The whole RESThub 2.0 Spring stack `Javadoc <http://jenkins.pullrequest.org/job/resthub-spring-stack-resthub2/javadoc/>`_ is available.

Common
------

RESThub Common define the dependencies commons to all RESThub Spring except Web Client :	
 	* Maven 3.0 (`complete reference <http://www.sonatype.com/books/mvnref-book/reference/public-book.html>`_)
 	* Spring 3.1 (`reference manual <http://static.springsource.org/spring/docs/3.1.x/spring-framework-reference/html>`_ and `Javadoc <http://static.springsource.org/spring/docs/3.1.x/javadoc-api/>`_)
 	* Logback (`manual <http://logback.qos.ch/manual/index.html>`_)

You should not have to use reference directly resthub-common module in your application, it will be a transtive dependency of other RESThub modules described bellow.

JPA support
-----------

JPA support is based on Spring Data JPA and include by default the H2 in memory database and is based on the following dependencies :
	 	* Spring Data JPA (`reference manual <http://static.springsource.org/spring-data/data-jpa/docs/current/reference/html/>`_ and `Javadoc <http://static.springsource.org/spring-data/data-jpa/docs/current/api/>`_)
	 	* Hibernate `documentation <http://www.hibernate.org/docs.html>`_
	 	* `H2 embedded database <http://www.h2database.com/html/main.html>`_

Thanks to Spring Data, it is possible to create Repositories (also sometimes named DAO) by writter only the interface.

In order to use it in your project, add the following code snippet to your pom.xml :

.. code-block:: xml

	<dependencies>
	
		<dependency>
			<groupId>org.resthub</groupId>
			<artifactId>resthub-jpa</artifactId>
			<version>2.0-SNAPSHOT</version>
		</dependency>

	</dependencies>

MongoDB support
---------------

MongoDB is also based on Spring Data MongoDB :
* Spring Data MongoDB `reference manual <http://static.springsource.org/spring-data/data-mongodb/docs/current/reference/html/>`_ and `Javadoc <http://static.springsource.org/spring-data/data-mongodb/docs/current/api/>`_

In order to use it in your project, add the following code snippet to your pom.xml :

.. code-block:: xml

	<dependencies>
	
		<dependency>
			<groupId>org.resthub</groupId>
			<artifactId>resthub-mongodb</artifactId>
			<version>2.0-SNAPSHOT</version>
		</dependency>

	</dependencies>

Web server
----------

RESThub Web Server module is designed to allow you to develop REST webservices. Both JSON(default) and XML serializatgion are supported.

It provides some generic REST controller classes, and include the following dependencies :
	* Spring MVC 3.1 (`reference manual <http://static.springsource.org/spring/docs/3.1.x/spring-framework-reference/html/mvc.html>`_)
	* Jackson 1.9 (`documentation <http://wiki.fasterxml.com/JacksonDocumentation>`_)

In order to use it in your project, add the following snippet to your pom.xml :

.. code-block:: xml

	<dependencies>
	
		<dependency>
			<groupId>org.resthub</groupId>
			<artifactId>resthub-web-server</artifactId>
			<version>2.0-SNAPSHOT</version>
		</dependency>

	</dependencies>

Web client
----------

RESThub Web client module goal is to give you an easy way to request other REST webservices. It is based on AsyncHttpClient and provide a `client API wrapper <http://jenkins.pullrequest.org/job/resthub-spring-stack-resthub2/javadoc/>`_ in order to use it easily. In order to limit conflicts it has no dependency on Spring, but only on :
 	* AsyncHttpClient `documentation <https://github.com/sonatype/async-http-client>`_ and `Javadoc <http://sonatype.github.com/async-http-client/apidocs/reference/packages.html>`_
 	* Jackson 1.9 (`documentation <http://wiki.fasterxml.com/JacksonDocumentation>`_)

 In order to use it in your project, add the following snippet to your pom.xml :

.. code-block:: xml

	<dependencies>
	
		<dependency>
			<groupId>org.resthub</groupId>
			<artifactId>resthub-web-server</artifactId>
			<version>2.0-SNAPSHOT</version>
		</dependency>

	</dependencies>
 
Testing
-------
	
The following test stack is inclusing in the RESThub test module :
	* TestNG `documentation <http://testng.org/doc/documentation-main.html>`_
	* Fest Assert 2.x `wiki <https://github.com/alexruiz/fest-assert-2.x/wiki>`_
	* `FluentLenium <http://www.fluentlenium.org/>`_

.. code-block:: xml

	<dependencies>
	
		<dependency>
			<groupId>org.resthub</groupId>
			<artifactId>resthub-test</artifactId>
			<version>2.0-SNAPSHOT</version>
			<scope>test</scope>
		</dependency>

	</dependencies>
