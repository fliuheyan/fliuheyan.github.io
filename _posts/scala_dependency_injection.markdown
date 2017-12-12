
## self-type
```
trait B
trait A { this: B => }
```
this means A has a dependency with B

## difference between self-types and trait subclasses
https://stackoverflow.com/questions/1990948/what-is-the-difference-between-self-types-and-trait-subclasses
self-types: `B` require `A`
```
trait A
trait B { this: A with C_trait with D => } //multiple dependency
```
trait subclasses `B extends A`, then `B` is an `A`

## Cake pattern
[real-world-scala-dependency-injection-di](http://jonasboner.com/real-world-scala-dependency-injection-di/)

```

```

## implicit

## Reader monad

## Goole guice
