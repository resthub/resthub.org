============
Spring Stack
============

RESThub 2 Spring stack provides a server side full stack and guidelines for building Java/Spring application (usually web application, but not only).

.. contents::
   :depth: 3

It provides a coherent stack based on :
	* `Java <http://www.oracle.com/technetwork/java/javase/downloads/index.html>`_ (at least JDK6, JDK7 recommended)
	* `Tomcat 7 <http://tomcat.apache.org/download-70.cgi>`_ (RESThub can also be used for non web applications)
	* Spring 3.1 (`reference manual <http://static.springsource.org/spring/docs/3.1.x/spring-framework-reference/html>`_ and `Javadoc <http://static.springsource.org/spring/docs/3.1.x/javadoc-api/>`_)
 	* SQL and NoSQL Persistence with `Spring Data <http://www.springsource.org/spring-data>`_
 	* Logging with SLF4J (`manual <http://www.slf4j.org/manual.html>`_) and Logback (`manual <http://logback.qos.ch/manual/index.html>`_)
 	* Maven 3.0 (`complete reference <http://www.sonatype.com/books/mvnref-book/reference/public-book.html>`_) is the reference build tool used.

It provides the following modules :
	* **resthub-archetypes** : project templates (WAR or multi-module layout) to start quickly a new project
	* **resthub-jpa** : support for JPA based persistence based on Spring Data, including embedded H2 database for testing
	* **resthub-mongodb** : support for MongoDB based on Spring Data
	* **resthub-test** : testing stack based on TestNG, Mockito and Fest Assert 2
	* **resthub-web-server** : generic REST webservices support based on Spring MVC 3.1 including exception mapping to HTTP status codes
	* **resthub-web-client** : simple to use HTTP client based on AyncHttpClient

The whole RESThub 2.0 Spring stack `Javadoc <http://jenkins.pullrequest.org/job/resthub-spring-stack-master/javadoc/>`_ is available.

Bootstrap your project
======================

Java and Maven 3 should be installed on your computer. RESThub based applications are usually developed thanks to a Java IDE like Eclipse, Netbeans or IntelliJ IDEA. If you don't know which IDE to choose, `Netbeans <http://netbeans.org/>`_ is recommended since it is free and has great Maven support and Java/Javascript capabilities.

The easiest way to start is to use RESThub archetypes to create your first web application. Just open a command line terminal, and type :

.. code-block:: bash

	mvn archetype:generate -DarchetypeCatalog=http://nexus.pullrequest.org/content/repositories/releases/

You will have to choose between the following RESThub archetypes :
	* **resthub-jpa-webservice-archetype** : simple webservice project with JPA persistence
	* **resthub-mongodb-webservice-archetype** : simple webservice project with MongoDB persistence
	* **resthub-jpa-backbonejs-archetype** : simple HTML5 web application with JPA persistence
	* **resthub-mongodb-backbonejs-archetype** : simple HTML5 web application with MongoDB persistence
	* **resthub-jpa-backbonejs-multi-archetype** : Multimodules HTML5 web application with JPA persistence
	* **resthub-mongodb-backbonejs-multi-archetype** : Multimodules HTML5 web application with MongoDB persistence
 
After choosing the right archetype and answering a few questions, your project is generated and ready to use.
You can run it thanks to built-in Jetty support :

.. code-block:: bash

	mvn jetty:run

Tutorial
========

You should follow `RESThub Spring Stack tutorial <tutorial/spring.html>`_ (solution provided at the end) in order to learn step by step how to use it.

Project layout
==============

Let's take a look at a typical RESThub based application...

RESThub stack based projects follow the "Maven standard" project layout :
	* /pom.xml: the Maven configuration file which defines dependencies, plugins, etc.
	* /src/main/java: your java classes go there
	* /src/main/java/\*\*/WebAppInitializer.java: Java based WebApp configuration (replaces your old web.xml file)
	* /src/main/resources : your xml and properties files go there
	* /src/main/resources/applicationContext.xml: this is your Spring application configuration file. Since we mainly use annotation based configuration, 
	* /src/main/webapp: your HTML, CSS and javascript files go there
 
RESThub based applications usually use one of these 2 layouts :
	* A single WAR project, usually for demo or small projects
 	* A multi-module project with the following sub-modules :
 		* myproject-webapp (WAR) : it is your web application, it contains static resources, environment specific configuration and it declares dependencies to other modules in the pom.xml
 		* myproject-contract (JAR) : contains your POJOs (Entities, DTO ...) and service interface. This module should be used by web client or RPC mechanism to know the public classes and interfaces of your application without retreiving all the implementation dependencies. As a consequence, if you need to add some implementation dependencies (usually needed for annotations), add them as optional Maven dependencies.
 		* myproject-core (JAR) : your project implementation (controllers, service implementations, repositories)
 		* myproject-client (JAR) : optional REST client that should implement controller interface with an implementation based on resthub-web-client and myproject-contract.

Check the `RESThub 2 Todo example application <https://github.com/resthub/todo-example>`_ source code to learn how to design your RESThub based web application.
 
How to run the todo application :
 * Download the `zip file <https://github.com/resthub/todo-example/zipball/master>`_ and extract it
 * Install `MongoDB <http://www.mongodb.org/downloads>`_, create the data folder (C:\data\db by default) and run mondgod
 * Run mvn jetty:run in the todo-example directory
 * Open your browser and browse http://localhost:8080/index.html

Configuration
-------------

You will find below the typical configuration file for your application.

Maven
~~~~~

Your project pom.xml defines your project name, version, dependencies and plugins used.
Please notice that it is easier to let RESThub archetypes create the pom.xml automatically for you.

pom.xml example :

.. code-block:: xml

	<?xml version="1.0" encoding="UTF-8"?>
	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
		xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
		<modelVersion>4.0.0</modelVersion>

		<groupId>com.mycompany</groupId>
		<artifactId>myproject</artifactId>
		<version>1.0-SNAPSHOT</version>
		<packaging>war</packaging>

		<name>My project</name>

		<properties>
			<resthub.spring.stack.version>2.x</resthub.spring.stack.version>
			<resthub.backbone.stack.version>2.x</resthub.backbone.stack.version>
		</properties>

		<dependencies>
			<dependency>
				<groupId>org.resthub</groupId>
				<artifactId>resthub-mongodb</artifactId>
				<version>${resthub.spring.stack.version}</version>
			</dependency>
			<dependency>
				<groupId>org.resthub</groupId>
				<artifactId>resthub-web-server</artifactId>
				<version>${resthub.spring.stack.version}</version>
			</dependency>
			<dependency>
				<groupId>org.resthub</groupId>
				<artifactId>resthub-backbone-stack</artifactId>
				<version>${resthub.backbone.stack.version}</version>
				<type>war</type>
			</dependency>
			<dependency>
				<groupId>javax.servlet</groupId>
				<artifactId>javax.servlet-api</artifactId>
				<version>3.0.1</version>
				<scope>provided</scope>
			</dependency>
		</dependencies>

		<build>
			<finalName>todo</finalName>
			<plugins>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-compiler-plugin</artifactId>
					<version>2.5.1</version>
					<configuration>
						<encoding>UTF-8</encoding>
						<source>1.7</source>
						<target>1.7</target>
					</configuration>
				</plugin>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-resources-plugin</artifactId>
					<version>2.6</version>
					<configuration>
						<encoding>UTF-8</encoding>
					</configuration>
				</plugin>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-war-plugin</artifactId>
					<version>2.3</version>
					<configuration>
						<failOnMissingWebXml>false</failOnMissingWebXml>
					</configuration>
				</plugin>
				<plugin>
					<groupId>org.mortbay.jetty</groupId>
					<artifactId>jetty-maven-plugin</artifactId>
					<version>8.1.7.v20120910</version>
					<configuration>
						<!-- We use non NIO connector in order to avoid read only static files under windows -->
						<connectors>
							<connector implementation="org.eclipse.jetty.server.bio.SocketConnector">
								<port>8080</port>
								<maxIdleTime>60000</maxIdleTime>
							</connector>
						</connectors>
					</configuration>
				</plugin>
			</plugins>
		</build>

		<repositories>
			<repository>
				<id>resthub</id>
				<url>http://nexus.pullrequest.org/content/groups/resthub</url>
			</repository>
		</repositories>

	</project>

The available RESThub dependencies are the following

.. code-block:: xml

    <dependency>
        <groupId>org.resthub</groupId>
        <artifactId>resthub-jpa</artifactId>
        <version>2.x</version>
    </dependency>

    <dependency>
        <groupId>org.resthub</groupId>
        <artifactId>resthub-mongodb</artifactId>
        <version>2.x</version>
    </dependency>

    <dependency>
        <groupId>org.resthub</groupId>
        <artifactId>resthub-web-server</artifactId>
        <version>2.x</version>
    </dependency>

    <dependency>
        <groupId>org.resthub</groupId>
        <artifactId>resthub-web-client</artifactId>
        <version>2.x</version>
    </dependency>

    <dependency>
        <groupId>org.resthub</groupId>
        <artifactId>resthub-test</artifactId>
        <version>2.x</version>
        <scope>test</scope>
    </dependency>

Web application initializer
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Web application initializer replaces the old web.xml file used with Servlet 2.5 or older webapps. It has the same goal, but since it is Java based, it is safer (compilation check, autocomplete).

WebAppInitializer.java example :

.. code-block:: java

	public class WebAppInitializer implements WebApplicationInitializer {

	    @Override
	    public void onStartup(ServletContext servletContext) throws ServletException {
	       	XmlWebApplicationContext appContext = new XmlWebApplicationContext();
	        appContext.getEnvironment().setActiveProfiles("resthub-jpa", "resthub-web-server");
	        String[] locations = { "classpath*:resthubContext.xml", "classpath*:applicationContext.xml" };
	        appContext.setConfigLocations(locations);

	        ServletRegistration.Dynamic dispatcher = servletContext.addServlet("dispatcher", new DispatcherServlet(appContext));
	        dispatcher.setLoadOnStartup(1);
	        dispatcher.addMapping("/*");

	        servletContext.addListener(new ContextLoaderListener(appContext));
	    }
	}

Spring profiles
~~~~~~~~~~~~~~~

RESThub 2 uses `Spring 3.1 profiles <http://blog.springsource.com/2011/02/14/spring-3-1-m1-introducing-profile/>`_ to let you activate or not each module. It allows you to add Maven dependencies for example on resthub-jpa and resthub-web-server and let you control when you activate these modules. It is especially useful when running unit tests: when testing your service layer, you may not need to activate the resthub-web-server module.

You can also use Spring profile for your own application Spring configuration.

Profile activation on your webapp is done very early in the application lifecycle, and is done in your Web application initializer (Java equivalent of the web.xml) described just before. Just provide the list of profiles to activate in the onStartup() method:

.. code-block:: java

	XmlWebApplicationContext appContext = new XmlWebApplicationContext();
	appContext.getEnvironment().setActiveProfiles("resthub-mongodb", "resthub-web-server");

In your tests, you should use the @ActiveProfiles annotation to activate the profiles you need:

.. code-block:: java

	@ActiveProfiles("resthub-jpa") // or @ActiveProfiles({"resthub-jpa","resthub-web-server"})
	public class SampleTest extends AbstractTransactionalTest {

	}

RESThub web tests comes with a helper to activate profiles too:

.. code-block:: java

	public class SampleControllerTest extends AbstractWebTest {

	    public SampleControllerTest() {
	        // Call AbstractWebTest(String profiles) constructor
	        super("resthub-web-server,resthub-jpa");
	    }
	}

RESThub built-in Spring profiles have the same name than their matching module :
	* resthub-jpa
	* resthub-mongodb
	* resthub-web-server

Spring based configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~

By default RESThub webservices and unit tests scan and automatically include all resthubContext.xml (RESThub context files) and applicationContext.xml files (your application context files) available in your application classpath, including its dependencies.

Here is an example of a typical RESThub based src/main/resources/applicationContext.xml (this one uses JPA, you may adapt it if you use MongoDB) :

.. code-block:: xml

	<beans xmlns="http://www.springframework.org/schema/beans"
	       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	       xmlns:jpa="http://www.springframework.org/schema/data/jpa"
	       xmlns:context="http://www.springframework.org/schema/context"
	       xsi:schemaLocation="http://www.springframework.org/schema/beans 
	                           http://www.springframework.org/schema/beans/spring-beans.xsd
	                           http://www.springframework.org/schema/context 
	                           http://www.springframework.org/schema/context/spring-context.xsd
	                           http://www.springframework.org/schema/data/jpa 
	                           http://www.springframework.org/schema/data/jpa/spring-jpa.xsd">

	    <context:component-scan base-package="org.mycompany.myproject" />
	    <jpa:repositories base-package="org.mycompany.myproject.repository" />
	    
	</beans>

logback.xml
~~~~~~~~~~~

You'll usually have a src/main/resources/logback.xml file in order to configure logging :

.. code-block:: xml

	<configuration> 
		<appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        	<encoder>
            	<pattern>%d{HH:mm:ss} [%thread] %-5level %logger{26} - %msg%n%rEx</pattern>
       		</encoder>
    	</appender>
		<root level="info"> 
			<appender-ref ref="CONSOLE"/> 
		</root> 
	</configuration>

Beans declaration and injection
-------------------------------

You should use J2EE6 annotations to declare and inject your beans.

To declare a bean:

.. code-block:: java

   @Named("beanName")
   public class SampleClass {
   
   }

To inject a bean by type (default):

.. code-block:: java

   @Inject
   public void setSampleProperty(...) {
   
   }

Or to inject a bean by name (Allow more than one bean implementing the same interface):

.. code-block:: java

   @Inject @Named("beanName")
   public void setSampleProperty(...) {
   
   }

CRUD services
-------------

RESThub is designed to give you the choice between a 2 layers (Controller -> Repository) or a 3 layers (Controller -> Service -> Repository) software architecture. If you choose the 3 layers one, you can use the RESThub CRUD service when it is convenient :

.. code-block:: java

	@Named("webSampleResourceService")
	public class WebSampleResourceServiceImpl extends CrudServiceImpl<Sample, Long, WebSampleResourceRepository>
        implements WebSampleResourceService {

	    @Override @Inject
	    public void setRepository(WebSampleResourceRepository webSampleResourceRepository) {
	        super.setRepository(webSampleResourceRepository);
	    }
	}

Environment specific properties
-------------------------------

There are various ways to configure your environment specific properties in your application: the one described below is the most simple and flexible way we have found. 

Maven filtering (search and replace variables) is not recommended because it is done at compile time (not runtime) and makes usually your JAR/WAR specific to an environment. This feature can be useful when defining your target path (${project.build.directory}) in your src/test/applicationContext.xml for testing purpose.

Spring properties placeholders + @Value annotation is the best way to do that.

.. code-block:: xml

   <context:property-placeholder location="classpath*:mymodule.properties"
                                 ignore-resource-not-found="true"
                                 ignore-unresolvable="true" />

You should now be able to inject dynamic values in your code, where InMemoryRepository is the default :

.. code-block:: java

	@Configuration
	public class RequestConfiguration {

	   @Value(value = "${repository:InMemoryRepository}")
	   private String repository;
	}

JPA support
===========

JPA support is based on Spring Data JPA and includes by default the H2 in memory database. It includes the following dependencies :
	 	* Spring Data JPA (`reference manual <http://static.springsource.org/spring-data/data-jpa/docs/current/reference/html/>`_ and `Javadoc <http://static.springsource.org/spring-data/data-jpa/docs/current/api/>`_)
	 	* Hibernate `documentation <http://www.hibernate.org/docs.html>`_
	 	* `H2 embedded database <http://www.h2database.com/html/main.html>`_

Thanks to Spring Data, it is possible to create repositories (also sometimes named DAO) by writing only the interface.

Configuration
-------------

In order to use it in your project, add the following snippet to your pom.xml:

.. code-block:: xml

    <dependency>
        <groupId>org.resthub</groupId>
        <artifactId>resthub-jpa</artifactId>
        <version>2.x</version>
    </dependency>

In order to import its `default configuration <https://github.com/resthub/resthub-spring-stack/blob/master/resthub-jpa/src/main/resources/resthubContext.xml>`_, your should activate the resthub-jpa Spring profile in your WebAppInitializer class:

.. code-block:: java

    XmlWebApplicationContext appContext = new XmlWebApplicationContext();
	appContext.getEnvironment().setActiveProfiles("resthub-jpa", "resthub-web-server");

Spring 3.1 allows to scan entities in different modules using the same PersitenceUnit, which is not possible with default JPA behaviour. You have to specify the packages where Spring should scan your entities by creating a database.properties file in your resources folder, with the following content :


.. code-block:: properties

   persistenceUnit.packagesToScan = com.myproject.model

Now, entities within the com.myproject.model packages will be scanned, no need for persistence.xml JPA file.


You also need to add an applicationContext.xml file in order to scan your repository package.

.. code-block:: xml

	<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:jpa="http://www.springframework.org/schema/data/jpa"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/data/jpa
                           http://www.springframework.org/schema/data/jpa/spring-jpa.xsd">

	    <jpa:repositories base-package="com.myproject.repository" />

	</beans>

You can customize the default configuration by adding a database.properties resource with one or more of the following keys customized with your values. You should include only the customized ones.

REShub JPA default properties are :
	* dataSource.driverClassName = org.h2.Driver
	* dataSource.url = jdbc:h2:mem:resthub;DB_CLOSE_DELAY=-1;MVCC=TRUE
	* dataSource.maxActive = 50
	* dataSource.maxWait = 1000
	* dataSource.poolPreparedStatements = true
	* dataSource.username = sa
	* dataSource.password = 
	* dataSource.validationQuery = SELECT 1

REShub Hibernate default properties are :
	* hibernate.dialect = org.hibernate.dialect.H2Dialect
	* hibernate.show_sql = false
	* hibernate.format_sql = true
	* hibernate.hbm2ddl.auto = update
	* hibernate.cache.use_second_level_cache = true
	* hibernate.cache.provider_class = net.sf.ehcache.hibernate.SingletonEhCacheProvider
	* hibernate.id.new_generator_mappings = true
	* persistenceUnit.packagesToScan = 

 If you need to do more advanced configuration, just override dataSource and entityManagerFactory beans in your applicationContext.xml.

Usage
-----

.. code-block:: java

	public interface TodoRepository extends JpaRepository<Todo, String> {
	    
	    List<Todo> findByContentLike(String content);
	       
	}

Console
-------

H2 console allows you to provide a SQL requester for your embedded default H2 database. It is included by default in JPA archetypes.

In order to add it to your JPA based application, add these lines to your WebAppInitializer class : 

.. code-block:: java

    public void onStartup(ServletContext servletContext) throws ServletException {
        ...
        ServletRegistration.Dynamic h2Servlet = servletContext.addServlet("h2console", WebServlet.class);
        h2Servlet.setLoadOnStartup(2);
        h2Servlet.addMapping("/console/database/*");
           
    }

When running the webapp, the database console will be available at http://localhost:8080/console/database/ URL with following parameters :
 * JDBC URL : jdbc:h2:mem:resthub
 * Username : sa
 * Password :

MongoDB support
===============

MongoDB support is based on Spring Data MongoDB (`reference manual <http://static.springsource.org/spring-data/data-mongodb/docs/current/reference/html/>`_ and `Javadoc <http://static.springsource.org/spring-data/data-mongodb/docs/current/api/>`_).

Configuration
-------------

In order to use it in your project, add the following snippet to your pom.xml :

.. code-block:: xml

    <dependency>
        <groupId>org.resthub</groupId>
        <artifactId>resthub-mongodb</artifactId>
        <version>2.x</version>
    </dependency>

In order to import the `default configuration <https://github.com/resthub/resthub-spring-stack/blob/master/resthub-mongodb/src/main/resources/resthubContext.xml>`_, your should activate the resthub-mongodb Spring profile in your WebAppInitializer class:

.. code-block:: java

    XmlWebApplicationContext appContext = new XmlWebApplicationContext();
	appContext.getEnvironment().setActiveProfiles("resthub-mongodb", "resthub-web-server");

You also need to add an applicationContext.xml file in order to scan your repository package.

.. code-block:: xml

	<beans xmlns="http://www.springframework.org/schema/beans"
	       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	       xmlns:mongo="http://www.springframework.org/schema/data/mongo"
	       xsi:schemaLocation="http://www.springframework.org/schema/beans
	                           http://www.springframework.org/schema/beans/spring-beans.xsd
	                           http://www.springframework.org/schema/data/mongo
	                           http://www.springframework.org/schema/data/mongo/spring-mongo.xsd">

	        <mongo:repositories base-package="com.myproject.repository" />

	</beans>

You can customize them by adding a database.properties resource with one or more following keys customized with your values. You should include only the customized ones.

REShub MongoDB default properties are :
	* database.dbname = resthub
	* database.host = localhost
	* database.port = 27017
	* database.connectionsPerHost = 10
	* database.threadsAllowedToBlockForConnectionMultiplier = 5
	* database.connectTimeout = 0
	* database.maxWaitTime = 120000
	* database.autoConnectRetry = false
	* database.socketKeepAlive = false
	* database.socketTimeout = 0
	* database.slaveOk = false
	* database.writeNumber = 0
	* database.writeTimeout = 0
	* database.writeFsync = false

Usage
-----

.. code-block:: java

	public interface TodoRepository extends MongoRepository<Todo, String> {
	    
	    List<Todo> findByContentLike(String content);
	       
	}

Web common
==========

RESThub Web Common comes with built-in XML and JSON support for serialization based on `Jackson 2.1 <http://wiki.fasterxml.com/JacksonHome>`_. RESThub uses `Jackson 2.1 XML capabilities <https://github.com/FasterXML/jackson-dataformat-xml>`_ instead of JAXB since it is more flexible. For example, you don't need to add classes to a context. Please read `Jackson annotation guide <http://wiki.fasterxml.com/JacksonAnnotations>`_ for details about configuration capabilities.

Maven dependency
----------------

In order to use it in your project, add the following snippet to your pom.xml :

.. code-block:: xml

    <dependency>
        <groupId>org.resthub</groupId>
        <artifactId>resthub-web-common</artifactId>
        <version>2.x</version>
    </dependency>

Usage
-----

.. code-block:: java

	// JSON
	SampleResource r = (SampleResource) JsonHelper.deserialize(json, SampleResource.class);
	JsonHelper.deserialize("{\"id\": 123, \"name\": \"Albert\", \"description\": \"desc\"}", SampleResource.class);

	// XML
	SampleResource r = (SampleResource) XmlHelper.deserialize(xml, SampleResource.class);
	XmlHelper.deserialize("<sampleResource><description>desc</description><id>123</id><name>Albert</name></sampleResource>", SampleResource.class);

Web server
==========

RESThub Web Server module is designed for REST webservices development. Both JSON (default) and XML serialization are supported out of the box.

**Warning**: currently Jackson XML dataformat does not support non wrapped List serialization. As a consequence, the findAll (GET /) method is not supported for XML content-type yet. `You can follow the related Jackson issue on GitHub <https://github.com/FasterXML/jackson-dataformat-xml/issues/38>`_.

It provides some abstract REST controller classes, and includes the following dependencies :
	* Spring MVC 3.1 (`reference manual <http://static.springsource.org/spring/docs/3.1.x/spring-framework-reference/html/mvc.html>`_)
	* Jackson 2.1 (`documentation <http://wiki.fasterxml.com/JacksonDocumentation>`_)

RESThub exception resolver allow to map common exceptions (Spring, JPA) to the right HTTP status codes :
	 * IllegalArgumentException -> 400
	 * ValidationException -> 400
	 * NotFoundException, EntityNotFoundException and ObjectNotFoundException -> 404
	 * NotImplementedException -> 501
	 * EntityExistsException -> 409
	 * Any uncatched exception -> 500

Configuration
-------------

In order to use it in your project, add the following snippet to your pom.xml :

.. code-block:: xml

    <dependency>
        <groupId>org.resthub</groupId>
        <artifactId>resthub-web-server</artifactId>
        <version>2.x</version>
    </dependency>

In order to import the `default configuration <https://github.com/resthub/resthub-spring-stack/blob/master/resthub-web/resthub-web-server/src/main/resources/resthubContext.xml>`_, your should activate the resthub-web-server Spring profile in your WebAppInitializer class:

.. code-block:: java

    XmlWebApplicationContext appContext = new XmlWebApplicationContext();
	appContext.getEnvironment().setActiveProfiles("resthub-web-server", "resthub-mongodb");

Usage
-----

RESThub comes with a REST controller that allows you to create a CRUD webservice in a few lines. You have the choice to use a 2 layers (Controller -> Repository) or 3 layers (Controller -> Service -> Repository) software design :

**2 layers software design**

.. code-block:: java

    @Controller @RequestMapping("/repository-based")
	public class SampleRestController extends RepositoryBasedRestController<Sample, Long, WebSampleResourceRepository> {

	    @Override @Inject
	    public void setRepository(WebSampleResourceRepository repository) {
	        this.repository = repository;
	    }

	}

**3 layers software design**

.. code-block:: java

	@Controller @RequestMapping("/service-based")
	public class SampleRestController extends ServiceBasedRestController<Sample, Long, SampleService> {

	    @Override @Inject
	    public void setService(SampleService service) {
	        this.service = service;
	    }

	}

	// and the inject CRUD service
	@Named("sampleService")
	public class SampleServiceImpl extends CrudServiceImpl<Sample, Long, SampleRepository> implements SampleService {

	    @Override @Inject
	    public void setRepository(SampleRepository SampleRepository) {
	        super.setRepository(SampleRepository);
	    }
	}

By default, generic controller use the database identifier (table primary key for JPA on MongoDB ID) in URLs to identify a resource. You can change this behaviour by overriding controller implementations to use the field you want. For example, this is common to use a human readable identifier called reference or slug to identify a resource. You can do that with generic repositories only by overriding findById() controller method :

.. code-block:: java

	@Controller @RequestMapping("/sample")
	public class SluggableSampleController extends RepositoryBasedRestController<Sample, String, SampleRepository> {

	    @Override @Inject
	    public void setRepository(SampleRepository repository) {
	        this.repository = repository;
	    }

	    @Override
	    public Sample findById(@PathVariable String id) {
	        Sample sample = this.repository.findBySlug(id);
	        if (sample == null) {
	            throw new NotFoundException();
	        }
	        return sample;
	    }   
	    
	}

With default behaviour we have URL like GET /sample/32.
With sluggable behaviour we have URL lke GET /sample/niceref.

.. warning::

	Be aware that when you override a Spring MVC controller method, your new method automatically reuse method level annotations from parent classes, but not parameter level annotations. That's why you need to specify parameters annotations again in order to make it work, like in the previous code sample.

Web client
==========

RESThub Web client module aims to give you an easy way to request other REST webservices. It is based on AsyncHttpClient and provides a `client API wrapper <http://jenkins.pullrequest.org/job/resthub-spring-stack-resthub2/javadoc/index.html?org/resthub/web/Client.html>`_ and OAuth2 support.

In order to limit conflicts it has no dependency on Spring, but only on :
 	* AsyncHttpClient `documentation <https://github.com/sonatype/async-http-client>`_ and `Javadoc <http://sonatype.github.com/async-http-client/apidocs/reference/packages.html>`_
 	* Jackson 2.1 (`documentation <http://wiki.fasterxml.com/JacksonDocumentation>`_)

Configuration
-------------

In order to use it in your project, add the following snippet to your pom.xml :

.. code-block:: xml

    <dependency>
        <groupId>org.resthub</groupId>
        <artifactId>resthub-web-client</artifactId>
        <version>2.x</version>
    </dependency>

Usage
-----

You can use resthub web client in a synchronous or asynchronous way. The synchronous API is easy to use, but blocks the current Thread until the remote server sends the full Response.

.. code-block:: java
	
		// One-liner version
		Sample s = httpClient.url("http//...").jsonPost(new Sample("toto")).resource(Sample.class);

		// List<T> and Page<T> use TypeReference due to Java type erasure issue
		List<Sample> p = httpClient.url("http//...").jsonGet().resource(new TypeReference<List<Sample>>() {});
		Page<Sample> p = httpClient.url("http//...").jsonGet().resource(new TypeReference<Page<Sample>>() {});


Asynchronous API is quite the same, every HTTP request returns a `Future <http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/Future.html>`_ <Response> object. Just call get() on this object in order to make the call synchronous.
The ``Future.get()`` method can throw Exceptions, so the method call should be surrounded by a try/catch or let the exceptions bubble up.

.. code-block:: java
	
		// 4 lines example
		Client httpClient = new Client();
		Future<Response> fr = httpClient.url("http//...").asyncJsonPost(new Sample("toto"));
		// do some computation while we're waiting for the response...

		// calling .get() makes the code synchronous again!
		Sample s = httpClient.url("http//...").asyncJsonPost(new Sample("toto")).get().resource(Sample.class);

Because the remote web server sometimes responds 4xx (client error) and 5xx (server error) HTTP status codes, RESThub HTTP Client wraps those error statuses and throws `specific runtime exceptions <https://github.com/resthub/resthub-spring-stack/tree/master/resthub-web/resthub-web-common/src/main/java/org/resthub/web/exception>`_. 

OAuth2.0 integration
--------------------

Here is an example of a simple OAuth2 support

.. code-block:: java

    String username = "test";
    String password = "t&5t";
    String clientId = "app1";
    String clientSecret = "";
    String accessTokenUrl = "http://.../oauth/token";

    Client httpClient = new Client().setOAuth2(username, password, accessTokenUrl, clientId, clientSecret);
    String result = httpClient.url("http://.../api/sample").get().getBody();

You can also use a specific OAuth2 configuration. For example, you can override the HTTP Header
used to send the OAuth token.

.. code-block:: java

    OAuth2Config.Builder builder = new OAuth2Config.Builder();
    builder.setAccessTokenEndpoint("http://.../oauth/token")
      .setUsername("test").setPassword("t&5t")
      .setClientId("app1").setClientSecret("")
      // override default OAuth HTTP Header name
      .setOAuth2Scheme("OAuth");

    Client httpClient = new Client().setOAuth2Builder(builder);
    String result = httpClient.url("http://.../api/sample").get().getBody();
 
Testing
=======
	
The following test stack is included in the RESThub test module :
	* Test framework with `TestNG <http://testng.org/doc/documentation-main.html>`_. If you use Eclipse, don't forget to install the `TestNG plugin <http://testng.org/doc/eclipse.html>`_.
	* Assertion with `Fest Assert 2 <https://github.com/alexruiz/fest-assert-2.x/wiki>`_
	* Mock with `Mockito <http://code.google.com/p/mockito/>`_

RESThub also provides generic classes in order to make testing easier.
   * AbstractTest : base class for your non transactional Spring aware unit tests
   * AbstractTransactionalTest : base class for your transactional unit tests, preconfigured with Spring test framework
   * AbstractWebTest : base class for your unit tests that need to run and embedded servlet container

Maven dependency
----------------

In order to use it in your project, add the following snippet to your pom.xml :

.. code-block:: xml

    <dependency>
        <groupId>org.resthub</groupId>
        <artifactId>resthub-test</artifactId>
        <version>2.x</version>
        <scope>test</scope>
    </dependency>

Data provisioning and cleanup
------------------------------

It is recommended to initialize and cleanup test data shared by your tests using methods annotated with TestNG's @BeforeMethod and @AfterMethod and using your repository or service classes.

**Warning:** : with JPA the default deleteAll() method does not manage cascade delete, so for your data cleanup you should use the following code in order to get your entities removed with cascade delete support:

.. code-block:: java

	Iterable<MyEntity> list = repository.findAll();
	for (MyEntity entity : list) {
		repository.delete(entity);
	}

Usage
-----

AbstractTest or AbstractTransactionalTest

.. code-block:: java

	@ActiveProfiles("resthub-jpa")
	public class SampleRepositoryTest extends AbstractTransactionalTest {

	    private SampleRepository repository;

	    @Inject
	    public void setRepository(SampleRepository repository) {
	        this.repository = repository;
	    }

	    @AfterMethod
	    public void tearDown() {
	        for (SampleRepository resource : repository.findAll()) {
	            repository.delete(resource);
	        }
	    }

	    @Test
	    public void testSave() {
	        Sample entity = repository.save(new Sample());
	        Assertions.assertThat(repository.exists(entity.getId())).isTrue();
	    }
	}

AbstractWebTest

.. code-block:: java

	public class SampleRestControllerTest extends AbstractWebTest {

	    public SampleRestControllerTest() {
        	// Call AbstractWebTest(String profiles) constructor
        	super("resthub-web-server,resthub-jpa");
    	}   
	    
	    // Cleanup after each test
	    @AfterMethod
	    public void tearDown() {
            this.request("sample").delete();
	    }

	    @Test
	    public void testCreateResource() {
	        Sample r = this.request("sample").jsonPost(new Sample("toto")).resource(Sample.class);
	        Assertions.assertThat(r).isNotNull();
	        Assertions.assertThat(r.getName()).isEqualTo("toto");
	    }
	    
	}

A sample assertion

.. code-block:: java

	Assertions.assertThat(result).contains("Albert");

Spring MVC Router
=================

Spring MVC Router adds route mapping capacity to any "Spring MVC based" webapp à la PlayFramework or Ruby on Rails. For more details, check its `detailed documentation <http://resthub.github.com/springmvc-router/>`_.

