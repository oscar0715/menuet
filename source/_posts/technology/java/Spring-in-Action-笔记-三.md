---
title: Spring_in_Action_笔记(三)
tags:
  - Spring
  - MVC
  - Java
categories:
  - Technology
  - Java
date: 2016-05-23 10:30:43
---
如题，阅读《Spring_in_Action》的笔记

(三)Aspect-oriented Spring
<!-- more -->

***
### What is AOP
As stated earlier, aspects help to modularize cross-cutting concerns. 

In short, a cross- cutting concern can be described as any functionality that affects multiple points of an application. 

With AOP, you define the common functionality in one
place, but you can declaratively define how and where this functionality is applied without having to modify the class to which you’re applying the new feature. Cross- cutting concerns can now be modularized into special classes called aspects. 

This has two benefits. 
- First, the logic for each concern is in one place, as opposed to being scat- tered all over the code base. 
- Second, your service modules are cleaner because they only contain code for their primary concern (or core functionality), and secondary concerns have been moved to aspects.

### AOP Terminology
1. Advice
Aspects have a purpose—a job they’re meant to do. In AOP terms, the job of an aspect is called advice.

2. Join Points
A join point is a point in the execution of the application where an aspect can be plugged in.
This point could be a method being called, an exception being thrown, or even a field being modified. 
These are the points where your aspect’s code can be inserted into the normal flow of your application to add new behavior.

3. Pointcuts
If advice defines the what and when of aspects, then pointcuts define the where. A pointcut definition matches one or more join points at which advice should be woven.

4. Aspects
An aspect is the merger of advice and pointcuts. Taken together, advice and point- cuts define everything there is to know about an aspect—what it does and where and when it does it.

5. Introductions
An introduction allows you to add new methods or attributes to existing classes.

6. Weaving
Weaving is the process of applying aspects to a target object to create a new proxied object.

### Spring's AOP support
The ability to create pointcuts that define the join points at which aspects should be woven is what makes it an AOP framework. 

#### SPRING ADVICE IS WRITTEN IN JAVA
All the advice you create in Spring is written in a standard Java class. 

The pointcuts that define where advice should be applied may be specified with annotations or configured in a Spring XML configuration.

#### SPRING ADVISES OBJECTS AT RUNTIME
In Spring, aspects are woven into Spring-managed beans at runtime by wrapping them with a proxy class. 

Spring doesn’t create a proxied object until that proxied bean is needed by the application. 

If you’re using an ApplicationContext, the proxied objects will be cre- ated when it loads all the beans from the BeanFactory. 

#### SPRING ONLY SUPPORTS METHOD JOIN POINTS
Because it’s based on dynamic proxies, Spring only supports method join points. 

Spring’s lack of field pointcuts prevents you from creating very fine-grained advice, such as intercepting updates to an object’s field. And without constructor pointcuts, there’s no way to apply advice when a bean is instantiated.

### Selecting join points with pointcuts

#### Annotating introductions
Using an AOP concept known as introduction, aspects can attach new methods to Spring beans.

    此处报了一个错
    Maven: Failed to read artifact descriptor
    解决：
    You can always try mvn -U clean install
    -U Forces a check for updated releases and snapshots on remote repositories

    @Aspect
    public class EncoreableIntroducer {
      @DeclareParents(value="concert.Performance+",
                      defaultImpl=DefaultEncoreable.class)
      public static Encoreable encoreable;
    }

The @DeclareParents annotation is made up of three parts:
1. The value attribute identifies the kinds of beans that should be introduced with the interface. In this case, that’s anything that implements the Performance interface. (The plus sign at the end specifies any subtype of Performance, as opposed to Performance itself.)
2. The defaultImpl attribute identifies the class that will provide the implemen- tation for the introduction. Here you’re saying that DefaultEncoreable will provide that implementation.
3. The static property that is annotated by @DeclareParents specifies the inter- face that’s to be introduced. In this case, you’re introducing the Encoreable interface.

### Declaring aspects in XML
XML略过不看了

### Injecting AspectJ aspects
Spring 不够就用 AspectJ
