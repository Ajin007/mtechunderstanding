## what is Spring bean ?
~~~
1. Spring, a bean is an object that the Spring container instantiates, assembles, and manages.
~~~
## what is ApplicationContext?
~~~
1. the primary job of the ApplicationContext is to manage beans.
2. resolving messages, supporting internationalization, publishing events, and application-layer specific contexts
3. In the Spring framework, the interface ApplicationContext represents the IoC container.
  The Spring container is responsible for instantiating, configuring and assembling objects known as beans, as well as managing their life cycles.
4. The Spring framework provides several implementations of the ApplicationContext interface:
      A)---> AnnotationConfigApplicationContext
      B)---> ClassPathXmlApplicationContext and FileSystemXmlApplicationContext for standalone applications
      C)---> WebApplicationContext for web applications.
~~~
## what is BeanFactory ?
~~~
1. BeanFactory is the root interface for accessing the spring container.
2. 
~~~
## what is IOC Container ?
~~~
Inversion of Control is a principle in software engineering which transfers the control of objects or portions of a program to a container or framework. We most often use it in the context of object-oriented programming.

In contrast with traditional programming, in which our custom code makes calls to a library, IoC enables a framework to take control of the flow of a program and make calls to our custom code. To enable this, frameworks use abstractions with additional behavior built in. If we want to add our own behavior, we need to extend the classes of the framework or plugin our own classes.

The advantages of this architecture are:

decoupling the execution of a task from its implementation
making it easier to switch between different implementations
greater modularity of a program
greater ease in testing a program by isolating a component or mocking its dependencies, and allowing components to communicate through contracts
We can achieve Inversion of Control through various mechanisms such as: Strategy design pattern, Service Locator pattern, Factory pattern, and Dependency Injection (DI).


~~~
## what is DI ?
~~~
~~~
## what is the use of @Configuration and the @Bean ?
~~~
1.  The @Bean annotation on a method indicates that the method creates a Spring bean.
2. Moreover, a class annotated with @Configuration indicates that it contains Spring bean configurations.
~~~
## Simple Project for understanding
~~~
https://medium.com/@hewage.d.sampath/building-the-first-maven-project-using-spring-framework-85d611d7d6da
~~~
