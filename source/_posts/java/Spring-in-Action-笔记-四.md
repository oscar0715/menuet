---
title: Spring_in_Action_笔记(四)
tags:
  - Spring
  - MVC
  - Java
categories:
  - Technology
  - Java
date: 2016-05-31 10:38:22
---
如题，阅读《Spring_in_Action》的笔记

(四)Spring on the web
In this chapter, we’ll explore the essentials of Spring’s MVC web framework. We’ll focus on using annotations to create controllers that handle various kinds of web requests, parameters, and form input.

<!-- more -->

***
### Getting started with Spring MVC

#### life of the request
The first stop in the request’s travels is at Spring’s `DispatcherServlet`.

A `front controller` is a common web application pattern where a single servlet delegates responsibility for a request to other components of an application to perform actual processing. In the case of Spring MVC, DispatcherServlet is the front controller.

A `controller` is a Spring component that `processes the request`.

A typical application may have several controllers, and DispatcherServlet needs some help `deciding which controller` to send the request to. 

So the DispatcherServlet consults one or more `handler mappings` to figure out where the request’s next stop will be. The handler mapping pays particular attention to the `URL` carried by the request when making its decision.

Once an appropriate controller has been chosen, DispatcherServlet sends the request on its merry way to the chosen controller。
>merry way 愉快的旅途 :）

At the controller, the request drops off its `payload` (the information submitted by the user) and patiently `waits` while the controller processes that information.

(Actually, a well-designed controller performs little or no processing itself and instead delegates responsibility for the business logic to one or more `service objects`.)

The logic performed by a controller often results in some `information` that needs to be carried back to the user and displayed in the browser. This information is referred to as the `model`.

But sending `raw information` back to the user isn’t sufficient—it needs to be formatted in a user-friendly `format`, typically HTML. For that, the information needs to be given to a `view`, typically a JavaServer Page (JSP).

One of the last things a controller does is package up the model data and identify the name of a view that should render the output.

It then sends the request, along with the `model and view name`, back to the DispatcherServlet.

So that the controller doesn’t get coupled to a particular view,  it only carries a `logical name` that will be used to look up the actual view that will produce the result.

The DispatcherServlet consults a `view resolver` to map the logical view name to a specific view implementation.

Its final stop is at the view implementation, typically a JSP, where it delivers the model data.

The view will use the model data to render output that will be carried back to the client by the (not- so-hardworking) response object.


#### CONFIGURING DISPATCHERSERVLET
When DispatcherServlet starts up, it creates a `Spring application context` and starts `loading it with beans` declared in the configuration files or classes that it’s given.

getServletConfigClasses():
With the `getServletConfigClasses()` method in listing 5.1, you’ve asked that DispatcherServlet load its application context with beans defined in the WebConfig configura- tion class (using Java configuration).

But in Spring web applications, there’s often another application context. This other application context is `created` by `ContextLoaderListener`.

Whereas DispatcherServlet is expected to load beans containing `web components` such as `controllers`, `view resolvers`, and `handler mappings`, ContextLoaderListener is expected to load the `other beans in your application`. These beans are typically the middle-tier and data-tier components that drive the back end of the application.

The @Configuration classes returned from `getServletConfigClasses()` will define beans for `DispatcherServlet’s application context`. 
Meanwhile, the @Configuration class’s returned `getRootConfigClasses()` will be used to configure the application context created by `ContextLoaderListener`.

It’s important to realize that configuring DispatcherServlet via AbstractAnnotationConfigDispatcherServletInitializer is an alternative to the traditional web.xml file.
