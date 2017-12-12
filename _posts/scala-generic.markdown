## Concept
* upper bounds
java
```
<T extends Test>
<? extends Test> //wildcard
```
scala
```
[T <: Test]
[_ <: Test]
```
upper bounds适用于把泛型对象当作数据的提供者(生产者)的场景下:(getter)
```
def pr(list : List[_ <: Any]) {
            list.foreach(print)
        }
```
* lower bounds
java
```
<T super Test>
<? super Test>
```        
scala
```
[T :> Test]
[_ :> Test]
```
lower bound适用于把泛型对象当作数据的消费者的场景下: (setter)
```
def append[T >: String] (buf : ListBuffer[T])  = {  
                buf.append( "hi")
        }
```
* multiple bounds
java
```
<T extends A & B>
```
scala don't have multiple bounds, using compund type instead
```
[T <: A with B]
```
