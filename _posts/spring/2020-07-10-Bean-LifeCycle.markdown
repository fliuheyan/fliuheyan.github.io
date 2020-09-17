---
layout: post
title:  "AQS & CLH"
date:   2020-07-10 11:11:11 +0100
---
## Spring

### Bean Lifecycle

#### 元信息配置 & 解析 (xml/annotation -> Resource -> BeanDefinition)

入口`SpringApplication#run`

* properties
* xml `BeanDefinitionReader` & `BeanDefinitionParser`
* annotation  `AnnotatedBeanDefinitionReader`
* API `BeanDefinitionReader#loadBeanDefinitions`

#### BeanDefinition 注册到IOC Container

* BeanDefinitionRegistry

* `DefaultListableBeanFactory implments BeanDefinitionRegistry`

* `public void registerBeanDefinition(String beanName, BeanDefinition beanDefinition)` 
会将`beanName`和`BeanDefinition`放入`Map<String, BeanDefinition> beanDefinitionMap`
会将`beanName`按照顺序放入`beanDefitionList`

#### BeanDefinition Merge Stage
* 父子 BeanDefinition合并
    * `RootBeanDefinition` 
    * `GenericBeanDefinition`

* `getMergedBeanDefinition(String beanName)` 递归`getParent`

#### 通过class loader加载bean 
`ClassUtils.forname(class)` 拿到bean的class然后放入beanDefinition当中

#### Bean 实例化

* Before `instantiationAwareBeanPostProcesor#postProcessBeforeInstantiation`
一般不用

* InstantiationStrategy 
1. 默认的实现
从`BeanDefinition`当中拿到`Class` ,然后`clazz.getDeclaredConstructor()`拿到构造器,
构造器参数通过`beanFactory.getParameterNameDiscoverer()`拿到//TODO resolveProperties
`Constructor.newInstan()`

2. cglib

* 

### FAQ

1. BeanFactory的层次性

2.`BeanPostProcessor`
