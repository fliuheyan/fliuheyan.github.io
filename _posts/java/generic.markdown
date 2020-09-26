### Generic

#### <? extends T> 及<? super T>

* <? extends T> : 上界通配符(Upper Bounds Wildcards),表示上界是T,用此修饰符修饰的泛类容器,可以指向本类及子类容器

`不能往里存，只能往外取`

* <? supper T> : 下界通配符(Lower Bounds Wildcards),表示下界是T,用此修饰符修饰的泛类容器,可以指向本类及父类容器

`不影响往里存，但是往外取只能放在 Object`
