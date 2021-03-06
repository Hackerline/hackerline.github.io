---
layout: post
title: Spring技术内幕:BeanFactory/FactoryBean
categories: [Java]
---


### BeanFactory 和 FactoryBean
BeanFactory，是Spring IoC的一个Bean工厂接口，用来实现一个Container的基本接口。ApplicationContext，XmlBeanFactory等都继承自这个接口。

FactoryBean，是为了方便Factory实例在Spring IoC中的配置而产生的。继承FactoryBean接口的Factory实例，当使用Spring的BeanFactory来产生这个实例时，会直接得到该Factory所产生的对象。想要得到Factory本身，则需要在name前加&字符。
FactoryBean接口的代码：

	package org.springframework.beans.factory;  
	public interface FactoryBean<T> {  
	    T getObject() throws Exception;  
	    Class<?> getObjectType();  
	    boolean isSingleton();  
	} 

getObject() 工厂产生的实例。 

getObjectType()获取工厂返回的Bean的类型

isSingleton() 判定当前Bean是Singleton还是prototype

如果工厂AFactoryBean继承FacotryBean接口，当使用BeanFactory的getBean()时：

factory.getBean("AFactoryBean")返回的不是AFactoryBean，而是AFactory.getObject()产生的Object。如果想要获取AFactoryBean实例，则应在name前加&，这样写factory.getBean("&AFactoryBean"),产生的就是AFactoryBean实例。

（今天刚开始看到这两个东西后，实在无法理解FactoryBean的作用。看了文档和博客才发现，原来FactoryBean的存在就是为了解决Factory作为实例时的配置问题，让工厂模式在开发的时候配置起来更方便）


