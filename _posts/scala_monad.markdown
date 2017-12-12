### Category

1. objects and maps
2. associative and identity

### Functor
Consider two categories C1 and C2; then a functor F is a structure-preserving mapping between these categories.
1. A => F[A]
2. (A -> B) => (F[A] -> F[B])

### EndoFunctor  map the category to itself

### Applicative trait 

### Monadic

### Monad

#### Unit
unit is an operation that creates a monad M[A] from an object of type A. So in some Monad implementations just using `apply` to instead

#### FlatMap
map then flatten

#### Why FlatMap not Map
FlatMap is way more powerful than map. It gives us the ability to chain operations together.
```
m map g = flatMap(x => unit(g(x)))
```
On the other hand, if all we had was map and unit, we would not be able to define flatMap because neither one of them has a clue about flattening.

### Why Monad is useful



```
val result = UserService.loadUser("mike")
if(result != null) {

}
```

```
UserService.loadUser("mike").flatMap(_.getChild)
```

further more...
```
val result = for{
  user <- UserService.loadUser("mike")
  child <- user.getChild
} yield child
```

#### Maybe Monad
Option is a construct that allows us to avoid null pointers in Scala.
null -> Empty Object -> Optional
#### IO Monad

#### Future
