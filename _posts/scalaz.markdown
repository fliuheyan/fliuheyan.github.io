### Applicative

### Apply
```
trait Apply[F[_]] extends Functor[F] { self =>
  def ap[A,B](fa: => F[A])(f: => F[A => B]): F[B]
}
```

### applicative Functor
Applicatives are generalized functors.

Given a function that takes multiple arguments A,B,… that returns a Result, how can that function by applied to arguments M[A], M[B],… and get M[Result]?
M is just a container , could be a List,Option,etc.

```
trait GenericFunctor[->>[_, _], ->>>[_, _], F[_]] {

  def fmap[A, B](f: A ->> B): F[A] ->>> F[B]
}
```



```
def sequenceA[F[_]: Applicative, A](list: List[F[A]]): F[List[A]] = list match {
  case Nil => (Nil: List[A]).point[F]
  case x :: xs => (x |@| sequenceA(xs)) {_ :: _}
}
```

### types kinds
kind is the type of type

first-order-kinded

### Questions

Reader
Validation
Type class

curried
`val addInts = ( (a:Int, b:Int, c:Int) => a + b + c ).curried` => `addInts(1)(2)(3)`
how to implements it ???

```
Functor[List[Int]].lift((_: Int) + 2)
```
got an error `error: List[Int] takes no type parameters`

tagtype

Object Equality

Functor Laws
1. The first functor law states that if we map the id function over a functor, the functor that we get back should be the same as the original functor.
`Functor[F] map {identity} === Functor[F]`

2. The second law says that composing two functions and then mapping the resulting function over a functor should be the same as first mapping one function over the functor and then mapping the other one.
`Functor[F] map {f1} map {f2} === Functor[F] map { f1 compose f2}`

```
trait FunctorLaw {
/** The identity function, lifted, is a no-op. */
def identity[A](fa: F[A])(implicit FA: Equal[F[A]]): Boolean = FA.equal(map(fa)(x => x), f
  /**
   * A series of maps may be freely rewritten as a single map on a
   * composed function.
   */
def associative[A, B, C](fa: F[A], f1: A => B, f2: B => C)(implicit FC: Equal[F[C]]): Bool }
```
