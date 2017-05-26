# Swift 02
----
## 可选型：(Optional)
变量声明时在数据类型后加?，表示该变量是可选型；（必须显示指定）
可选性表示该变量可以赋值为指定类型或`nil`类型：
```swift
var code:Int? = 200
code = 0
code = nil
```
`nil` 单独的特殊类型 ，代表没有的意思, 只有可选型变量才能赋值为nil

可选型的使用需要解包：`unwrap`
> 强制解包： 在可选型变量后加！

> if let 解包：将可选型变量在if 表达式中赋值给另一个新的常量使用, 常量的名字可以与可选型变量名字一样
解包后的获得的新的常量只能在if 后的{ }内使用
新的常量的值就是可选类型变量中的非空的值，这样可以保证不会出现空指针错误
```swift
var errorCode:String? = "500"
if let errorCode = errorCode {
    "The code is " + errorCode
}
```
同时需要对多个可选型进行解包：
```swift
var code:Int? = 200
var status:String? = "normal"
if let code = code , let status = status{
    print(code)
    print(status)
}
```
###### `optional chaining`
```swift
if let status=status {
    status.uppercased()
}
//在可选型后加问号表示对可选型进行尝试解包，解包成功调用后续方法，失败则不执行
//下面的写法等同于上面的写法 ， 下面的表达式返回的是一个可选型， 返回的可选型还可以通过?.进行解包，形成opptional chaining
status?.uppercased()
```
Swift3 中`if` 语句的表达式可选型不支持`where` ，需要增加判断条件 直接用 , 取代`where`即可

`nil coalesce 聚合运算符 ??`
在可选型后接2个问号，在加上默认值，当该可选型是nil时返回默认值，否则返回可选型变量的值，相当于三目运算符
```swift
let status1 = status == nil ? "error" : status
let status2 = status ?? “error” //此句等于上面一句
```
Explain：status2 需要取得status这个可选型的值，当status是nil时则将默认的error赋值给status2，否则将status的值赋给status2
###### 隐式可选型：
在指定类型后加 `!` 将该变量指定为隐式可选型，隐式可选型变量在使用时不需要解包，直接使用即可，但是需要保证在使用时该对象有值，如果是`nil`会报错
常用于类的初始化
```swift
var error1:String! = "隐式可选型"
"this is" + error1
```

## 数组：
数组中的数据类型需要一致:
```swift
var array1 = [1,2,3,4]
var array2 = ["a" , "b" , "c"]
```
显式声明数据内的元素类型：
```swift
var array3:[Int] = [3,4,5]
var array4:[Character] = ["c" , "d"]
```
或者使用泛型指定：
```swift
var array5:Array<String> = ["cc" ,"dd"]
```
初始化数组时指定长度和数组内元素的初始值：
```swift
var array6 = [String] (repeatElement("a", count: 5))
var array7 = [Int] (repeatElement(0, count: 5))
```
创建空的数组：
```swift
var array8 = [Int]()
```
获得数组长度：
```swift
array8.count
array7.count
```
判断数组是否为空：
```swift
array8.isEmpty
```
通过索引获得元素值：
```swift
array6[0]
```
获得数组的起始值和结尾值：返回值是可选型，因为数组可能是空数组，其内的元素可能是nil
```swift
array5.first
array5.last
```
获取数组中的最大值和最小值： 返回值是可选型
```swift
array4.min()
array3.max()
```
通过区间获得数组的指定范围的子数组：
```swift
array2[1..<2]
```
判断数组中是否包含指定元素： 返回boolean
```swift
array2.contains("b")
```
获得元素在数组中的索引位置：返回Int的可选型，该元素不在数组中时返回nil
```swift
array2.index(of: "b")
array3.index(of: 3)
```
遍历数组：
```swift
for value in array4 {
    print(value)
}
```
遍历数组获得索引和值：
```swift
for (index ,value) in array4.enumerated(){
    print("\(index) : \(value)")
}
```
向数组末位增加元素：
```swift
array1.append(5)
array2.append("d")
```
通过 += 一个新的数组给数组增加元素：
```swift
array2 += ["e"]
```
在数组指定的索引位置增加元素：
```swift
array2.insert("f", at: 2)
```
删除数组末位的元素：Int参数表示删除从末位算起的几个元素
```swift
array2.removeLast(1)
```
删除数组起始位的元素：Int参数表示删除从起始位算起的几个元素
```swift
array2.removeFirst(1)
//调用不带参数的removeFirst 和removeLast 删除起始和结束位的元素并返回该元素
array2.removeLast()
array2.removeFirst()
```
删除数组指定索引位置的元素：返回的是该被删除的元素
```swift
array2.remove(at: 2)
```
删除数组的中的所有元素，删除后数组变成空数组：
```swift
array2.removeAll()
```
通过索引修改数组中的值：
```swift
array2[0] = "v"
```
通过区间修改数组中指定范围的值：
```swift
array2[1...3] = ["l" , "m" , "n"]
```
如果区间给区间赋值的数组中只有一个元素，那么被修改的数组中指定区间的值会变成一个值
```swift
array2[1...3] = ["q"]
// 数组值将会由[“v” ,”l” ,”m” ,”n”] 变成 [“v” ,”q"]
```
对数组进行排序：从小到大
```swift
var array = ["cty" , "b" , "d" , "g" ,"中国"]
array.sort()
```
> NSArray: OC语言的数组类
通过 as 转化：
```v
array as NSArray
```
NSArray中的元素可以是不同的数据类型
```swift
var nArray:NSArray = [1, "a" , 3.05 , true]
```

## Dictionary:字典
键-值对数据类型的无序集合，字典内所有键的类型必须一致，所有值的类型也必须一致
```swift
var d1 = ["name":"patrick" , "age":"30" , "sex":"male"]
```
显式声明字典中键和值的数据类型：两种方式
```swift
var d2:[String :Int] = ["1":1 , "2":2 , "3":3]

var d3:Dictionary<String , Int> = ["1":1 , "2":2 , "3":3]
```
创建空字典：
```swift
var emptyDic:[String:String] = [:]

var emptyDic2:Dictionary<String,Int> = [:]

var emptyDic1 = [String:Int]()

var emptyDic3 = Dictionary<String,String>()
```
通过键获得字典的值：返回的是可选型，当字典没有这个键时返回nil
```swift
var age = d1["age"]
```
通过键获得的字典的值用解包进行操作：
```swift
if let age = d1["age"]{
    print(age)
}
```
获取字典中键-值对的数量：
```swift
d1.count
```
是否为空
```swift
d1.isEmpty
```
获得字典中的所有键的集合：返回的是一个数组
```swift
Array(d1.keys)
```
获得字典中的所有值的集合：返回的是一个数组
```swift
Array(d1.values)
```
遍历所有的键：值也一样
```swift
for key in d1.keys{
    print(key)
}
```
通过元组遍历所有的键-值：
```swift
for (key , value) in d1{
    print("\(key) : \(value)")
}
```
修改字典中的值：

通过键索引修改 返回修改后的值
```swift
d1["age"] = "31"
```
通过updateValue方法修改返回的是修改前的值
```swift
d1.updateValue("32", forKey: "age")

var user = ["username":"patrick" , "password":"123"]
let oldPassword = user.updateValue("123", forKey: "password")
if let oldPassword = oldPassword , let newPassword = user["password"] , oldPassword == newPassword{
    print("password id same")
}
```
字典中增加键-值：
直接通过给不存在的键赋值完成
```swift
user["email"] = "patrick@qq.com"
```
updateValue也一样，返回的是nil
```swift
user.updateValue("30", forKey: "age")
```
字典的删除：
给现有的键赋值`nil`
```swift
user["age"] = nil
```
通过removeValue 方法，传入key参数 ， 返回的是删除掉的值 可选型
```swift
user.removeValue(forKey: "email")
```
清空字典
```swift
user.removeAll()
```

## 集合：Set
相对于数组查找速度快
无序的数据集, 集合中元素是唯一的，集合会自动去重
```swift
var chars:Set<String> = ["a" , "b" , "c"]

var nums:Set<Int> = [1,2,3]
```
数组可以很方便的转换为集合：Set ( )
```swift
var nums1 = Set([1,2,3,4,5])
```
获取集合中的元素个数：（重复的元素不会计数）
```swift
nums1.count
```
`isEmpty` 判空

集合中取出第一个元素，因为集合是无序的，所以取出的元素是随机的 ， 返回的是可选型：
```swift
nums1.first
```
是否包含某个元素：
```swift
nums1.contains(1)
```
遍历集合中的元素：
```swift
for n in nums1 {
    print(n)
}
```
增加元素：
```swift
nums1.insert(6)
```
删除元素： 返回删除的元素
```swift
nums1.remove(3)
removeAll() 删除全部元素 ， 集合变空
```
集合的并集： 取2个集合的并集
```swift
var nums2:Set<Int> = [1,2,3,4]
var nums3:Set<Int> = [3,4,5,6]
nums2.union(nums3)
```
集合的交集：
```v
nums2.intersection(nums3)
```
集合的减法：从前一个集合中减去2个集合交集的部分
```swift
nums2.subtract(nums3)
```
判断一个集合是否是另一个集合的子集：
```swift
var nums2:Set<Int> = [1,2,3,4]
var nums4:Set<Int> = [1]
nums4.isSubset(of: nums2)
nums4.isStrictSubset(of: nums2)  //真子集
```
反向判断：判断父集
```swift
nums4.isSuperset(of: nums2)
nums4.isStrictSuperset(of: nums2)
```
判断2个集合是否相离（没有相同的元素）
```swift
nums4.isDisjoint(with: nums2)
```

## 函数：
```swift
func 函数名( 参数：参数类型 ， …) -> 返回值参数{
    ...
}
```
```swift
func run(name:String) -> String{
    return name + " running"
}
run(name: "patrick")
```
函数的参数可以是可选型， 可选型参数使用时需要注意解包：
```swift
func run1(name:String?) -> String{
    return (name ?? "no one") + " running"
}
var testName:String? = nil
run1(name: testName)
```
无参数和返回值的函数定义： 3种
```swift
func run2() {
    print("running")
}
```
```swift
func run3() -> () {
    print("running")
}
```
```swift
func run4() -> Void {
    pring("running")
}
```
可以使用元组让函数返回多个值：
```swift
func jiSuan (a:Int , b:Int) -> (sum:Int ,subtraction:Int , product:Int , quotient:Int)?{

//    if b == 0 {
//        return nil
//    }
    guard b != 0 else {
        return nil
    }
    return(a+b , a-b , a*b , a/b)
}

if let result = jiSuan(a: 3, b: 2) {
    print(result)
}
// 计算结果： (5, 1, 6, 1)
// 使用可选型返回结果 ，当b是0时返回nil 避免除法分母是0时的错误
```
函数的外部参数名和内部参数名：外部参数名供调用时使用，内部参数名在函数内部使用
外部参数名和内部参数名用空格分隔 ，调用时内部参数名会自动被屏蔽掉
```swift
func 函数名 （外部参数名 内部参数名：参数类型）-> 返回值类型 {
    return ...
}
```
```swift
func talk (with name:String) -> String{
    return "I'm talking with " + name
}

talk(with: "carina")
```
不需要外部参数名时通过 _ 省略
```swift
func sum (_ num1:Int , _ num2:Int) -> Int{
    return num1 + num2
}
sum(1, 2)
```
参数可以设置默认值，当调用时没有传入该参数就使用默认值：
```swift
func learn (course:String = "swift") -> String {
    return "I'm learning "+course
}
learn(course:"java")
learn()
```
可变长度参数：在参数类型后加 … 此时的参数可以当做数组看待
一个函数只能有一个变长的参数
```swift
func 函数名 (参数名：参数类型 … ) -> 返回类型 {
    return ...
}
```
```swift
func maxIn (nums:Int ...) -> Int?{
    guard !nums.isEmpty else {
        return nil
    }
    var max = nums [0]
    for n in nums{
        max = max > n ? max : n
    }
    return max
}
maxIn(nums: 1,2,3,4,7,8,99,12)
```

函数的参数默认是let ，在函数体内无法变更 , 可以在函数体内设置新的变量接收参数值
```swift
func getDouble(n:Int) ->Int {
    var n1:Int = n
    n1+=2
    return n1
}
getDouble(n:2)
```
或通过指定调用引用: 在参数的类型前增加 inout 关键字
表示按引用传递参数 ， 在函数体内改变参数值后，外部的变量值也会改变
```swift
func getDouble1(n:inout Int) ->Int{
    n+=2
    return n
}
getDouble(n: 6)

func swapNumber(n1:inout Int ,n2:inout Int) {
    (n1,n2) = (n2, n1)
}
var x = 1
var y = 2
swapNumber(n1: &x, n2: &y)
x
y
// swift可以利用元组交换2个数 ， inout关键字限制的参数，使用时在传入的变量前加 &符号
```

## 函数式编程：
将函数当做变量使用，将函数名赋值给一个变量，该变量就拥有了函数的功能
```swift
func sum1 (a:Int ,b:Int) -> Int{
    return a + b
}
let sum2 = sum1
sum2(1,2)
```
赋值为函数的变量称为函数型变量
函数型变量的类型声明：
```swift
let sum3:(Int ,Int) -> Int = sum1
```
参数值有一个时 ，( )可以省略
没有返回值时使用  ->( ) 或 ->Void
没有参数也没有返回值时：
```swift
( ) ->( )
( ) -> Void
Void ->( )
Void -> Void
```
数组的`sort`函数可以传入一个函数型变量作为参数用来指定排序的方法：
```swift
array.sort(by: <#T##(String, String) -> Bool#>)
```
通过函数型变量自定义数组从大到小排序
```swift
var array1 = [4,7,2,99,22]
func bigSort(a:Int , b:Int) -> Bool {
    return a > b
}
array1.sort(by: bigSort)
```

## 高阶函数：
高阶函数接收另外一个函数作为参数，在高阶函数体内使用另外一个函数
```swift
func showInfo (name:String) -> String{
    return "my name is "+name
}

func getInfo (name:String , f1:(String)->String) {
    print(f1(name) , "i'm 31")
}

let name = "patrick"
getInfo(name: name, f1: showInfo)
```
数组的`map`函数就是一个高阶函数，根据传入的函数参数对数组的数据遍历进行操作：
执行`map`操作后返回一个新的数组，并不改变原数组中的数据
```swift
var a1 = [2,3,4,5]
func add5 (a:Int) -> Int{
    return a+5
}
a1.map(add5)
```
`filter`高阶函数用于对数组中的数据进行过滤：
```swift
func biggerThan3(a:Int) ->Bool{
    return a > 3
}
a1.filter(biggerThan3)
```
`reduce` 从首位开始对数组中的两个数进行指定操作，并将第一次计算的结果作为参数与后一位元素进行操作：
```swift
[1, 2, 3, 4, 5]   ==>> 1+2= 3 ==>> 3 +3=6 ==>> 6+4=10 ==>> 10+5=15
```
`reduce`接收一个初值作为计算的起始值
```swift
func sum3 (a:Int , b :Int) -> Int{
    return a+b
}
a1.reduce(0, sum3)
//计算结果为14
```
函数也可以做为函数的返回值返回：
```swift
func r1(n:Int) -> String{
    return String(n) + " is a 正数"
}

func r2(n:Int) -> String{
    return String(n) + " is a 负数"
}

func validateNum (n:Int) -> (Int) -> String{
    return n>0 ? r1 : r2
}

func show1 (n:Int) ->Void {
    let validate = validateNum(n:n)
    print("经过计算" + validate(n))
}
var n1 = 7
show1(n: n1)
```
函数中还可以嵌套其他函数: 函数内部嵌套的函数只能在该函数内部使用
上例的validateNum函数可以放到show1函数内部：
```swift
func r1(n:Int) -> String{
    return String(n) + " is a 正数"
}

func r2(n:Int) -> String{
    return String(n) + " is a 负数"
}

func show1 (n:Int) ->Void {
    func validateNum (n:Int) -> (Int) -> String{
        return n>0 ? r1 : r2
    }
    let validate = validateNum(n:n)
    print("经过计算" + validate(n))
}
var n1 = 7
show1(n: n1)
```

## 闭包：
好像是一个匿名函数,`lambada`表达式，当一个函数只使用一次时使用：
语法 ： 闭包没有外部参数名
```swift
{ ( 参数名：参数类型 ， …) -> 返回值类型 in
    Statement
}
```
```swift
var aa = [1,2,3,4,5,6]
aa.sort(by: {(a:Int ,b:Int) ->Bool in
    return a > b
})
```
闭包语法的简化：
```swift
aa.sort(by: {(a:Int ,b:Int) ->Bool in
    return a > b
})
```
当内部语句只有一句时 可以直接放在in 后面：
```swift
aa.sort(by: {(a:Int ,b:Int) -> Bool in return a > b })
```
使用闭包是闭包函数的类型都是被函数定义好了的，因此类型定义可以省略：
```swift
aa.sort(by: { a , b in return a > b})  //推荐写法
```
return关键字也可以省略：
```swift
aa.sort(by: { a , b in a > b})
```
结尾闭包：trailing closeure
当把闭包当做函数变量作为最后一个参数传入时，闭包可以写到( )的外面，方便阅读：
```swift
aa.sort(by: { a , b in return a > b})
                ==
aa.sort(){a , b in return a > b}
                ==  （ ）也可以省略
aa.sort{a , b in return a > b}
```
闭包的内容捕获：
当使用函数型变量时，如果需要在作为变量的函数内传入一个参数，但是函数没有提供传参的接口，使用闭包可以实现
```swift
var aa1 = [1,2,3,4,5,6,7,8]
var key = 5
aa1.filter(){ a in
    return a > key
}
```
