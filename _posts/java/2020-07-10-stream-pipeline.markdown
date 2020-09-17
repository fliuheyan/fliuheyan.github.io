---
layout: post
title:  "stream pipeline"
date:   2020-07-10 11:11:11 +0100
---

## Stream Pipelines

基于lambda. 对于函数借口(SAM, 只有一个抽象方法的接口)，它的实现可以简化为lambda.
```shell script
@FunctionalInterface
public interface ConsumerInterface<T>{
	void accept(T t);
}

ConsumerInterface<String> consumer = (str) -> System.out.println(str);
```

| 操作类型  | 接口方法  |
|  ----  | ----  |
| 中间操作  | concat() distinct() filter() flatMap() limit() map() peek() skip() sorted() parallel() sequential() unordered() |
| 结束操作	  | allMatch() anyMatch() collect() count() findAny() findFirst() forEach() forEachOrdered() max() min() noneMatch() reduce() toArray()|

一般返回stream的是中间操作，否则是结束操作

`StatelessOp`
`StatefulOp`

stream的每一步都是`<数据来源，操作，回调函数>`, Stream当中将这些步骤封装成为`Sink`.

```java
interface Sink<T> extends Consumer<T> {
    default void begin(long size) {}
    default void end() {}
    default boolean cancellationRequested() {
        return false;
    }
    default void accept(int value) {
        throw new IllegalStateException("called wrong accept method");
    }
}
```

对于不同的操作就是实现不同的Sink对象
`begin` 初始化容器
`accept`
`end`

`Sink`接口的作用就是

```shell script
public final <R> Stream<R> map(Function<? super P_OUT, ? extends R> mapper) {
    Objects.requireNonNull(mapper);
    return new StatelessOp<P_OUT, R>(this, StreamShape.REFERENCE,
                                 StreamOpFlag.NOT_SORTED | StreamOpFlag.NOT_DISTINCT) {
        @Override
        Sink<P_OUT> opWrapSink(int flags, Sink<R> sink) {
            return new Sink.ChainedReference<P_OUT, R>(sink) {
                @Override
                public void accept(P_OUT u) {
                    downstream.accept(mapper.apply(u));
                }
            };
        }
    };
}
```


#### collect() 方法

```shell script
<R> R collect(Supplier<R> supplier, BiConsumer<R,? super T> accumulator, BiConsumer<R,R> combiner)
```
活脱脱的就是一个scala里的fold， `R.method(T)`的时候调用accumulator, `R.method(R)`的时候调用combiner


#### Supplier

#### Consumer


#### Predicate   


### 参考

https://github.com/CarpenterLee/JavaLambdaInternals
