# Kotlin
----
## <font color=#ffff00>变量 var
```kotlin
var 变量名：类型 = 初始值
var a:Int = 18
```
常量用`val`修饰
```kotlin
val 常量名: 类型 = 值
val name:String = "Steve"
```
## <font color=#ffff00>Control Flow 控制语句

### if
普通用法
```kotlin
var x = 8
var y:Int
if (x > 0) y = x
```
if ... else ...
```kotlin
var x = 8
var y = 1
 if (x > 0) {
   y = x
} else{
   y = 0
}
```
if 语句可以当表达式使用，可以有返回值
```kotlin
var x = 8
var y = if (x > 0) x else 0
```
if语句当表达式时，有多行语句时最后一行表达式将作为返回值
```kotlin
var x = 8
 var y = if (x > 0){
     //do something
    x
} else {
    //do something
    0
}
```

### when
相当于java的switch语句
```kotlin
var x = 1
when(x){
    0 -> print("the value is 0")
    1 -> print("the value is 1")
    2 -> print("the value is 2")
    else -> print("the value is wrong") //like default in java
}
```
when 的每个条件可以匹配多个值，每个值中间用,隔开
```kotlin
var x = 1
when(x){
    0 -> print("the value is 0")
    1, 3 -> print("the value is 1, 3")
    2, 4 -> print("the value is 2, 4")
    else -> print("the value is wrong") //like default in java
}
```
when的条件也可以是一个表达式、有返回值的方法
```kotlin
var x = 1
when(x){
   0 -> print("the value is 0")
   1, 3 -> print("the value is 1, 3")
   2, 4 -> print("the value is 2, 4")
   3+5 -> print("the value is 8") //the condition can be a expression
   else -> print("the value is wrong") //like default in java
}
```
when可以判断值是否在某个区间
```kotlin
var x = 1
when(x){
   in 0..10 -> print("the value is right")
   !in 10..20 -> print("the value is not in 10 ~ 20")
   else -> print("the value is wrong") //like default in java
}
```
when 条件也可以进行类型判断,也可以判断表达式代替if ... else...
```kotlin
var x = 1
when(x){
   is Int -> print("the value is Int")
   else -> print("the value is wrong") //like default in java
}
```

### for
遍历可迭代对象中的元素(value)
```kotlin
val array = arrayOf(1,2,3,4,5)
for (a in array){
    print(a)
}
```
遍历index
```kotlin
for (index in array.indices){
    print(index)
}
```
遍历index 和value
```kotlin
for ((index ,value) in array.withIndex()){
    print("index:$index value:$value")
}
```
### while & do...while...
跟java一样
```kotlin
while(true){
    statement
}
```


## <font color=#ffff00>类 class
类的定义：
```kotlin
class 类名 构造函数{
    //...
}
```
e.g.
```kotlin
class Person constructor(name:String){

}
```
当构造函数没有其他注解和修饰时constructor关键字可以省略
```kotlin
class Person (name:String){

}
```
当构造的初始参数没有时( )省略
```kotlin
class Person ( ){

}
```
构造函数不能包含任何语句，可以通过`init`关键字定义初始化方法
```kotlin
class MainPresenter(activity: MainActivity){

    var context:Context

    init {
        //在init函数中可以直接使用类名后定义的构造参数
        Logger.d(activity.name)
        context = activity
    }

}
```
也可以通过类的属性来对构造器传入的参数进行初始化
```kotlin
class Person (name:String){
   val firstName = name.toUpperCase()
}
```
当类的构造有注解和修饰时放在`constructor`前
```kotlin
class Person public @Inject constructor (name:String){
    val firstName = name.toUpperCase()
}
```
类也可以定义多个构造器，后面定义的构造器必须要调用primary的构造器，如果没有primary构造器，则不需要调用,多个构造器也是用`constructor`定义
```kotlin
class Person (name:String){
    //:this (name) 表示的就是调用primary的构造器
    constructor(name:String, gender:String): this(name){
        //do something
    }
}
```
```kotlin
class Person{
    //primary构造器不存在时就不需要调用
    constructor(name:String, gender:String){
        //do something
    }
}
```
类的实例化：通过调用构造器实例化，注意不需要new关键字，其他和java类似
```kotlin
var p = Person("Steve" ,"male")
```
###### 类的继承
所有类的基类时Any(相当于java的Object),所有类默认是不可被继承的,如果需要被继续需要在类前加open关键字或定义类abstract类
通过: 表示继承关系，父类有primary构造器时，需要在继承时调用
```kotlin
//super class
open class Person(name:String){

}
//sub class
class User(name:String) : Person(name){

}
```
父类没有primary构造器，有其它构造器时，使用super关键字调用
```kotlin
open class Person{
     constructor(name:String)
 }

 class User : Person{
     constructor(name:String, gender:String) : super(name)
 }
```
继承时对父类方法的重载使用`override`关键字, 并且父类被重载的方法需要用`open`修饰
```kotlin
open class Person{
    constructor(name:String)
    open fun run(){

    }
}

class User : Person{
    constructor(name:String, gender:String) : super(name)

    override fun run() {
        super.run()
    }
}
```
抽象类和抽象方法的继承，不需要使用open关键字, Note:抽象类默认有一个空的primary构造函数需要实现
```kotlin
abstract class Person{
   abstract fun run()
}

class User : Person(){

   override fun run() {

   }
}
```
###### 匿名内部类定义
```kotlin
object1.setOnLoadListener(object:匿名类(接口){
      override fun 方法名 ( 参数...) {
        //....
      }
  })
```
```kotlin
HttpMaster(Constant.FULL_URL).execute(object:HttpMaster.OnLoadListener{
            override fun onSuccess(s: String) {
                Logger.d(s)
            }

            override fun onFailure(e: String) {
                Logger.d(e)
            }

})
```
###### object类
相当于java中静态方法类，用objec修饰类，就可以通过类名.方法名 对方法进行直接调用，不需要实例化
```kotlin
object SPUtil{

    var sharedPreferences:SharedPreferences = null!!
    var editor:SharedPreferences.Editor

    fun init(context:Context){
        sharedPreferences = context.getSharedPreferences("sp" , Context.MODE_PRIVATE)
        editor = sharedPreferences.edit()
    }

    fun put(context:Context , key:String , obj:Object){

    }
}
```
###### data类(POJO类)
使用data关键字修饰， 只需要定义属性，kotlin会自动实现setter getter toString hashCode ,equals 等方法
```kotlin
data class ImageInfo(
        var id: Int,
        var name: String,
        var url: String,
        var link:String
)
```
###### 常量类
```swifr
class Constant{
    companion object{
        val URL = "http://www.baidu.com"
        val CITY = "Shenzhen"
    }
}
```

## <font color=#ffff00>函数 fun
fun 关键字定义函数，有返回值时在参数后用 ：类型 定义，没有则不需要些
```kotlin
fun getInfo (name:String) : String{
    return name + "is ..."
}
```
当函数体可以用一个表达式表示，可以省去{}，直接用 = 接表达式
```kotlin
fun sum (a:Int , b :Int) :Int = a + b
```
函数的参数可以设置默认值， 当不传入该参数时就使用该默认值
```kotlin
fun hello(name:String = "obama"){
    print("hello" + name)
}
```

##  <font color=#ffff00>Label
在lambda表达式中使用return会跳出整个上一级方法，而不是跳出表达式，可以通过定义label指定跳出表达式
```kotlin
val array = arrayOf(1, 2, 3, 4, 5)
 fun iterate(){
     array.forEach fe@ { //在lambda表达式中通过label名+@定义一个label
         //通过@+label名称使用一个label，表明返回中断的label定义的这一层代码，而不是return终止整个方法
         if(it == 0) return@fe
     }
 }
```
或者可以直接使用@+表达式方法名
```kotlin
val array = arrayOf(1, 2, 3, 4, 5)
fun iterate(){
    array.forEach {
        //通过@+方法名
        if(it == 0) return@forEach
    }
}
```
