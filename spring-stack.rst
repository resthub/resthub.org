============
Spring Stack
============

Here is the description of each RESThub modules, with related dependencies, code snippets and documentation.

The whole RESThub 2.0 Spring stack `Javadoc <http://jenkins.pullrequest.org/job/resthub-spring-stack-master/javadoc/>`_ is available.

Project layout
==============

Let's take a look at a typical RESThub based application...

RESThub stack based projects follow the "Maven standard" project layout :
	* /pom.xml : the Maven configuration file which defines dependencies, plugins, etc.
	* /src/main/java : your java classes go there
	* /src/main/resources : your xml and properties files go there
	* /src/main/resources/applicationContext.xml : this is your Spring application configuration file. Since we mainly use annotation based configuration, this file will usually be short
	* /src/main/webapp : your HTML, CSS and javascript files go there
 
In bigger projects, functionalities are divided in several JAR modules (customer management, product management ...). Those modules are included as dependencies in a WAR module.  

Common
======

Maven 3.0 (`complete reference <http://www.sonatype.com/books/mvnref-book/reference/public-book.html>`_) is the reference build tool used.

RESThub 2 based applications are designed to use Java 7 for both compile and runtime.

RESThub defines a set of common dependencies for all RESThub modules except web client :	
 	* Spring 3.1 (`reference manual <http://static.springsource.org/spring/docs/3.1.x/spring-framework-reference/html>`_ and `Javadoc <http://static.springsource.org/spring/docs/3.1.x/javadoc-api/>`_)
 	* SQL and NoSQL Persistence with `Spring Data <http://www.springsource.org/spring-data>`_
 	* Logging with SLF4J (`manual <http://www.slf4j.org/manual.html>`_) and Logback (`manual <http://logback.qos.ch/manual/index.html>`_)

Configuration
-------------

You will find below the typical configuration file for your application.

pom.xml
~~~~~~~

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
			<resthub.spring.stack.version>2.0-beta2</resthub.spring.stack.version>
			<resthub.backbone.stack.version>2.0-beta2</resthub.backbone.stack.version>
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
					<version>2.4</version>
					<configuration>
						<encoding>UTF-8</encoding>
						<source>1.7</source>
						<target>1.7</target>
					</configuration>
				</plugin>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-resources-plugin</artifactId>
					<version>2.5</version>
					<configuration>
						<encoding>UTF-8</encoding>
					</configuration>
				</plugin>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-war-plugin</artifactId>
					<version>2.1.1</version>
					<configuration>
						<failOnMissingWebXml>false</failOnMissingWebXml>
					</configuration>
				</plugin>
				<plugin>
					<groupId>org.mortbay.jetty</groupId>
					<artifactId>jetty-maven-plugin</artifactId>
					<version>8.1.3.v20120416</version>
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

applicationContext.xml
~~~~~~~~~~~~~~~~~~~~~~

By default RESThub webservices and unit tests scan and automatically include all applicationContext.xml files (your application context files) available in your application classpath, including its dependencies.

RESThub default context files should be included thanks to <import /> elements. The name of each module RESThub context are specified in the configuration section of each module documentation.

Here is an example of a a RESThub based typical src/main/resources/applicationContext.xml (this one use MongoDB, adapt it if you use JPA) :

.. code-block:: xml

	<beans xmlns="http://www.springframework.org/schema/beans" 
	       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	       xmlns:context="http://www.springframework.org/schema/context"
	       xmlns:mongo="http://www.springframework.org/schema/data/mongo"
	       xmlns:mvc="http://www.springframework.org/schema/mvc"
	       xsi:schemaLocation="http://www.springframework.org/schema/beans
	                           http://www.springframework.org/schema/beans/spring-beans.xsd
	                           http://www.springframework.org/schema/context
	                           http://www.springframework.org/schema/context/spring-context.xsd
	                           http://www.springframework.org/schema/data/mongo
	                           http://www.springframework.org/schema/data/mongo/spring-mongo.xsd
	                           http://www.springframework.org/schema/mvc
	                           http://www.springframework.org/schema/mvc/spring-mvc.xsd">
	
        <!-- Default Spring MVC configuration for JSON + XML webservices -->
	    <import resource="classpath*:resthubWebServerContext.xml" />

	    <!-- Default JPA conguration -->
	    <import resource="classpath*:resthubJpaContext.xml" />
	
	    <!-- Scan your services and controllers -->
	    <context:component-scan base-package="com.mycompany.myproject" />

	    <!-- Create your repositories implementation from their interface -->
	    <mongo:repositories base-package="com.mycompany.myproject" />
	
	</beans>

If some functionalities are not intended to be always used, you may customize your context filename (for example securityDisabledContext.xml) and customize your unit tests and WebApplicationInitializer in order to specify it, for example annotate your unit test with :

.. code-block:: java
	
	@ContextConfiguration(locations = { "classpath*:resthubContext.xml", "classpath*:applicationContext.xml", "classpath*:securityDisabledContext.xml" })

And modify your WebApplicationInitializer

.. code-block:: java

	String[] locations = {"classpath*:resthubContext.xml", "classpath*:applicationContext.xml"};
	appContext.setConfigLocations(locations);

It is a good practice to always prefix the filename by "classpath*:"" in order to enable scanning in all the classathes of your applications.

logback.xml
~~~~~~~~~~~

You usually also will have a src/main/resources/logback.xml file in order to configure logging :

.. code-block:: xml

	<configuration> 
		<appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender"> 
			<layout class="ch.qos.logback.classic.PatternLayout"> 
				<Pattern>%d [%thread] %level %logger - %m%n</Pattern> 
			</layout> 
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

Or to inject a bean by name (more specific than injection by type):

.. code-block:: java

   @Inject @Named("beanName")
   public void setSampleProperty(...) {
   
   }

CRUD services
-------------

RESThub is designed to give you the choice between 2 layers (Controller -> Repository) or 3 layers (Controller -> Service -> Repository)  software design. If you choose the 3 layer software design, you can use the RESThub CRUD service when it is accurate :

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

There are various ways to configure your environment specific properties in your application: the one described below is the most simple and flexible way we have found to do it. 

Maven filtering (search and replace variables) is not recommended because it is done at compile time (not runtime) and makes usually your JAR/WAR specific to an environment. This feature can be useful when defining your target path (${project.build.directory}) in your src/test/applicationContext.xml for testing purpose.

Spring properties placeholders allow you to reference in your application context files some values defined in external properties. This is useful in order to keep your application context generic (located in src/main/resources or src/test/resources), and put all values that depend on the environment (local, dev, staging, production) in external properties. These dynamic properties values are resolved during application startup.

In order to improve testabilty and extensibility of your modules, you should set default values in case no properties are found in the classpath - if properties are found, then default values are obviously overridden. It is achieved by declaring the following lines in your applicationContext.xml :

.. code-block:: xml

   <context:property-placeholder location="classpath*:mymodule.properties"
                                 properties-ref="databaseProperties"
                                 ignore-resource-not-found="true"
                                 ignore-unresolvable="true" />

   <util:properties id="mymoduleProperties" >
      <prop key="param1">param1Value</prop>
      <prop key="param2">param2Value</prop>
   </util:properties>

You should now be able to inject dynamic values in your beans :

.. code-block:: xml

   <bean id="sampleBean" class="org.mycompany.MyBean">
      <property name="property1" value="${param1}"/>
      <property name="property2" value="${param2}"/>
   </bean>

You can also inject direcly these values in your Java classes thanks to the @Value annotation :

.. code-block:: java

   @Value("${param1}")
   protected String property1;

Or :

.. code-block:: java

   @Value("${param1}")
   protected void setProperty1(String property1) {
      this.property1 = property1;
   }

Disable context XSD validation
------------------------------

By default, Spring validates XML schemas declared in your application context. Depending on the schemas used, this validation could prevent you to use properties placeholder described previously, because you will put a value like ${paramStatus} in a boolean attribute that can take only true or false value.

Since there is no way to fix that in vanilla Spring, RESThub provides a way to disable application context XSD validations.

In order to disable validation in your unit tests, annotate your test classes with :

.. code-block:: java

   @ContextConfiguration(loader = ResthubXmlContextLoader.class)

In order to disable validation in your web application, you should declare it in your WebApplicationInitializer instead of the XmlWebApplicationContext (ResthubXmlWebApplicationContex is located in resthub-web-server dependency) :

.. code-block:: java

	@Override
    public void onStartup(ServletContext servletContext) throws ServletException {
                
        ResthubXmlWebApplicationContext appContext = new ResthubXmlWebApplicationContext();
        // ...

    }

JPA support
===========

JPA support is based on Spring Data JPA and includes by default the H2 in memory database and is based on the following dependencies :
	 	* Spring Data JPA (`reference manual <http://static.springsource.org/spring-data/data-jpa/docs/current/reference/html/>`_ and `Javadoc <http://static.springsource.org/spring-data/data-jpa/docs/current/api/>`_)
	 	* Hibernate `documentation <http://www.hibernate.org/docs.html>`_
	 	* `H2 embedded database <http://www.h2database.com/html/main.html>`_

Thanks to Spring Data, it is possible to create Repositories (also sometimes named DAO) by writing only the interface.

Entity scan
-----------

Spring 3.1 allows to scan entities in different modules using the same PersitenceUnit, which is not possible with default JPA behaviour. You have to specify the packages where Spring should scan your entities by creating a database.properties file in your src/main/resources folder, with the following content :


.. code-block:: properties

   persistenceUnit.packagesToScan = com.myproject.model

Now, entities within the com.myproject.model packages will be scanned.

Configuration
-------------

In order to import default configuration, your should add the following line in your applicationContext.xml :

 .. code-block:: xml

    <import resource="classpath*:resthubJpaContext.xml" />

resthubJpaContext.xml defines some default values. You can customize them by adding a database.properties in src/main/resources with one or more of the following keys customized with your values. You should include only the customized ones.

REShub JPA default properties are :
	* dataSource.driverClassName = org.h2.Driver
	* dataSource.url = jdbc:h2:mem:resthub;DB_CLOSE_DELAY=-1
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

 If you need to do more advanced configuration, just override dataSource and entityManagerFactory beans in your applicationContext.xml file like below :

 .. code-block:: xml

	<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
		<property name="xxxx" value="..." />
	</bean>

	<bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
		<property name="xxxx" value="..." />
	</bean>

Usage
-----

.. code-block:: java

	@Repository
	public interface TodoRepository extends JpaRepository<Todo, String> {
	    
	    List<Todo> findByContentLike(String content);
	       
	}

You also need to add an applicationContext.xml file in order to scan your repository package.

.. code-block:: xml

	<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	       xmlns:jpa="http://www.springframework.org/schema/data/jpa"
	       xsi:schemaLocation="http://www.springframework.org/schema/beans
	                           http://www.springframework.org/schema/beans/spring-beans.xsd
	                           http://www.springframework.org/schema/data/jpa
	                           http://www.springframework.org/schema/data/jpa/spring-jpa.xsd">

        <import resource="classpath*:resthubJpaContext.xml" />

	    <jpa:repositories base-package="com.myproject.repository" />

	</beans>

Maven dependency
----------------

In order to use it in your project, add the following snippet to your pom.xml :

.. code-block:: xml

	<dependency>
		<groupId>org.resthub</groupId>
		<artifactId>resthub-jpa</artifactId>
		<version>2.0-beta2</version>
	</dependency>

MongoDB support
===============

MongoDB support is based on Spring Data MongoDB :
	* Spring Data MongoDB `reference manual <http://static.springsource.org/spring-data/data-mongodb/docs/current/reference/html/>`_ and `Javadoc <http://static.springsource.org/spring-data/data-mongodb/docs/current/api/>`_

Configuration
-------------

In order to import default configuration, your should add the following line in your applicationContext.xml :

 .. code-block:: xml

    <import resource="classpath*:resthubMongodbContext.xml" />

resthubMongodbContext.xml defines some default values. You can customize them by adding a database.properties in src/main/resources with one or more following keys customized with your values. You should include only the customized ones.

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

	@Repository
	public interface TodoRepository extends MongoRepository<Todo, String> {
	    
	    List<Todo> findByContentLike(String content);
	       
	}

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

Maven dependency
----------------

In order to use it in your project, add the following snippet to your pom.xml :

.. code-block:: xml

	<dependency>
		<groupId>org.resthub</groupId>
		<artifactId>resthub-mongodb</artifactId>
		<version>2.0-beta2</version>
	</dependency>

Web Common
==========

RESThub Web Common comes with built-in XML and JSON support for serialization based on `Jackson 2.0 <http://wiki.fasterxml.com/JacksonHome>`_. RESThub uses `Jackson 2.0 XML capabilities <https://github.com/FasterXML/jackson-dataformat-xml>`_ instead of JAXB since it is more flexible. For example, you don't need to add classes your need to a context. Please read `Jackson annotation guide <http://wiki.fasterxml.com/JacksonAnnotations>`_ for details about configuration capabilities.

Usage
-----

.. code-block:: java

	// JSON
	SampleResource r = (SampleResource) JsonHelper.deserialize(json, SampleResource.class);
	JsonHelper.deserialize("{\"id\": 123, \"name\": \"Albert\", \"description\": \"desc\"}", SampleResource.class);

	// XML
	SampleResource r = (SampleResource) XmlHelper.deserialize(xml, SampleResource.class);
	XmlHelper.deserialize("<sampleResource><description>desc</description><id>123</id><name>Albert</name></sampleResource>", SampleResource.class);

Maven dependency
----------------

In order to use it in your project, add the following snippet to your pom.xml :

.. code-block:: xml

	<dependency>
		<groupId>org.resthub</groupId>
		<artifactId>resthub-web-common</artifactId>
		<version>2.0-beta2</version>
	</dependency>

Web server
==========

RESThub Web Server module is designed to allow you to develop REST webservices. Both JSON (default) and XML serialization are supported out of the box.

**Warning**: currently Jackson XML dataformat does not support non wrapped List serialization. As a consequence, the findAll (GET /) method is not supported for XML content type yet. `You can follow the related Jackson issue on GitHub <https://github.com/FasterXML/jackson-dataformat-xml/issues/6>`_.

It provides some abstract REST controller classes, and includes the following dependencies :
	* Spring MVC 3.1 (`reference manual <http://static.springsource.org/spring/docs/3.1.x/spring-framework-reference/html/mvc.html>`_)
	* Jackson 2.0 (`documentation <http://wiki.fasterxml.com/JacksonDocumentation>`_)

Configuration
-------------

In order to import default configuration, your should add the following line in your applicationContext.xml :

 .. code-block:: xml

    <import resource="classpath*:resthubWebServerContext.xml" />

RESThub 2 based web applications do not contain web.xml files, but use Servlet 3.0 and Spring 3.1 new capabilities in order to initialize your webapp with a Java class and extend WebApplicationInitializer. This class just need to be in the classpath, here is the default one (the RESThub archetypes can create it for you if needed) :

.. code-block:: java
	
	public class WebAppInitializer implements WebApplicationInitializer {

	    @Override
	    public void onStartup(ServletContext servletContext) throws ServletException {
	                
	        XmlWebApplicationContext appContext = new XmlWebApplicationContext();
	        String[] locations = {"classpath*:resthubContext.xml", "classpath*:applicationContext.xml"};
	        appContext.setConfigLocations(locations);

	        ServletRegistration.Dynamic dispatcher = servletContext.addServlet("dispatcher", new DispatcherServlet(appContext));
	        dispatcher.setLoadOnStartup(1);
	        dispatcher.addMapping("/*");
	        
	        servletContext.addListener(new ContextLoaderListener(appContext));

	    }
	}

Usage
-----

RESThub comes with a REST controller that allows you to create a CRUD webservice in a few lines. You have the choice to use 2 layers (Controller -> Repository) or 3 layers (Controller -> Service -> Repository) software design :

**2 layers software design**

.. code-block:: java

    @Controller @RequestMapping("/repository-based")
	public class SampleRestController extends RepositoryBasedRestController<Sample, Long, WebSampleResourceRepository> {

	    @Override @Inject
	    public void setRepository(WebSampleResourceRepository repository) {
	        this.repository = repository;
	    }

	    @Override
	    public Long getIdFromResource(Sample resource) {
	        return resource.getId();
	    }

	}

**3 layers software design**

.. code-block:: java

	// REST Controller
	@Controller @RequestMapping("/service-based")
	public class SampleRestController extends ServiceBasedRestController<Sample, Long, WebSampleResourceService> {

	    @Override @Inject @Named("webSampleResourceService")
	    public void setService(WebSampleResourceService service) {
	        this.service = service;
	    }

	    @Override
	    public Long getIdFromResource(Sample webSampleResource) {
	        return webSampleResource.getId();
	    }
	}

	// and the inject CRUD service
	@Named("webSampleResourceService")
	public class WebSampleResourceServiceImpl extends CrudServiceImpl<Sample, Long, WebSampleResourceRepository>
        implements WebSampleResourceService {

	    @Override @Inject
	    public void setRepository(WebSampleResourceRepository webSampleResourceRepository) {
	        super.setRepository(webSampleResourceRepository);
	    }
	}

Maven dependency
----------------

In order to use it in your project, add the following snippet to your pom.xml :

.. code-block:: xml

	<dependency>
		<groupId>org.resthub</groupId>
		<artifactId>resthub-web-server</artifactId>
		<version>2.0-beta2</version>
	</dependency>

Web client
==========

RESThub Web client module goal is to give you an easy way to request other REST webservices. It is based on AsyncHttpClient and provides a `client API wrapper <http://jenkins.pullrequest.org/job/resthub-spring-stack-resthub2/javadoc/index.html?org/resthub/web/Client.html>`_ and a OAuth2 support.

In order to limit conflicts it has no dependency on Spring, but only on :
 	* AsyncHttpClient `documentation <https://github.com/sonatype/async-http-client>`_ and `Javadoc <http://sonatype.github.com/async-http-client/apidocs/reference/packages.html>`_
 	* Jackson 2.0 (`documentation <http://wiki.fasterxml.com/JacksonDocumentation>`_)

Usage
-----

You can use resthub web client in a synchronous or asynchronous way. The API is the same, every Http request returns a `Future <http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/Future.html>`_ <Response> object. Just call get() on this object in order to make the call synchronous.

.. code-block:: java
	
		// 3 line example
		Client httpClient = new Client();
		Future<Response> fr = httpClient.url("http//...").jsonPost(new Sample("toto"));
		Response r = fr.get();
		Sample s = r.resource(Sample.class);

		// Same but in a one line
		Sample s = httpClient.url("http//...").jsonPost(new Sample("toto")).get().resource(Sample.class);

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
    String result = httpClient.url("http://.../api/sample").get().get().getBody();

You can also configure a specific OAuth2 configuration. For example, you can override the HTTP Header
used to send the OAuth token.

.. code-block:: java

    OAuth2Config.Builder builder = new OAuth2Config.Builder();
    builder.setAccessTokenEndpoint("http://.../oauth/token")
      .setUsername("test").setPassword("t&5t")
      .setClientId("app1").setClientSecret("")
      // override default OAuth HTTP Header name
      .setOAuth2Scheme("OAuth");

    Client httpClient = new Client().setOAuth2Builder(builder);
    String result = httpClient.url("http://.../api/sample").get().get().getBody();

Maven dependency
----------------

In order to use it in your project, add the following snippet to your pom.xml :

.. code-block:: xml

	<dependency>
		<groupId>org.resthub</groupId>
		<artifactId>resthub-web-client</artifactId>
		<version>2.0-beta2</version>
	</dependency>
 
Testing
=======
	
The following test stack is included in the RESThub test module :
	* Test framework with `TestNG <http://testng.org/doc/documentation-main.html>`_. If you use Eclipse, don't forget to install the `TestNG plugin <http://testng.org/doc/eclipse.html>`_.
	* Assertion with `Fest Assert 2 <https://github.com/alexruiz/fest-assert-2.x/wiki>`_
	* Mock with `Mokito <http://code.google.com/p/mockito/>`_
	* Webapp testing with `FluentLenium <http://www.fluentlenium.org/>`_

RESThub also provides generic classes in order to make testing easier.
   * AbstractTest : base class for your non transactional Spring aware unit tests
   * AbstractTransactionalTest : base class for your transactional unit tests, preconfigure Spring test framework
   * AbstractWebTest : base class for your unit test that need to run and embedded servlet container

Data provisioning and cleanup
------------------------------

It is recommended to initialize and cleanup test data common to all your tests thanks to methods with TestNG annotations @BeforeMethod and @AfterMethod and using your repository or service classes.

**Warning:** : with JPA the default deleteAll() method does not manage cascade delete, so for your data cleanup you should use the following code in order to get your entities removed with cascade delete support:

.. code-block:: java

	Iterable<MyEntity> list = repository.findAll();
	for (MyEntity entity : list) {
		repository.delete(entity);
	}

Usage
-----

A sample REST webservice test

.. code-block:: java

	public class SampleRestControllerTest extends AbstractWebTest {

	    protected String rootUrl() {
	        return "http://localhost:9797/api/sample";
	    }    
	    
	    // Cleanup after each test
	    @AfterMethod
	    public void tearDown() {
	    	try (Client httpClient = new Client()) {
	            httpClient.url(rootUrl()).delete().get();
	        } catch (InterruptedException | ExecutionException e) {
	            Assertions.fail("Exception during delete all request", e);
	        }
	    }

	    @Test
	    public void testCreateResource() throws IllegalArgumentException, InterruptedException, ExecutionException, IOException {
	        Sample r = new Sample("toto");
	        Client httpClient = new Client()
	        Response response = httpClient.url(rootUrl()).jsonPost(r).get();
	        r = (Sample)response.resource(r.getClass());
	        Assertions.assertThat(r).isNotNull();
	        Assertions.assertThat(r.getName()).isEqualTo("toto");
	    }
	    
	}

A sample assertion

.. code-block:: java

	Assertions.assertThat(result).contains("Albert");

Maven dependency
----------------

In order to use it in your project, add the following snippet to your pom.xml :

.. code-block:: xml

	<dependency>
		<groupId>org.resthub</groupId>
		<artifactId>resthub-test</artifactId>
		<version>2.0-beta2</version>
		<scope>test</scope>
	</dependency>

Spring MVC Router
=================

Spring MVC Router adds route mapping capacity to any "Spring MVC based" webapp à la PlayFramework or Ruby on Rails. For more details, check its `detailed documentation <https://github.com/resthub/springmvc-router>`_.

