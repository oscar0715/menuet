---
title: Spring_in_Action_笔记(一)
tags:
  - Spring
  - MVC
  - Java
categories:
  - Technology
  - Java
date: 2016-05-20 17:33:01
---
如题，阅读《Spring_in_Action》的笔记

(一)Introduction
介绍 injection dependency, wiring, aspect oriented programming等概念
<!-- more -->

***

## Injection dependency
### BraveKnight类
{% codeblock  lang:Java %}
package com.springinaction.knights;
public class BraveKnight implements Knight {
    private Quest quest;
    public BraveKnight(Quest quest) {
        this.quest = quest;
    }
    public void embarkOnQuest() {
        quest.embark();
    } 
}
{% endcodeblock %}

As you can see, BraveKnight, unlike DamselRescuingKnight, doesn’t create his own quest. Instead, he’s given a quest at construction time as a constructor argument. This is a type of DI known as constructor injection.

What’s more, the quest he’s given is typed as Quest, an interface that all quests implement. 

The point is that BraveKnight isn’t coupled to any specific implementation of Quest. It doesn’t matter to him what kind of quest he’s asked to embark on, as long as it implements the Quest interface. That’s the key benefit of DI—loose coupling.

If an object only knows about its dependencies by their interface (not by their implementation or how they’re instantiated), then the dependency can be swapped out with a different implementation without the depending object knowing the difference. One of the most common ways a dependency is swapped out is with a mock imple- mentation during testing.

### mock Quest
{% codeblock  lang:Java %}
package com.springinaction.knights;
import static org.mockito.Mockito.*;
import org.junit.Test;
public class BraveKnightTest {
    @Test
    public void knightShouldEmbarkOnQuest() {
        Quest mockQuest = mock(Quest.class);
        BraveKnight knight = new BraveKnight(mockQuest);
        knight.embarkOnQuest();
        // ask Mockito to verify that the mock Quest’s embark() method was called exactly once.
        verify(mockQuest, times(1)).embark();
    } 
}
{% endcodeblock %}

### wiring 
The act of creating associations between application components is commonly referred to as wiring.
In Spring, there are many ways to wire components together, but a common approach has always been via XML.
If XML configuration doesn’t suit your tastes, you might like to know that Spring also allows you to express configuration using Java.
This makes it possible to change those dependencies with no changes to the depending classes.
In a Spring application, an application context loads bean definitions and wires them together. 

## declarative programming through aspects.
Systems are composed of several components, each responsible for a spe- cific piece of functionality. But often these components also carry additional respon- sibilities beyond their core functionality.
System services such as logging, transaction management, and security often find their way into components whose core responsi- bilities is something else
These system services are commonly referred to as cross-cut- ting concerns
- The code that implements the system-wide concerns is duplicated across multi- ple components.
- Your components are littered with code that isn’t aligned with their core func- tionality. 

AOP makes components more cohesive and that focus on their own specific concerns. In short, aspects ensure that POJOs remain plain.

At its core, an application consists of modules that implement business functionality. With AOP, you can then cover your core application with layers of functionality. These layers can be applied declaratively throughout your application in a flexible manner without your core application even knowing they exist.

## Spring container
the container creates the objects, wires them together, configures them, and manages their com- plete lifecycle from cradle to grave (or new to finalize(), as the case may be).
>学到一个词组，from cradle to grave，感觉很不错的样子

There’s no single Spring container. Spring comes with several container imple- mentations that can be categorized into two distinct types.
- Bean factories (defined by the org.springframework.beans.factory.BeanFactory interface) are the simplest of containers, providing basic support for DI.
- Application contexts (defined by the org.springframework.context.ApplicationContext interface) build on the notion of a bean factory by providing application-framework services,
