# Swift 03
----
## 枚举：
枚举是一种类型，定义后数据类型就是定义时的名字 ， 可选型实质就是一个枚举类型，是枚举的一种包装类型
定义：
```swift
enum 名称 {
    case 可能性1
    case 可能性2
    ...
}
```
```swift
enum Week {
    case Monday
    case Tuesday
    case Wednesday
    case Thursday
    case Friday
    case Saturday
    case Sunday
}
```
`case` 后的可能性也可以写在一行 用 , 分隔
```swift
enum Season {
    case Spring ,Summer ,Autumn , Winter
}
```
使用：
```swift
let today = Week.Sunday

let today1:Week = .Sunday
```
###### `Raw Value`:
在定义枚举时指定每个值类型，并给每个case 赋值
```swift
enum Week:Int {
    case Monday = 1
    case Tuesday = 2
    case Wednesday = 3
    case Thursday = 4
    case Friday = 5
    case Saturday = 6
    case Sunday = 7
}
```
使用时就可通过 枚举值.rawValue获得对应的值
```swift
func daysBeforeNextWeek(week:Week) ->Int{
    return 7 - week.rawValue
}
daysBeforeNextWeek(week: Week.Friday)
```
Raw Value 指定为Int类型时可以不写值， 默认从0开始递增；
```swift
enum Season:Int {
    case Spring ,Summer ,Autumn , Winter
}
let s:Season = .Winter
print("the num is \(s.rawValue)")
```
Raw Value 指定为String类型时默认值是变量名本身

###### Associate Value ：关联值
在枚举的定义时用 ( 类型 )给每个可能值指定一个需要的类型进行关联 ，也可以不指定不关联
在使用时通过 .可能值( 参数) 将值传递进去
```swift
enum Status {
    case Code(Int)
    case Message(String)
    case Extra
}

func networkConnect (a:Int) -> (Status , Status){
    if a > 0 {
        return (.Code(200) , .Message("request success"))
    }else {
        return (.Code(500) , .Message("request failure"))
    }
}

networkConnect(a: 1)
```
枚举的每个case的关联值可以设置多个，相当于设置成一个元组
```swift
enum Shape{
    case Square(side:Double)
    case Rect(width:Double , height:Double)
    case Circle(x:Double , y:Double , r:Double)
    case Point
}

let square = Shape.Square(side: 2.0)
let rect = Shape.Rect(width: 3.0, height: 2.0)
let circle = Shape.Circle(x: 0, y: 0, r: 5.0)
let point = Shape.Point

func getArea (shape:Shape) -> Double{
    switch shape {
    case let .Square(side):
        return side * 2
    case let .Rect(width ,height):
        return width * height
    case let .Circle(_ ,_ , r):
        return M_PI * r * r
    case let .Point:
        return 0
    }
}
getArea(shape: square)
getArea(shape: rect)
getArea(shape:circle)
getArea(shape: point)
```

###### 枚举的递归：
在`enum` 关键字前 增加`indirect` 关键字 ，或者在使用了递归的case 前增加
```swift
indirect enum Arithmetic {
    case Number(Int)
    case Sum(Arithmetic , Arithmetic)
    case Sub(Arithmetic , Arithmetic)
}

indirect enum Arithmetic {
    case Number(Int)
    case Sum(Arithmetic , Arithmetic)
    case Sub(Arithmetic , Arithmetic)
    case Multi(Arithmetic , Arithmetic)
}

// (8-1) * (7+2)
let one = Arithmetic.Number(1)
let two = Arithmetic.Number(2)
let seven = Arithmetic.Number(7)
let eight = Arithmetic.Number(8)
let sub = Arithmetic.Sub(eight, one)
let sum = Arithmetic.Sum(seven, two)
let multi = Arithmetic.Multi(sub, sum)

func test(a:Arithmetic) -> Int{
    switch a {
    case let .Number(v):
        return v
    case let .Sum(v, m):
        return test(a:v) + test(a:m)
    case let .Sub(v, m):
        return test(a:v) - test(a:m)
    case let .Multi(v, m):
        return test(a:v) * test(a:m)
    }
}
test(a:multi)
```

## 结构体：Struct
定义：
```swift
struct 名称{
    ...
}
```
结构体的语法与类的语法相同
```swift
struct Computer {
    var cpu:String
    var ram:String
    var gpu:String
    var screen:String
}
let mac = Computer(cpu:"intel" ,ram:"8GB" ,gpu:"nivida" ,screen:"svmsung") //调用的是结构体的默认构造函数，属性必须按顺序书写
```
结构体的属性也可以赋予初值：
```swift
struct Computer {
    var cpu:String = "intel"
    var ram:String = "8GB"
    var gpu:String = "nivida"
    var screen:String = "shape"
}
var mac = Computer()
var mac1 = Computer(cpu:"intel" , ram:"16GB" , gpu:"nivida" , screen:"shpae")
mac.cpu = "amd"
mac1.ram = "32G"
```
结构体的属性也可以在声明后修改： 必须定义结构体和结构体内的属性为 var 类型 ，let类型无法修改
```swift
mac.cpu = "amd"
```
为结构体创建自定义的构造函数：
当为结构体定义构造函数后默认的构造函数就无法使用了， 需要重写默认的构造函数
```swift
init （参数 ，…）{
    ...
}
```
构造函数可以同时声明多个
```swift
struct Computer {
    var cpu:String = "intel"
    var ram:String = "8GB"
    var gpu:String = "nivida"
    var screen:String = "shape"
    var user:String? //结构体的属性可以是可选型，可选型可以不赋初值，默认初值是nil

    // "intel,8GB,nivida,shape"
    init (_ info:String) {
        let sArray = info.components(separatedBy: ",")
        self.cpu = sArray[0]
        self.ram = sArray[1]
        self.gpu = sArray[2]
        self.screen = sArray[3]

    }

    init (cpu:String ,ram:String ,gpu:String ,screen:String){
        self.cpu = cpu
        self.ram = ram
        self.gpu = gpu
        self.screen = screen
    }
}
var mac = Computer("intel,8GB,nivida,shape")
var mac1 = Computer(cpu:"intel" , ram:"16GB" , gpu:"nivida" , screen:"shpae")
```

###### Failable initializer ,可失败的构造函数， 当传入的参数不合法时返回nil
在`init`后加?即可
```swift
// "intel,8GB,nivida,shape"
init?(_ info:String) {  //在init后加?定义为 failable initializer
    let sArray = info.components(separatedBy: ",")
    guard sArray != nil && sArray.count == 4 else {
        return nil //判断传入的参数是否可正确解析，错误的话返回nil
    }
    self.cpu = sArray[0]
    self.ram = sArray[1]
    self.gpu = sArray[2]
    self.screen = sArray[3]
}
```
结构体的方法：在结构体内部定义func 函数
```swift
 func boot () ->String {
    return "boot success"
 }

 func getCpuInfo() -> String {
    return self.cpu
 }

var mac = Computer("intel,8GB,nivida,shape")
var mac1 = Computer(cpu:"intel" , ram:"16GB" , gpu:"nivida" , screen:"shpae")
mac?.boot()
mac1.getCpuInfo()
```
swift中枚举也可以定义方法
枚举和结构体是值类型：`Value Type`
定义2个枚举或结构体的实例，使实例2 = 实例1 (赋值就是copy)
当修改实例2中的属性值，实例1的属性不会改变 ， 他们分别对应内存中的2块空间
```swift
var mac1 = Computer(cpu:"intel" , ram:"16GB" , gpu:"nivida" , screen:"shpae")
var mac2 = mac1
mac2.cpu = "amd"
```
`struct` `enum` `Array` `Dictionary` `Set` `String` `Int` `Float` `Double` `Bool` 都是值类型

> Note:结构体中无法自己修改自己的属性，需要在修改自己属性值的方法前加`mutating` 关键字
```swift
struct Location {
    var x = 0
    var y = 0

    mutating func moveX () {
        self.x += 1
    }
}
var l1 = Location()
l1.moveX()
```
类是引用类型:
定义实例2 = 实例1 （赋值就是引用指向） ， 修改实例2的属性值，实例1的值也会改变，他们都指向内存中的同一块空间，与java的类一致
```swift
var patrick = Person(name:"patrick" , age:30)
var patrick1 = patrick
patrick1.name = “patrick1” //修改patrick1的name属性值， patrick的name属性值也会改变
```

## 类：
```swift
class 类名 {
    ...
}
```
类的属性在定义时如果没有赋初始值就必须定义构造函数
```swift
class Person {
    var name:String = ""
    var age:Int = 0
}
```
或
```swift
class Person {
    var name:String
    var age:Int

    init (name:String , age:Int){
        self.name = name
        self.age = age
    }
}
var patrick = Person(name:"patrick" , age:30)
```
类也可定义 `Failable initializer` 构造函数
两个类的实例不能用`==`判断 （`==` 用于判断值类型）， 要用`===`判断两个实例是否指向同一个对象（`!==` 判断不等）

计算型属性：
指该属性由其他变量计算而来
变量声明后在类型后直接接 { } , 在大括号内书写计算表达式即可
```swift
struct Point {
    var x:Float = 0
    var y:Float = 0
}
```
```swift
class Circle {
    var origin:Point
    var radius:Double
    var area:Double {
        return .pi * radius * radius
    }

    init(origin:Point , radius:Double) {
        self.origin = origin
        self.radius = radius
    }
}
var c = Circle(origin:Point(x:2 ,y:2) , radius:7)
c.area
```
计算型值必须是`var` 并且要指定数据类型
计算型属性需要可以改变值的话需要增加`set` `get`方法：(`getter` 和`setter`并非必须的)
```swift
struct Point {
    var x:Float = 0
    var y:Float = 0
}

class Circle {
    var origin:Point
    var radius:Double
    var area:Double {
        //setter
        set{
            radius = sqrt(newValue / .pi) //newValue是swift默认的新的值的名字
        }
        //getter
        get{
            return .pi * radius * radius
        }
    }

    init(origin:Point , radius:Double) {
        self.origin = origin
        self.radius = radius
    }
}
var c = Circle(origin:Point(x:2 ,y:2) , radius:7)
c.area
//设置setter函数后就可以给area属性重新赋值，赋值后radius属性会根据setter函数中的表达式计算改变
c.area = 80
```
###### 类型属性：Type Property
该属性属于整个类型，不属于类型的某个实例 ，在变量前加static关键字即可 ，相当于静态属性
```swift
class User {

    static var group:String = "GROUP1"
    var userName:String

    init (userName:String) {
        self.userName = userName
    }
}
//调用
User.group
```
###### 类型方法： Type Method
方法属性类型 ， 不属性类型的实例， 在变量前加static关键字 ， 相当于静态方法
```swift
class User {

    static var group:String = "GROUP1"
    var userName:String

    init (userName:String) {
        self.userName = userName
    }

    static func setGroup (g:String) -> Void {
        group = g
    }

}
User.group
User.setGroup(g: "GROUP2")
User.group
```

###### 属性观察器： Property Observer
主要用于保护数据传入的合法性，在属性后加{ }，在{ }中用`didSet` ，`willSet`关键字定义一段语法
> `didSet`和`willSet`不会在初始化(`init` ， 赋默认值)的时候执行，

`didSet` : 在新的值赋值之后运行
```swift
var property:类型 {
    didSet (oldProperty){
        //...
    }
}
```
```swift
class Person {

    static var maxAge:Int = 120
    var age:Int {
        didSet (oldAge){
            if age > Person.maxAge {
                age = oldAge
                print("age value is too large")
            }
        }
    }

    init(age:Int){
        self.age = age
    }
}

var patrick = Person(age:30)
patrick.age = 120
patrick.age = 121 //此时age会被返回为上一次设置的值 120 ，并且打印提示
```

`willSet` : 在新的值赋值之前运行
```swift
var property:类型 {
    willSet (oldProperty){
        //...
    }
}
```
###### Lazy Property 延迟属性
定义：延迟属性在第一次使用时会根据预定的表达式计算值，并将值存储在变量中，后续使用时直接调用，没有使用前不会计算
```swift
lazy var 属性名:类型 =  {
        //Statement
}( )
```
```swift
class CloseRange {

    let start:Int
    let end:Int

    var length:Int {
        return end - start + 1
    }

    lazy var sum: Int = {
        var result = 0
        for i in self.start...self.end {
            result += i
        }
        print(String(result))
        return result
    }()

    init? (_ start:Int , _ end:Int){
        if start > end {
            return nil
        }
        self.start = start
        self.end = end
    }
}

if let range = CloseRange(1 , 100) {
    range.length
    range.sum
}
```

###### deinit :析构函数
用于在对象删除前做清理动作，与初始化函数init相对

###### 继承： 结构体没有继承模式，继承模式是类特有的
子类继承父类在子类名后加 : 再加父类名称
```swift
class Animal{

    func run ()-> Void{
        print("i'm running")
    }
}

class Cat:Animal {

    override func run() {
        print("cat is running")
    }
}
```
当一个类不希望被继承时在类的定义前增加`final`关键字
```swift
final class 类名{
      //...
}
```
> 子类重载父类的属性和方法时需要在方法前加override关键字，不希望被重载的属性和方法可以用`final`关键字修饰

当子类需要重写`init`构造器时，需要在完成自身的构造后通过`super.init(参数)`调用父类的构造
```swift
public class Person{

    var name:String

    init(_ name:String){
        self.name = name
    }
}

public class Man:Person {

    var gender:String

    init(_ name:String , _ gender:String){
        self.gender = gender
        super.init(name)//父类的构造需要在子类构造完成后调用，与java的调用顺序是反的

        //构造完成后才能对属性和方法做调用和修改
    }
}
```
> 在构造过程中要调用类的属性和方法时需要在构造过程(本身和父类)完成后调用
###### 便利构造函数
当在一个类中有多个构造器，其中的一个构造器需要调用其他的构造器时，需要在构造器前`convenience`关键字
便宜构造器表示没有完成构造，而是在执行一些逻辑后再调用自身其它的构造器完成构造
```swift
public class Woman {

    public var name:String

    init(_ name:String) {
        self.name = name
    }

    public convenience init() {
        self.init("carina")
    }
}
```
`convenience`的构造器必须要调用自身的一个构造器，不能调用父类的构造
- 如果子类实现了父类的所有指定的构造函数(非convenience)，则会自动继承父类的便利构造函数(convenience);
- 如果子类没有继承父类的构造函数，则自动实现父类所有的指定构造函数，继而继承父类的便利构造函数
###### required构造函数
当一个类需要继承它的子类都实现某个构造函数时，在该构造函数前加`required`关键字 ， 子类实现时也需要加`required`，不需要`override`
```swift
public class User{

    var name:String

    public required init (_ name:String) {
        self.name = name
    }
}

public class User1:User{

    required public init(_ name: String) {
        super.init(name)
    }
}
```


## 访问控制
- public ：可以被模块外访问
- internal：可以被本模块访问 默认值
- private：可以被本文件访问

## 单例模式
将`init`函数设置为private使外部无法初始化，在用static let 修饰一个默认的单例对象调用本身的构造器，这样外部就可以通过这个static的对象获得该类的单例
```swift
public class UserManager {

    public static let instance = UserManager()

    private init(){

    }

    public func delete(){
        print("delete a user")
    }
}
```
调用单例对象
```swift
let u = UserManager.instance
u.delete()
```

## 文档注释
单行文档注释使用 `///`， 比注释多一个/

多行文档书写格式 和多行注释比在开始行多一个* ,使用MarkDown语法
```swift
/**
 write some desciption about this class
 */
```
在文档中描述函数的参数：

使用 - Parameter 参数名: 参数描述
```swift
/**
     - Parameter start:param1
     - Parameter end:param2
*/
```
或者：使用 Paramenters
```swift
/**
   - Parameters:
      - start:param1
      - end:param2
*/
```
对函数返回值进行文档说明：-Returns: ...
```swift
/**
     - Returns: this is return value
*/
```
对异常抛出的文档描述：- Throws: ...
```swift
/**
     - Throws: this is exception
*/
```

在类视图中分隔属性和方法，方便查看： `// MARK: - `

未完善的待修改的代码标记： `// TODO: 描述` 或 `// FIXME: 描述`
