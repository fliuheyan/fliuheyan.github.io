# Scala Introduction

## Features

* Immutable
* No Side effect

## Benefit
* Scala natively support some java patterns.
singleton pattern -> object
visitor pattern -> pattern matching
factory -> apply
builder -> currying
Dependency Injection -> cake pattern
immutable -> val
value object -> case class

* powerful collection

##
函数式程序是跟值打交道的，而不是跟状态打交道，它们的工具是表达式，而不是命令
在java当中，这样写是正确的
```
int a,b,c;
a=b=c=0;
```
而在scala当中会出得到一个`error: type mismatch`的错误
```
val a,b,c:Int=_
a=b=c=0
```
原因是因为scala当中将表达式c=0作为一个值，它的返回类型为Unit，而b类型为Int
Java当中逐行读取文本
```
while (( line = readLine() ) != null ) {
  //bala bala
}
```
在scala当中则是不行的，因为line=readLine() 这里返回值永远为Unit，永远不会等于null
Method:
在trait/class/object当中以def关键字声明的，不能被直接传递
Function:
A=>B的变量，这些变量背后是用FuncfonN对象来封装的;可以被传递。方法可以转换为函数

### 函数作为一等公民的体现
* 可传递
```
  def haha(f:() => String)
```
* 嵌套函数/赋值
val fun = (x:Int) => print(x)
嵌套函数应用场景并不多,其中一个场景是将递归函数的转为尾递归方式。
* 高阶
匿名函数or lambda也叫function literal
高阶函数则是接受另一个函数做参数，或者返回的而结果是一个函数
lambda演算中，每个表达式都代表一个只有单独参数的函数。(参数本身如果是一个函数，那么它也能有一个函数)，对于多参函数，
Currying
```
  def sum(x:Int,y:Int) = x+y
  def sum2(x:Int)(y:Int)
```
相当于将一个参数函数，转化为多个只有一个参数的函数来执行
```
  f(1,2)    =>  (f(1))(2)
```
Currying的实际作用
1. 改变代码的书写风格
```
  foo(res,() => "haha")   => foo(res)(() => "haha") => foo(res){ () => "haha"}
```
2. 实现partial application function(把一个函数适配为另一个函数)

* partial application

* closure

### Trait, Sealed Trait

### case class

###


###Lambda

### Scala泛型
java generic(proper type)
```
  class List2<C> //works
  class List2<C<T>> //doesn't work
```
Scala(higher-kinded type)
```
  import scala.language.higherKinds
  class List2[C[T]] //works
```

### type
通过type实现java中interface的作用
type X = { def close():Unit}
def free(res: X) = res.close
free(new {def close()=println("haha")})

#### compound type
```
trait X1; trait X2;
def test(x: X1 with X2) = ???
test(new X1 with X2)
type X = X1 with X2
type X = X1 with X2 { def close():Unit } //复合类型+结构类型, X必须有X1,X2特质，同时有close
```

### options
for yield
Map
Map2
FlatMap
Unzip
zip
traverse

### Functor

#### Definition
```
trait Functor[F[_]] {
    def map[A,B](fa: F[A], f: A=>B): F[B]
}
```
#### When and How to use it

### Monad

### Monoid
mappend
zero

```
trait Monoid[A] {
  def mappend(a1: A, a2: A): A
  def mzero: A
}

object IntMonoid extends Monoid[Int] {
  def mappend(a: Int, b: Int): Int = a + b
  def mzero: Int = 0
}

def sum(xs: List[Int], m: Monoid[Int]): Int = xs.foldLeft(m.mzero)(m.mappend)

scala> sum(List(1, 2, 3, 4), IntMonoid)
res7: Int = 10
```
further more , we could refactor it wiht `implicit`
```
def sum[A](xs: List[A])(implicit m: Monoid[A]): A = xs.foldLeft(m.mzero)(m.mappend)

implicit val intMonoid = IntMonoid

scala> sum(List(1, 2, 3, 4))
res9: Int = 10
```
(implicit m: Monoid[A]) here, the implicit parameter is often written as a context bound, so we could refactor more:
```
def sum[A: Monoid](xs: List[A]): A = {
  val m = implicitly[Monoid[A]]
  xs.foldLeft(m.mzero)(m.mappend)
}
```
hummm, could we abstract more ? Absolutely yes, we could abstract `List[A]` to `Functor[_]`,同时要对list的foldLeft方法做抽象出`trait FoldLeft`

```
trait Monoid[A] {
  def mappend(a1: A, a2: A): A def mzero: A
}

trait FoldLeft[F[_]] {
  def foldLeft[A, B](xs: F[A], b: B, f: (B, A) => B): B
}

def sum[M[_] : FoldLeft, A: Monoid](xs: M[A]): A = {
  val m = implicitly[Monoid[A]]
  val fl = implicitly[FoldLeft[M]]
  fl.foldLeft(xs, m.mzero, m.mappend)
}
```
使用的时候定义`M[A]`当中的`AMonoid`，指明它的`mappend`和`zero`方法
```
implicit val stringMonoid = new Monoid[String] {
  override def mappend(a1: String, a2: String): String = a1.concat(a2)
  override def mzero: String = ""
}
sum(List("a","b","c"))
```
我们也可以通过这个`monoid`很方便的去定义其他方法
```
trait Monoid[A] {
  def mappend(a1: A, a2: A): A
  def mzero: A
}
implicit val IntMonoid: Monoid[Int] = new Monoid[Int] {
  def mappend(a:Int,b:Int):Int = a+b
  def mzero:Int = 0
}
def plus[A: Monoid](a: A, b: A): A = implicitly[Monoid[A]].mappend(a, b)
```
result
```
scala> plus(2,3)
res4: Int = 5
```

对于所有已经拥有monoid实现的类型，我们都可以为其添加plus方法
```
trait MonoidOp[A] {
  val F: Monoid[A]
  val value: A
  def |+|(a2: A) = F.mappend(value, a2)
}
implicit def toMonoidOp[A: Monoid](a: A): MonoidOp[A] = new MonoidOp[A] {
  val F = implicitly[Monoid[A]]
  val value = a
}
```

### Method Injection



### Monadic

### Function1, FunctionN ????
apply
compose
andThen

### Implicitly

ad-hoc polymorphism  ?????




### type constructor ?????
->>


```
trait Functor[F[_]] { self =>
  def map[A, B](fa: F[A])(f: A => B): F[B]
}

trait FunctorOps[F[_],A] extends Ops[F[A]] {
  implicit def F: Functor[F]
  import Leibniz.===
  final def map[B](f: A => B): F[B] = F.map(self)(f) ...
}
```
