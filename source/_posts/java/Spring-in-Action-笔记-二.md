---
title: Spring_in_Action_笔记(二)
tags:
  - Spring
  - MVC
  - Java
categories:
  - Technology
  - Java
date: 2016-05-21 11:45:57
---
如题，阅读《Spring_in_Action》的笔记

(二)Basic Wiring Beans 

<!-- more -->

***

### Spring’s configuration options

- Explicit configuration in XML
- Explicit configuration in Java
- Implicit bean discovery and automatic wiring

1. my recommendation is to lean on automatic configuration as much as you can. The less configuration you have to do explicitly, the better.
2. When you must explicitly configure beans (such as when you’re configuring beans for which you don’t maintain the source code), I’d favor the type-safe and more powerful JavaConfig over XML. 
3. Finally, fall back on XML only in situations where there’s a convenient XML namespace you want to use that has no equivalent in JavaConfig.

#### Automatically wiring beans

This simple annotation identifies this class as a component class and serves as a clue to Spring that a bean should be created for the class.

There’s no need to explicitly configure a SgtPeppers bean; Spring will do it for you because this class is annotated with @Component.

Component scanning isn’t turned on by default, however. You’ll still need to write an explicit configuration to tell Spring to seek out classes annotated with @Component and to create beans from them.

With no further configuration, @ComponentScan will default to scanning the same package as the configuration class.

If you’d rather turn on component scanning via XML configuration, then you can use the <context:component-scan> element from Spring’s context namespaceflying colors

>The test should pass with flying colors
用这本书，我可以学英语：flying colors 胜利的旗帜

Spring supports the @Named annotation as an alternative to @Component. There are a few subtle differences, but in most common cases they’re interchangeable.

With that said, I have a strong preference for the @Component annotation, largely because @Named is ... well ... poorly named. It doesn’t describe what it does as well as @Component.

Or what if you want to scan multiple base packages? To specify a different base package, you only need to specify the pack- age in @ComponentScan’s value attribute。

Rather than specify the packages as simple String values, @ComponentScan also offers you the option of specifying them via classes or interfaces that are in the packages. the basePackages attribute has been replaced with basePackage- Classes. 

autowiring is a means of letting Spring automatically satisfy a bean’s dependencies by finding other beans in the application context that are a match to the bean’s needs. To indicate that autowiring should be performed, you can use Spring’s @Autowired annotation.

If there are no matching beans, Spring will throw an exception as the application context is being created. To avoid that exception, you can set the required attribute on @Autowired to false. When required is false, Spring will attempt to perform autowiring; but if there are no matching beans, it will leave the bean unwired.

#### Wiring beans with Java
For instance, let’s say that you want to wire components from some third-party library into your application. Because you don’t have the source code for that library, there’s no opportunity to annotate its classes with @Component and @Autowired. Therefore, automatic configu- ration isn’t an option. In that case, you must turn to explicit configuration.

The key to creating a JavaConfig class is to annotate it with @Configuration.

#### Wiring beans with XML
{% codeblock lang:xml %}
<beans>  <!-- root element -->

    <bean>XML analogue to JavaConfig's @Bean annotaion</bean>

    <!-- it’s usually a good idea to give each bean a name of your own choosing via the id attribute: -->
    <bean id="compactDisc" class="soundsystem.SgtPeppers" />
    </beans>

    <bean id="cdPlayer" class="soundsystem.CDPlayer">
          <constructor-arg ref="compactDisc" />
          <!-- The <constructor-arg> element tells it to pass a reference to the bean whose ID is compactDisc to the CDPlayer’s constructor. -->
    </bean>

    <bean id="cdPlayer" class="soundsystem.CDPlayer"
        c:cd-ref="compactDisc" />
    <!-- Here you’re using the c-namespace to declare the constructor argument as an attribute of the <bean> element.  -->
    </bean>
</beans>
{% endcodeblock %}

这个太复杂了，有必要再细化。

***
Over！












