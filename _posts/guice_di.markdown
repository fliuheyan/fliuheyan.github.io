# Guice

## Feature
* 100 times fast than Spring
* easy, fast to learn
Guice更适合与嵌入式或者高性能但项目简单方案，如OSGI容器，spring更适合大型项目组织。

## Annotation

* Inject
* Provides
* Named
* AssistedInject

## How to init collection



### Multibinder
`Multibinder` is binding multiple implementations of an interface

### Mapbinder
``

### TypeLiteral



```
bind(new TypeLiteral<List<String>>() {}).toInstance(new ArrayList<String>());
```
