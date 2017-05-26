# Swift-04
----
## subscript 自定义下标
通过自定义下标获取和设置对象的属性值，通过`subscript`关键字
```swift
public struct ColorBox{
    var r:Int = 0
    var g:Int = 0
    var b:Int = 0

    public init(r:Int , g:Int , b:Int) {
        self.r = r
        self.g = g
        self.b = b
    }

    public subscript(index:Int) -> Int?{
        get{
            switch index {
            case 0 : return r
            case 1 : return g
            case 2 : return b
            default : return nil
            }
        }
        set{
            guard let newValue = newValue else { return}
            switch index {
            case 0 : r = newValue
            case 1 : g = newValue
            case 2 : b = newValue
            default : return
            }

        }
    }
}

var colorBlue = ColorBox(r:1 , g:1, b:1)
colorBlue[0]
colorBlue[1] = 23
```
多维下标
```swift
public struct Matrix{

    var data:[[Double]]
    let row:Int
    let column:Int

    public init(row:Int , column:Int){
        self.row = row
        self.column = column
        data = [[Double]]()
        for _ in 0..<row {
            let perRow = Array(repeating: 0.0, count: column)
            data.append(perRow)
        }
    }

    public subscript (row:Int , column:Int) -> Double {
        get{
            //断言关键字， 判断后面表达式的结果，如果为true不做任何事，如果为false，抛出异常，中断程序
            //第2个参数为异常提示信息
            assert(row>=0 && row < self.row && column>=0 && column < self.column , "index out of range")
            return data [row] [column]
        }
        set{
            assert(row>=0 && row < self.row && column>=0 && column < self.column , "index out of range")
            data [row] [column] = newValue
        }
    }

}

var matrix1 = Matrix(row: 5, column: 5)
matrix1[1,2]
matrix1[2 ,2] = 6
```
## Nested Type 嵌套
类中嵌套类 ， 类中嵌套结构体， 类中嵌套枚举



## Extension 扩展
用`extension`关键字修饰需要扩展的类，在不修改已有类的情况下增加指定的方法
```swift
extension type {
  func 方法名(参数...){
    //body
  }
}
```
```swift
public class Circle{
    var originX:Int
    var originY:Int
    var r:Double

    public init(_ originX:Int , _ originY:Int , _ r:Double) {
        self.originX = originX
        self.originY = originY
        self.r = r
    }
}

public extension Circle{
    func perimeter() -> Double{
        return .pi * 2 * self.r
    }
}
```
```swift
var circle = Circle(3 ,4, 5.0)
var c = circle.perimeter()
```
扩展还可以用来扩展计算型的属性
```swift
public class Circle{
    var originX:Int
    var originY:Int
    var r:Double

    public init(_ originX:Int , _ originY:Int , _ r:Double) {
        self.originX = originX
        self.originY = originY
        self.r = r
    }
}

public extension Circle{
    var square:Double{
        get{
            let sq = .pi * self.r * self.r
            return sq
        }
        //计算型属性的setter方法可以通过该属性赋值改变对象原始的存储型属性
        set(newSquare){
            self.r = sqrt(newSquare / .pi)
        }
    }
}
```
```swift
var sq = circle.square
circle.square = 20
```
扩展中增加的构造函数必须是便利的构造函数,扩展也可以扩展嵌套类型、下标
> extension 可以扩展系统类库和第三方类库


## Generic 泛型
通过泛型自定义一个栈结构体， 先进后出
```swift
public struct Stack<T> {

    public init() {

    }

    var array = [T]()

    public func isEmpty() -> Bool {
        return array.count > 0
    }

    public mutating func push (_ sub:T){
        array.append(sub)
    }

    public mutating func pop() -> T?{
        guard array.count > 0 else{
            return nil
        }
        return array.removeLast()
    }
}
```
```swift
var stack = Stack<Int>()
stack.push(3)
stack.push(5)
stack.isEmpty()
stack.pop()
stack.pop()
stack.isEmpty()
```


## Protocol 协议
可以相当于java的接口,但是协议可以有属性
```swift
protocol OnLoadListener{
    //协议的属性需要指定读取类型 get 或 set
    var url:String {get set}
    //协议的方法不能赋初始值
    func onSuccess (s:String)
    func onFailure ()

}
```
e.g.
```swift
public protocol Eatable{

    var number:Int {get set}
    var eatSpeed:Int {get}

    func eatWith(_ food: String)
}
```
```swift
public class Person : Eatable {

    public var number: Int
    public let eatSpeed: Int

    public init(){
        number = 2
        eatSpeed = 9
    }

    public func eatWith(_ food: String) {
        print("I want eat \(number) pcs \(food), I can finished in \(eatSpeed) minutes")
    }
}
```
```swift
var e:Eatable = Person()
e.eatWith("apple")
```
如果协议中定义了改变自身属性的方法，当协议被结构体实现时，需要在该方法前加`mutating`关键字
可以在协议后加 `: class` 限制协议只能被类实现
```swift
public protocol Eatable: class{

    var number:Int {get set}
    var eatSpeed:Int {get}

    func eatWith(_ food: String)
}
```
当一个类需要继承类并实现协议时，需要先写父类再用` , ` 连接其他需要实现的协议

当协议定义了`init`构造函数时，类实现的时候需要用`required`修饰构造函数，以保证其它类继承该类时也要重载`init`函数
(如果该类时`final`修饰的，`required`可以不需要)
```swift
protocol Phone{
    var model:String { get set}

    init(model: String)

    func call(number: Int)
}

class IPhone: Phone{
    var model: String

    required init(model: String) {
        self.model = model
    }

    func call(number: Int) {
        print("iphone7 is calling \(number)")
    }
}

var i7 = IPhone(model: "7")
i7.call(number: 18666667777)
```
## typealias 类型别名
将Double 类型命名为Height,此时可以用Height类型来声明一个Doube数据，提高代码的可读性
```swift
typealias Height = Double
extension Height {
    var m: Height { return self}
    var km: Height { return self * 1000}
    var cm: Height { return self / 10}
}
let h: Height = 26.01.km
let h1: Height = 21.0.cm
```

## associatedtype 用在协议中的类型别名（关联类型）
当协议中的一个属性在被实现时需要返回不同的数据类型，可以用`associatedtype`定义，具体实现时再设置具体的类型
```swift
protocol Measureable{
    associatedtype Height
    var height: Height { get }
}

class Person : Measureable{

    typealias Height = Double

    var height: Height

    init(height:Double){
        self.height = height
    }

}
```
###### 协议也可以继承和扩展
协议中无法定义函数的具体实现，协议的扩展可以指定统一的计算型属性和函数的实现(默认实现，如果实现类也实现了该函数再调用类实现的)


###### 协议聚合
protocol <协议1 , 协议2 ...> 表示该类型需要实现列举的所有协议


###### 协议的泛型约束
用泛型约束将协议当成一个类型使用
