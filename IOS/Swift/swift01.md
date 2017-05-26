# Swift 01
----
## 变量：
> var 变量名 = 变量初始值
```swift
var a1 = 10
var str = "Hello, playground"
str += " 1233"
```
可以在同一条语句中声明多个变量：
```swift
var x = 1 , y = 2, z = 3
```
显式声明变量：
```swift
var name1: String = "carina"
```
声明多个变量并指定变量数据类型：（swift是强类型语言）
```swift
var a2 , b2 , c2: Int
var a3 , a4: Double
```
变量赋予初始值时会自动根据值指定类型，playground 中 option(alt) + 左键点击变量可以查看变量类型
swift 语句结尾可以不加分号

## 常量：
> let 常量名 = 值
```swift
let a = 100
```
显式声明常量
```swift
let c : Int = 20
```

## 基本数据类型：
###### Int整型:
 Int.max  最大值
Int.min 最小值
UInt 无符号位的整型
整数可以用 _ 做分隔符，方便阅读
`let m = 100_0000`

###### Float浮点数: 32位
###### Double双精度浮点数:64位
```swift
var f1 = 3.1415926
var f2 = 1.8e10
var f3 = 1.8e-3
var f4 = 100_0000.0000_3
```
不同数据类型进行计算时，swift不会自动进行类型转换， 需要显示的进行类型转换
```swift
var c1:Int8 = 1
var c3:Int16 = 18
c1 + Int8(c3)
```
###### UIColor , CGFloat
`UIColor` 用来表示颜色 ,`CGFloat`类型代表颜色值:
```swift
let red:CGFloat = 0.3
let green:CGFloat = 0.4
let blue:CGFloat = 0.3
UIColor(red: red , green: green , blue: blue ,alpha: 1.0)
```
###### Bool布尔型：`true` 和 `false`

###### Tuple元组：
将多个不同的值集合成一个数据， 元组中可以有任意多个值， 不同的值可以是不同的类型
```swift
var point = (1, 2)
var response = (200 , "HTTPOK")
```
显式的指定元组中的数据类型：
```swift
var point1:(Int ,Int , Int) = (1,2,3)
```
将元组中的每个值分别赋值给变量：
```swift
var point = (1, 2)
var (x1, y1) = point
x1
y1
```
或 通过 `tupel.index` 获得
```swift
var x2 = point.0
var y2 = point.1
```
定义元组时给元组的每个元素指定名称：
```swift
var point2 = (x:1 , y:2 , z:3)
```
此时获取元组中的值就可以通过 tuple.元素名称
```swift
point2.x
```
或者在指定元组内数据类型时定义名称：
```swift
var point4:(x:Int , y:Int) = (1,2)
point4.x
point4.y
```
取值时忽略一个值使用 _ 代替：
```swift
var user = ("patrick" , 30)
var (userName , _) = user
userName
```

## print()
`print()`可以打印所有的基础数据类型和字符串，当打印多个变量时用,分隔
```swift
var d = 1 ,e = 2 ,f = true , g = 3.0
print(d, e , f , g)
```
默认的分隔符是空格 ， 可以指定separator参数来修改
```swift
var d = 1 ,e = 2 ,f = true , g = 3.0
print(d, e , f , g)
print(d, e , f , g , separator:"/")
```
默认的结束符是回车， 可以指定`terminator`参数修改
```swift
var d = 1 ,e = 2 ,f = true , g = 3.0
print(d, e , f , g , separator:"?" , terminator:"~~")
```

## 判断语句：
> if 条件 {
    。。。
}
```swift
if true {
    print("patrick")
}else if 3>2{
    print("patrick1")
}else {
    print("patrick2")
}
```
三目运算符： `true ? a1 : a2`
```swift
var battery = 21
var bColor = battery <= 20 ? UIColor.blue : UIColor.green
```
区间运算符：

闭区间运算符：
`a…b`  表示从a 到b的一个序列，包含a 和b
```swift
for i in 1...10{
    i
}
```
前闭后开区间运算符：
`a..<b` 表示从a 到b前一位元素的一个序列，包含a 但不包含b
```swift
for j in 0..<10{
    j
}
```
## 控制流：
###### 循环结构：
> for i in 序列
for . in . 循环必须将序列中的元素都循环一次
```swift
let nums = [1,2,3,4,5,6,7,8,9,10]
for k in 0..<nums.count{
    print(nums[k] ,terminator:" ")
}
```
> initialization
>
> while 条件表达式 {
    statements
    increments
}
```swift
var p = 0
while p < 10 {
    p+=1
    print(p)
}
```
> initialization
>
> repeat  {
    statements
    Increments
}while 条件表达式
- 相当于java中的do while 循环 , 循环最少执行一次
```swift
var tr = 1
repeat{
    tr += 2
    print(tr)
}while tr < 10
```
`break`
立即结束循环

`continue`
结束当前循环体的内容，执行下一次循环
```swift
while true{
    let a = arc4random_uniform(6) + 1
    let b = arc4random_uniform(6) + 1

    if a == b {
        print("打平")
        continue
    }

    let winner = a > b  ? "a" : "b"
    print("\(winner) win")
    break
}
```
###### 选择结构： (条件不需要用小括号)
```swift
if … {

}else if .. {

}else{

}
```
swtch 的每个分支不需要写`break`
```swift
switch consider {
    case value1:
        …
    case value2:
        …
    default:
        ...
}
```
```swift
let m1 = "patrick"
switch m1 {
    case "patrick":
        print("right1")
    case "carina":
        print("right2")
    default:
        print("wrong")
}
```
每个`case`可以判断多个值， 每个值用 `,` 分隔：
```swift
let s = "a"
switch s {
    case "a","A":
        print("right")
    default:
        break
}
```
`switch` 可以判断所有基础类型

`switch` 中的`default` 设置空语句时可以用 `break` 和 `（）`

`switch` 可以判断区间值 ,元组:
```swift
let score = 60
switch score {
case 0..<60:
    print("不及格")
case 60..<80:
    print("良好")
case 80..<100:
    print("优秀")
default:
    break
}
```
```swift
let point = (1,1)
switch point {
case (0 , 0):
    print("原点")
case (_,0):
    print("X轴")
case (0,_):
    print("Y轴")
case (-10...10 , -10...10):
    print("坐标是 \(point.0) ,\(point.1)")
default:
    print("point")
}
```
`case` 语句后用`fallthrough` 可以在执行完当前`case` 语句后执行下一个`case`语句
`case` 的判断条件后可以加上 < `where` 逻辑表达式 > 对`case`条件进行更多的限制判断
```swift
let p2 = (2 , 2)
switch p2 {
case let(x, y) where x>y:
    print("/(x) > /(y)")
case let(x, y) where x==y:
    print("/(x) = /(y)")
case let(x, y) where x<y:
    print("/(x) < /(y)")
default:
    break
}
```
`case` 可以与`if` 结合使用简化代码：表达式书写时值在前 变量在后
```swift
var l1 = 3
if case 1...10 = l1{
    print("right")
}
```
`case` 与`for` `where`结合使用:
```swift
for i in 1...100{
    if i%2 == 0{
        print(i)
    }
}
```
```swift
for case let i in 1...100 where i%2 == 0{
    print(i)
}
```
###### 控制转移
`break` `continue` `fallthrough` `return` `throw`

## `guard`: 使代码更易读，避免多层嵌套判断
```swift
func c1 (a:Int , b:Int , c:Double ,d:Double) {
    if a > b {
        if c > d {
            print("right")
        }else{
            print("wrong2")
        }
    }else{
        print("wrong1")
    }
}
```
相当于
```swift
func c2 (a:Int , b:Int , c:Double ,d:Double){
    guard a > b else{
        print("wrong1")
        return
    }
    guard c > d else{
        print("wrong2")
        return
    }
    print("right")
}
```
## String:
`str.isEmpty` ,`String`的成员变量判断`string`是否为空

遍历字符串中的字符：
```swift
var s1 = "patrick"
for i in s1.characters{
    print(i)
}
```
`append` 方法在字符串末尾拼接新的字符： 此方法会改变字符串本身
```swift
s1.append("xl")
```
判断字符串的长度：
```swift
s1.characters.count
```
`String.Index`
对字符串中的字符进行索引：

通过`str.startIndex` 和 `str.endIndex` 获取字符串起始和结束位置的索引 ， 通过改索引获取起始和结束位置字符（`endIndex`放回的索引是最后一个字符位置+1）
```swift
let start_index = s1.startIndex
start_index
s1[start_index]
```
从起始位置开始索引指定位置的字符：
```swift
s1[s1.index(start_index, offsetBy:5)]
```
从结束位置往回索引指定位置字符：
```swift
s1[s1.index(end_index , offsetBy:-3)]
```
获取指定索引下一个索引的字符：
```swift
s1[s1.index(after: start_index)]
```
获取指定索引的前一个索引的字符：
```swift
s1[s1.index(before: end_index)]
```
通过索引区间获得字符串的子串：
```swift
var end = s1.index(before: end_index)
s1[start_index...end]
```
在指定索引位置插入一个字符：
```swift
s1.insert("s", at:end)
```
`Range`:

两个字符串索引形成的区间返回一个`range`
```swift
var r1 = start_index...end
```
也可以通过`range`方法返回指定子串在字符串的`range`位置：
```swift
var r2 = s1.range(of: "pat")
```
将字符串全部大写：
```swift
s1.uppercased()
```
将字符串全部小写：
```swift
s1.lowercased()
```
将每个单词的首字母大写：
```swift
s1.capitalized
```
字符串中是否包含指定子串：
```swift
s1.contains("pa")
```
判读字符串的前缀 后缀：
```swift
s1.hasPrefix("pa")
s1.hasSuffix("xl")
```
`NSString`: OC语言中的字符串类

通过 as 转化：
```
s as NSString
```
