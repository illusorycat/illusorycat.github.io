---
title: Dart 简述
date: 2024-05-20 09:25:31
tags: 语法
---
# 目录
[前言](#0)
[基础表达式](#1)
[类型](#2)
[模式匹配](#3)
[函数方法](#4)
[控制流](#5)
[错误处理](#6)
[类 & 对象](#7)
[类修饰符](#8)
[并发](#9)
[空安全](#10)

<h1 id = "0">前言</h1>

本文简单整理了来自 [Dart 官网的《 Dart 开发语言 》](https://dart.cn/language)章节内容。仅用于简单理解 Dart 的语法。<br>

<h1 id="1">基础表达式</h1>

## 变量
以下是创建并初始化变量的例子：<br>
```dart
var name = 'Bob';
```
使用 `var` 定义了一个变量，并且通过初始化赋值触发类型推导确定变量的类型。<br>
如果你明确变量的类型，也可以直接用具体类型替换 `var` 。<br>
Dart 支持空值。<br>
```dart
String? name    //Nullable type. Can be `null` or string
String name     //Non-nullable type.
```
你必须在使用变量之前对其进行初始化，Dart 不会为非可空类型设置默认初始值。而可空变量是默认初始化为 null 的。也就是说对于非可空类型变量，在声明时要提供一个默认初始值。<br>
Dart 中的非可空变量类型都继承于 Object ，因此你可以使用 `Object` 类型来定义一个可以存储任意非可空类型数据的变量。<br>
如果你必须推迟变量的类型检测到运行时，那你可以使用特殊类型 `dynamic` 。<br>
Dart 提供了一个关键字 `late` 。该关键字允许你声明非可空类型变量时不初始化，支持在第一次使用到该变量时才执行初始化流程。<br>
如果你不打算更改一个变量，可以使用 `final` 或 `const` 。
```dart
final name = 'Bob';
final String nickname = 'Bobby';
var foo = const [];
final bar = const [];
const baz = [];
```
final 对象不能被修改，但是其字段可能可以被修改。 const 对象本身及其内容不能被更改。
你可以在定义常量时使用类型检查 `is` 和转换 `as` 、集合中的 `if` 和展开操作符 (`...` 和 `...?`) 。<br>
```dart
const object i = 3;
const list = [i as int];
const map = {if (i is int) i: 'int'};
const set = {if (list is List<int>) ...list}
```
## 操作符
操作符与其他语言中的大体相符。此处只介绍一些不常见的操作符。<br>
 
 1. ！
    - 作为后缀操作符，用于强调表达式的计算结果永远不会是 null
    - 作为前缀操作符号，则是常见的反转值功能
 2. ～/
    - 除法，但是返回整数结果
 3. is!
    - 与 is 操作符的结果相反
4. ??= 
    - 仅在被赋值对象为 null 时进行赋值
5. >>>
    - 无符号右移
6. .. 和 ?.. 
    - 级联操作符，用于省略调用的变量
        ```dart
        var paint = Paint()
        ..color = Colors.black  // paint.color = COlors.black
        ```
7. ... 和 ...?
    - 这其实不是一个运算符，而是集合本身的一部分，是一种将一个集合中的多个值插入到集合中的简洁方法
        ```dart
        var list = [1,2,3];
        var list2 = [0, ...list];
        ```

## 注释
注释的写法与常见语言中的一致。<br>
## 注解
元数据注解以 `@` 开始。<br>
所有 Dart 代码中都内置了四个注解：@Deprecated 、@deprecated 、@override 和 @pragma 。<br>
@Deprecated 和 @deprecated 的区别在于前者可以指定消息。<br>
```dart
@Deprecated('Use turnOn instead')
```
我们可以定义自己的注解。<br>
```dart
class Todo {
    final String who;
    final String what;

    const Todo(this.who, this.what);
}
@Todo('Dash', 'Implement this function')
void doSomething() {
    print('Do something');
}
```
## 库和导库
Dart 使用 import 关键字来导入库。<br>
并且 Dart 没有其他语言中的那些访问权限关键字，而是仅仅使用如下规则：<br>

- 以下划线 `_` 开通的标识符仅在库中可见。

下面是 Dart 中的一个导库的例子：<br>
```dart
import 'dart:html';
import 'package:test/test.dart';
```
我们可以仅加载库的部分：<br>
```dart
//Import only foo.
import 'package:lib1/lib1.dart' show foo;

//Import all banes EXCEPT foo.
import 'package:lib2/lib2.dart' hide foo;
```
当导入的多个库中具有冲突的标识符时，我们可以为库指定前缀：<br>
```dart
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;

Element elememt1 = Element();
lib2.Element element2 = lib2.Element();
```
如果你在使用 Web 相关功能，你可能会需要 Dart 延迟加载库的功能，请查阅文档。<br>

<h1 id="2">类型</h1>

## 基本类型
Dart 中支持的基本数据类型与其他面对对象的语言类似。<br>
值得注意的是 Dart 中的字符串的保存使用 UTF-16 。<br>
同时它可以使用 r 前缀来输出不转义的字符串：<br>
```dart
var s = r'In a raw string, not even \n gets special treatment.'
```
同时，我们可以使用 `$` 将变量放入字符串中，也可以使用 `${}` 将表达式放入字符串中：<br>
```dart
var s = 'string interpolation';
var m = {1: 'int'};

print('Dart has $s, which is very handy.');
print('${m}');
```
Dart 是类型安全语言，这也就意味着你不能不使用像 `if(nonbooleanValue)` 这样的代码。<br>
## Records 类型
Records 是匿名的、不可变的聚合类型。<br>
```dart
var record = ('first', a: 2, b: true, 'last');  // this type is (String, int, bool, String)
```
我们可以使用占位符或者字段描述名访问字段值：<br>
```dart
print(record.$1);   // Prints 'first'   
print(record.a);
```
Records 类型对象是不可变的，其只有 getter 字段，而没有 setting 字段。<br>
字段描述名不影响 Records 本身的相等性。<br>
我们可以使用模式匹配将 Records 值解构为局部局部变量：<br>
```dart
(String name, int age) userInfo(Map<String, dynamic> json) {
    retur (json['name'] as String, json['age'] as int);
}

final json = <String, dynamic> {
    'name': 'Dash',
    'age': 10,
    'color': 'blue'
};

var (name, age) = userInfo(json);
```
## 集合
Dart 的集合包括 List 、Set 和 Map
同时支持使用 `...` 、`...?` 、 `if` 、`for` 、`if-case` 构建集合。<br>
```dart
var list = [1,2,3];
var list2 = [0, ...list];
assert(list2.length == 4);

List<int>? list3 = null;
var list4 = [0, ...?list3];
assert(list4.length == 1);

var nav = ['Home', 'Furniture', 'Plants', if (promoActive) 'Outlet'];    //if promoActive is true, this list has 4 elements, otherwise only 3 elements.

var nav1 = ['Home', 'Furniture', 'Plants', if (login case 'Manager') 'Inventory'];

var listOfInts = [1, 2, 3];
var listOfStrings = ['#0', for (var i in listOfInts) '#$i'];
assert(listOfStrings[1] == '#1');
```
## 泛型
```dart
abstrct class Cache<T> {
    T getByKey(String key);
    void setByKey(String key, T value);
}

class Foo<T extends Object> {
    //Any type provided to Foo for T must be non-nullable
}

T first<T>(List<T> ts) {
    T tmp = ts[0];

    return tmp;
}
```
## 别名
```dart
typedef IntList = List<int>;

typedef ListMapper<X> = Map<X, List<X>>;
```
对于函数，我们推荐使用内联函数类型而不是函数等 typedef。<br>
```dart
typedef Compare<T> = int Function(T a, T b);

int sort(int a, int b) => a - b;

void main() {
    assert(sort is Compare<int>);   //True
}
```
## 类型系统
Dart 支持在重写方法时，使用一个新类型（在新方法中）替换旧类型（在旧方法中）。类似的，当参数传递给函数时，可以使用另一种类型（实际参数）的对象替换现有类型（具有声明类型的参数）要求的对象。从生产者和消费者的角度来看，消费者接受类型，生产者产生类型。可以使用父类型替换消费者类型，使用子类型替换生产者类型。<br>
```dart
Cat c = Cat();
Animal a = Cat();
Cat c2 = MaineCoon();
```
赋值运算符左边是消费者，右边是生产者。<br>

<h1 id="3">模式匹配</h1>

## 概览
```dart
switch (number){
    case 1:
        print('one');
    //Matches of tje value pf pbj is between the constant values of 'first' and 'last'
    case >= first && <= last:
        print('in range');
}

const a = 'a';
const b = 'b';
switch (obj) {
    case [a,b]:
        print('$a, $b');
}

var numList = [1, 2, 3];
var [a, b, c] = numList;
print(a + b + c);

//下面匹配了一个第一个元素是 'a' 或 'b' 的两元素列表
switch (list) {
    case ['a' || 'b', var c]:
        print(c);
}
```
总的来说，Dart 提供的模式匹配功能十分强大。<br>
我们可以使用模式匹配很方便地实现交换两个变量的值。<br>
```dart
var (a, b) = ('left', 'right');
(b, a) = (a, b);
print('$a $b');
```
可以让多个 case 共享一个 body：<br>
```dart
var isPrimary = switch (color) {
    Color.red || Color.yellow || Color.blue => true,
    _ => false
}
```
可以在 case 中增加添加：<br>
```dart
switch (shape) {
    case Square(size: var s) || Circle(size: var s) when s> 0:
        print('Non-empty symmetric shape');
}
```
将 getter 调用的结果绑定到同名变量：<br>
```dart
Map<String, int> hist {
    'a': 23,
    'b': 100,
};

for (var MapEntry(:key, value: count) in hist.entries) {
    ///
}
```
上述 `.entries` 返回的是一个 `Iterable<MapENtry<K, V>>` ，因为其字段 key 与我们要赋值的变量 key 同名，因此可以将 `key:key` 简化为 `:key` 。<br>
我们可以使用模式将 Records 的字段直接结构为局部变量，与函数内联。也可以使用模式解构类实例。<br>
```dart
var (name, age) = userInfo(json);

final Foo myFoo = Foo(one: 'one', tew: 2);
var Foo(:one, :teo) = myFoo;
```
使用模式实现函数的策略设计模式：<br>
```dart
sealed class Shape{}

class Square implements Shape {
    final double length;
    Square(this.length);
}

class Circle implements Shape {
    final double radius;
    Circle(this. radius);
}

double calculateArea(Shape shape) => switch (shape) {
    Square(;ength: var l) => l * l,
    Circle(radius: var r) => math.pi * r * r
};
```
使用模式我们可以很便捷的验证并使用 json 结构中的数据：<br>
```dart
if (json case {'user': [String name, int age]}) {
    print('User $name is $age years old.');
}
```
## 模式匹配类型
模式匹配与逻辑组合使用时同样有运算优先级存在。<br>
我们还可以使用模式匹配来实现空校验或空断言：<br>
```dart
String? maybeString = 'nullable with base type String';
switch (maybeString) {
    case var s?:
    // 's' has type non-nullable String here.
}

List<String?> row = ['user', null];
switch (row) {
    case ['user', var name!]: //...
    //'name' is a non-nullable here.
}
```

<h1 id="4">函数方法</h1>

Dart 中的函数也是对象，类型是 `Function`。<br>
```dart
bool isNoble(int atomicNumber) {
    return _nobleGases[atomicNumber] != null;
}
```
```dart
bool isBoble(int atomicNumber) => _nobleGases[atomicNumber] != null;
```
上述第二种函数声明是第一种的简写语法，仅当函数中只包含一个表达式时可用。也就是说你不可以在 `=>` 之后放一个 if 语句。<br>
`=> expr` 语法是 `{ return expr; }` 的简写。（类似于其他语言中的宏定义。<br>
一个函数可以有任意数量的普通参数。这些参数后面可以跟着命名参数和位置参数（但不能同时跟着这两个参数）。<br>
函数命名参数和位置参数都可以指定默认值，但必须是编译时常量。普通参数不能指定默认值。<br>
## 命名参数
定义函数时，使用 `{param1, param2, ...}` 指定命名参数。如果你没有**提供默认值**或将命名参数标记为 `required`,则它们类型必须是可空的，因为 Dart 提供的默认值是 null。<br>
```dart
void enableFlags(int a, {bool? bold, bool hidden = false, required Widget child}) {...}
```
命名参数指这些参数必须通过命名来传递。<br>
## 位置参数
定义函数时，使用 `[param1, param2, ...]` 指定位置参数。如果不提供默认值，则它们的参数类型必须是可空的。<br>
位置参数要求传参时安装定义的顺序传递入参。<br>
```dart
String say(String from, String msg, [String? device]) {
    var result = '$from says $msg';
    if (deivce != null) {
        result = '$result with a $device';
    }
    return result;
}
```
## main 函数
main 作为 flutter 应用程序的入口点。<br>
```dart
void main(List<String> args) {
    ...
}
```
## 函数闭包
Dart 支持函数闭包。<br>
```dart
Function makeAdder(int addBy) {
    return (int i) => addBy + i;
}

void main() {
    var add2 = makeAdder(2);

    assert(add2(3) == 5);
}
```
Dart 闭包访问的是其词法作用域中的变量。如果原变量的值在被捕捉之后发生了变化，也不会影响 Dart 闭包中已经捕捉的值。<br>
## 生成器
当你需要延迟产生一个值序列时，可以考虑使用生成器函数。Dart 内置支持两种生成器函数：<br>

- 同步生成器：返回一个 Iterable 对象
- 异步生成器：返回一个 Stream 对象

#### 实现同步生成器
使用 `sync*` 标记函数，并使用 `yield` 语句来传递值：
```dart
Iterable<int> naturalsTo(int n) sync* {
    int k = 0;
    while (k < n) yield k++;
}
```
#### 实现异步生成器
使用 `async*` 标记函数，并使用 `yield` 语句传递值。<br>
```dart
    Stream<int> asynchronousNaturalsTo(int n) async* {
        int k = 0;
        while (k < n) yield k++;
    }
```
#### 使用 `yield*` 提高递归生成器函数的性能
```dart
Iterable<int> naturalsDownFrom(int n) sync* {
    if (n > 0) {
        yield n;
        yield* naturalsDownFrom(n - 1);
    }
}
```
### 外部函数
外部函数是其主体与其声明分开实现的函数。使用 `external` 关键字。<br>
```dart
external void someFunc(int i);
```

<h1 id="5">控制流</h1>

## 循环
Dart 支持 for 循环、while 和 do-while 回路，以及 break 和 continue。<br>
```dart
var callbacks = [];
for (var i = 0; i < 2; i++) {
callbacks.add(() => print(i));
}

for (final c in callbacks) {
c();
}
//The ouput is 0 and then 1, as expected.
```
## 分支
Dart 还支持 if 、 if-case 和 switch 分支语句。<br>
if 后面括号中的条件必须是计算结果为布尔值的表达式。<br>
if-case 语句支持模式匹配。<br>
```dart
if (pair case [int x, int y]) {
    print('Was coordinate array $x, $y');
} else {
    throw FormatException('Invalid coordinates.');
}
```
switch 语句有如下规则：<br>

- 非空的 case 子句中完成后跳转到 switch 的末尾， 不需要 break。
- 空 case 子句会隐式添加 fallthrough
- 可以使用 continue +标签 来实现非顺序的 fallthrough
- 可以使用default 或 _
- 支持进一步条件约束，仅需要在 case 主体之后添加 `when` 子句（if-case 语句中也支持）

```dart
switch (command) {
    case 'OPEN':
        executeOpen();
        continue newCase;
    case 'DENIED':
    case 'CLOSE':
        executeClosed();    //Runs for both DENIED and CLOSED
    newCase:
    case 'PENDING':
        executeNowClosed(); //Runs for both OPEN and PENDING
}
```
从 Dart 3.0 版本开始，支持 switch 表达式。 switch 表达式必须作为语句的一部分，而不能是单独的语句。<br>
```dart
var x = switch (y) {...};   //this will run switch
print(switch (x) {...});
return switch (x) {...};
/*error usage
* switch (x) {...};
*/
```
switch 表达式遵循下述规则：<br>

- Cases 不再需要使用 case 关键字
- case 主体是一个表达式而不是一系列语句。
- 每个 case 都必须有一个 body ，空 case 没有隐式的 fallthrough
- 使用 `=>` 而不是 `:`
- 使用 `,` 分隔 Cases
- 默认只能使用 `_`

```dart
token = switch (charCode) {
    slash || star || plus || minus => operator(charCode),
    comma || semicolon => punctuation(charCode),
    >= digit0 && <= digit9 => number(),
    _ => throw FormatException('Invalid')
}
```

<h1 id="6">错误处理</h1>

Dart 使用 `throw` 抛出异常，默认提供了 Exception 和 Error 类型，但实际上 Dart 可以抛出任意非空对象作为异常。<br>
Dart 使用 `catch` 捕捉异常，使用 `on` 在捕捉时指定异常类型，使用 `rethrow` 传播异常， 使用 `finally` 执行最终执语句。<br>
如果在 finally 子句之前异常匹配任意 catch 字句，则执行完 finally 字句后异常会被隐式传播。<br>
catch 子句允许一个或两个参数，第一个参数时抛出的异常，第二个参数的堆栈跟踪（ StackTrace 对象）。<br>
Dart 中的方法并不声明它们可能抛出哪些异常，并且不需要捕获任何异常。<br>
```dart
void misBehave() {
    try {
        breedMoreLlamas();
    } on OutofLamasException{
        buyMoreLlamas();
    } on Exception catch (e) {
        print('Unknown exception: $e');
        rethrow;
    } catch (e, s) {
        print('Exception details:\n $e');
        print('Stack trace:\n $s');
    } finally {
        cleanLlamaStalls();
    }
}
```

<h1 id="7">类 & 对象</h1>

## 概述
Dart 是一种面向对象的语言，具有类和基于 mixin 的继承。每个对象都是一个类的实例，除 Null 之外的所有类都是从 Object 中派生。尽管每个类（顶层类 Object? 除外）只有一个超类，但是使用 mixin 可以实现类体在多个类层次结构中重用。扩展方法是一种在不更改类或创建子类的情况下向类添加功能的方法。类修饰符允许你控制库如何对类进行子类型化。<br>
#### 成员使用
使用点（`.`）来引用对象的变量或方法。<br>
#### 构造方法重载
构造方法的名称可以是 ClassName 或 Classname.identifier ，构造函数名称前的 new 关键字可以省略：<br>
```dart
var p1 = new Point(2, 2);
var p2 = Point(2, 2);
var p3 = Point.fromJson({'x': 1, 'y': 2});
```
大多数语言重载构造函数的方式是重载同名构造函数，而 Dart 选择给构造函数提供 identifier 来实现重载。<br>
#### 常量对象
有些类提供了常量构造函数，如果要使用该常量构造函数创建编译时常量，要在构造函数名称前使用 `const` 关键字。<br>
```dart
var p = const ImmutablePoint(2, 2);
```
const 关键字作用的一定得是常量构造函数，而不能是普通的构造函数。<br>
如果构造两个相同的编译时常量实际上它们会是一个实例。<br>
```dart
var a = const ImmutablePoint(2, 2);
var b = const ImmutablePoint(2, 2);

assert(identical(a, b));    //They are the same instance!
```
如果是在常量上下文中，你可以省略除第一个 const 关键字之外的其他 const 声明。比如下面的两个语句是等效的：<br>
```dart
const pointAndLine = const {
    'point': const [const ImmutablePoint(2, 2)],
    'line': const [const ImmutablePoint(1, 10), const ImmutablePoint(-2, 11)],
};
const pointAndLine = {
    'point': [ImmutablePoint(2, 2)],
    'line': [ImmutablePoint(1, 10), ImmutablePoint(-2, 11)],
};
```
常量构造函数也可以构建非常量对象：<br>
```dart
var a = ImmutablePoint(2, 2);   // a isn't a constant
```
#### 对象类型
我们可以获取对象的类型：<br>
```dart
print('The type of a is ${a.runtime.Type}');
```
但是，使用 `object as type` 而不是 `object.runtimeType` 会使你的代码更加安全。<br>
#### 变量
对于普通的成员变量，必须设定初始值，可以在声明时，也可以在构造函数参数中，还可以在初始值设定列表中。<br>
```dart
class Point {
    double? x;  //initially null
    double y = 0;
    double z;

    Point(this.z);
    Point.unz(): z = 0;
}
```
没有使用 `late` 关键字描述的成员变量的创建时机为创建实例后，构造函数及其初始值设定列表项执行之前。<br>
```dart
class Point {
    double? x= 1.5;
    //ERROR 
    //double? y = this.x;
    double? y = 1.5;

    late double? y = this.x;

    Point(this.x, this.y);
}
```
所有成员变量都会生成一个隐式的 getter 方法。没有使用 `final` 描述的成员变量，或者使用了 `late final` 描述但是没有初始值设定项的成员变量，都会生成一个隐式的 setter 方法。<br>
使用 `final` 描述的成员变量必须只设置只设置一次值。
```dart
class ProfileMark {
    final String name;
    final DateTime start = DateTime.now();

    ProfileMark(this.name);
    ProfileMark.unnamed(): name = '';
}
```
如果要在构造函数政务启动后才分配 final 成员变量的值，可以使用下述方法：<br>
- 使用工厂构造函数
- 使用 late final 关键字描述。注意，没有初始值设定项的 late final 成员变量会隐式添加一个 setting 方法。
#### 隐式接口
所有类都隐式定义了一个接口，其中包含该类的所有实例成员（不包括类成员）及其实现的任何接口。<br>
类可以通过在 `implements` 子句中声明一个或多个接口，然后提供接口所需的实现。<br>
```dart
class Person {
    final String _name;

    Person(this._name);

    String greet(String who) => 'Hello, $who. I am $_name';
}

class Impostor implements Person {
    String get _name => '';

    String greet(String who) => 'Hello, $who. Do you kone who I am?';
}
```
#### static
使用 `static` 关键字可以在类中定义类变量（也称静态变量）和类方法。<br>
静态变量在使用之前不会初始化。<br>
```dart
class Queue {
    static const initialCapacity = 16;
}
```
类方法不在实例上运行，因此也无权访问 `this.` 。静态方法可以作为编译时常量。<br>
## 构造方法
最常见的构造函数是创建与其类同名的函数来声明构造函数。<br>
```dart
class Point {
    double x = 0;
    double y = 0;

    Point(this.x, this.y);
}
```
一般会使用**初始化形式参数**的功能来简化将构造函数的参数分配给实例变量的过程。<br>
**构造函数是不会继承的。**<br>
#### 默认构造函数
如果未声明构造函数，则会提供默认构造函数。默认构造函数没有参数，并且调用超类中的无参数构造函数。<br>
#### 命名构造函数
命名构造函数可以为类实现多个构造函数。<br>
```dart
const double xOrigin = 0;
const double yOrigin = 0;

class Point {
    final double x;
    final double y;

    Point(this.x, this.y);

    Point.origin(): x = xOrigin, y = yOrigin;
}
```
上述的构造函数还使用到了**初始值设定列表**的功能。<br>
命名构造函数的使用也很方便，就是 `ClassName.identifier()`。<br>
#### 初始值设定列表
```dart
//...
Point.fromJson(Map<String, double> json): x = json['x']!, y = json['y'] {
    print('In Point.fromJson(): ($x, $y)');
}
//...
```
初始值设定列表项的右侧无权访问 this 。
#### 调用非默认超类构造函数
默认情况下，子类中的构造函数会调用超类的未命名、无参数构造函数。执行顺序如下：<br>

- 初始值设定列表
- 超类构造函数
- 构造函数的主体

如果超类没有无参构造函数，或者你需要调用超类的其他构造函数，你就需要手动调用超类的构造函数。<br>
手动调用超类构造函数的方法是在冒号( `:` )之后，函数体之前。<br>
```dart
class Person {
    String? firstName;

    Person.fromJson(Map data) {
        print('in Person');
    }
}

class Employee extends Person {
    Employee.fromJson(super.data) : super.fromJson() {
        print('in Employee');
    }
}

void main {
    var employee = Employee.fromJson({});
    print(employee);
}
```
上述代码中手动调用了超类的构造函数，并且使用了**超级参数**功能将子类构造函数的入参直接分配到超类的构造函数入参上。<br>
#### 超级参数
超级参数功能可以避免手动将每个参数传递到超类的构造函数中。其语法与初始化形式参数的类似。
```dart
class Vector2d {
    final double x;
    final double y;

    Vector2d(this.x, this.y);
}

class Vector3d extends Vector2d {
    final double z;

    Vector3d(super.x, super.y, this.z);
}
```
此功能不能与**重定向构造函数**一起使用。<br>
#### 重定向构造函数
有时，构造函数的唯一目的是重定向都同一个类中的另一个构造函数。<br>
重定向构造函数没有函数体，使用 `this.` 调用其他构造函数。<br>
```dart
class Point {
    double x, y;

    Point(this.x, this.y);

    Point.alongXAxis(double x): this(x, 0);
}
```
#### 常量构造函数
如果类生成的对象永不更改，则可以使这些对象成为编译时常量。为此我们可以定义一个 const 构造函数， 并确保所有成员变量都是 final 。<br>
```dart
class ImmutablePoint {
    static const ImmutablePoint origin = ImmutablePoint(0, 0);

    finnal double x, y;

    const ImmutablePoint(this.x, this.y);
}
```
#### 工厂构造函数
在实现并不总是创建其类的新实例的构造函数时，我们可以使用关键字 `factory` 来声明一个工厂构造函数，它可能会从缓存中返回一个实例，或者返回子类型的实例。<br>
工厂构造函数无法访问 this 。
工厂构造函数的调用与其他构造函数一样。
## 成员方法
成员方法与其他语言的类似。<br>
Dart 允许你使用 `operator` 关键字重载下述的运算符：<br>
`<`     `>`    `<=`     `>=`    `==`    `~`
`-`     `+`     `/`     `~/`    `*`     `%`
`|`     `^`     `&`     `<<`    `>>>`   `>>`
`[]=`   `[]`
#### 抽象方法
只有声明没有实现的方法叫做抽象方法。<br>
抽象方法只能存在于抽象类或混合类中。<br>
使用 `abstrcat` 关键字声明抽象类。<br>
#### getter 和 setter
getter 和 setter 是对对象属性进行读写访问的特殊方法。我们可以使用 `get` 和 `set` 关键字实现 getter 和 setter 来创建其他属性：<br>
```dart
class Rectangle {
    double left, top, width, height;

    Rectangle(this.left, this.top, this.widht, this.height);

    double get right => left + width;
    set right(double value) => left = value - width;
    double get bottom => top + height;
    set bottom => top = value - height;
}

void main() {
    var rect = Rectangle(3, 4, 20, 15);
    assert(rect.left == 3);
    rect.right = 12;
    assert(rect.left == -8);
}
```
#### 特殊的成员方法
如果要实现像调用函数一样使用对象，要在类中定义 call() 方法。<br>
call() 方法的返回值和参数都可以自定义。<br>
```dart
class WannabeFunction {
    String call(String a, String b, String c) => '$a $b $c';
}

var wf = WannabeFunction();
var out = wf('Hi', 'there', 'gang');

void main() => print(out);
```
## 继承
使用 `extends` 关键字表示继承， 使用 `super` 引用超类。<br>
子类可以重写实例方法、 getter 和 setter 。你如果你愿意，你可以使用 @override 批注。<br>
重写需要满足下述要求：<br>

- 返回类型必须与重写方法的返回类型相同或是其子类。
- 参数类型必须与重写方法的参数类型相同或是其超类。
- 参数中，位置参数的数量要一致。
- 泛型方法不能重写非泛型方法，非泛型方法不能重写泛型方法。

有时你可能想要通过使用子类作为参数类型来缩小输入，那请使用 covariant 关键字。
需要注意的是，如果你重写了 `==` 运算符，那你也要重写 hashCode 的 getter 。
#### noSuchMethod()
这个一个特殊的方法，如果你需要可以重写这个方法。<br>
## 混入( Mixin )
Mixins 是一种定义代码的方法，可以在多个类层次结构中重用。它们旨在为成员提供集体实现。<br>
在定义类时，使用 with 子句并声明一个或多个 mixin 。<br>
```dart
class Musician extends Performer with Musical {

}
```
使用 `mixin` 关键字声明一个 mixin 。同时 mixin 需要满足下述规则：<br>

- Mixin 中不能使用 extends 。
- Mixin 中不能声明任何生成构造函数。（也就是只允许一个默认构造函数）

```dart
mixin Musical {
    bool canPlayPiano = false;
    bool canCompose = false;
    bool canConduct = false;

    void entertainMe() {
        if (canPlayPiano) {
            print('Playing piano');
        } else if (canConduct) {
            print('Waving hands');
        } else {
            print('Humming  to self');
        }
    }
}
```
有时候你希望限制可以使用 mixin 的类型，则可以在声明 mixin 时使用 `on` 关键字来指定用于限制的超类。<br>
这样子，只有继承链 (`extends`) 或接口实现链 (`implements`) 中有该超类的类才能使用该 mixin 。<br>
```dart
class Musician {}
mixin MusicalPerformer on Musician {}
class SingerDancer extends Musician with MusicalPerformer {}
```
#### mixin class
`mixin` 声明定义类 mixin ，`class` 声明定义类一个类。`mixin class` 声明定义一个类，这个类既可用作常规类，也可用作 mixin 。它需要同时遵守 mixin 和 class 的约束。<br>

- mixin 声明不能有 extends 或 with 字句
- class 声明不能有 on 字句

#### abstract mixin class
如果你想在 mixin class 上实现类似 on 字句的效果，可以使用 `abstract` 关键字，并在其中定义所依赖的抽象方法。<br>
```dart
abstract mixin class Musician {
    void playInstrument(String instrumentName);

    void playPiano() {
        playInstrument('Piano');
    }

    void playFlute() {
        playInstrument('Flute');
    }
}

class Virtuoso with Musician {
    void playInstrument(String instrumentName) {
        print('Plays the $instrumentName beautifully');
    }
}

class Novice extends Musician {
    void playInstrument(String instrumentName) {
        print('Plays the $instrumentName poorly');
    }
}
```
## 枚举
```dart
enum Color {red, green, blue}
```
#### 增强枚举
枚举允许在其中声明变量、方法和 const 构造函数，使其获得与类相似的行为。但需遵循下述规则：<br>

- 成员变量必须是 final ，包括 mixin 添加的变量。
- 只允许 const 生成构造函数
- 工厂构造函数只允许返回一个字段，这个字段是已知的枚举实例 ( enum instance) 。
- 没有其他类可以 extends Enum 。
- 不能重写 index 、hashCode 、== 运算符。
- 不能在枚举中声明与成员变量同名的枚举项。
- 枚举项是声明必须在整个枚举声明的开头，并且至少声明一个枚举项。

```dart
enum Vehicle implements Comparable<Vehicle> {
    car(tires: 4, passengers: 5, carbonPerKilometer: 400),
    bus(tires: 6, passengers: 50, carbonPerKilometer: 800),
    bicycle(tires: 2, passengers: 1, carbonPerKilometer: 0);

    const Vehicle({required this.tires, required this.passemgers, required this.carbonPerKilometer});

    final int tires;
    final int passengers;
    final int carbonPerKilometer;

    int get carbonFootprint => (carbonPerKilometer / passengers).round();

    bool get isTwoWheeled => this == Vehicle.bicycle;

    @override
    int compareTo(Vehicle other) => carbonFootprint- other.carbonFootprint;
}
```
#### 使用枚举
像访问类的静态变量一样访问枚举值。<br>
枚举中的每个值都有一个 index 的 getter ，它返回枚举值声明的顺序，从 0 开始。<br>
使用 values 常量可以获取该枚举的值列表。<br>
使用 name 属性获取枚举值名称。<br>
```dart
final favoriteColor = Color.blue;
assert(Color.red.index == 0);
List<Color> colors = Color.values;
print(Color.blue.name);//'blue'
```
## 扩展方法
我们可以使用 `extension on` 关键字扩展类的方法、getter 、setter 或运算符。也可以扩展静态字段和 static helper methods 。<br>
在 extension 和 on 中间我们可以定义扩展的名称。如果扩展名没有显式定义或者扩展名是以 `_` 开头的，那该扩展仅在声明它们的库中可见。<br>
```dart
extension NumberParsing on String {
    int parseInt() {
        return int.parse(this);
    }

    double parseDouble() {
        return double.parse(this);
    }
}
print('42'.parseInt());
print(NumberParsing('42').parseInt());
```
如果变量类型声明为 dynamic ，则不能在该变量上调用扩展方法。<br>
## 扩展类型
事实上，扩展类型是不安全的抽象，尽管它能轻松修改现有类型的接口而不产生实际包装器的成本。<br>

<h1 id="8">类修饰符</h1>

类修饰符关键字位于类或 mixin 声明之前，包括：<br>

- abstract
- base
- final
- interface
- sealed
- mixin

只有 base 修饰符可以出现在 mixin 声明之前。<br>
#### abstract
声明一个抽象类，抽象类不能直接构造实例。<br>
#### base
强制lei或 mixin 实现。同时基类不允许在其自己的库之外进行被 implements 。<br>
必须将实现或继承基类的任何类标记为 base 、final 或 sealed 。这样可以防止外部库破坏基类保证。<br>
#### interface
interface 定义的接口不能在外部库中被继承。
#### final
禁止当前类被外部库继承或 implements 。不过可以在同一库中被继承或 implements 。因此其任何子类也必须标记为 base 、final 或 sealed。<br>
#### sealed
描述该类的子类是已知的、可枚举的。<br>
## 组合类修饰符
类声明可以安装顺序排列：<br>

- （可选）abstract
- （可选）base 、interface 、 final 或 sealed
- （可选）mixin
- class 关键字本身

一些类修饰符是不能组合使用的：<br>

1. abstract 和 sealed 。因为 sealed 已经是隐式 abstract 。
2. interface 、final 或 sealed ，它们与 mixin 不能混用，因为它们会阻止混入。


<h1 id="9">并发</h1>

此章节由于对于 Dart 的并发编程没有什么理解，无法进行较好的整理。故延后。
[Dart 并发](https://dart.cn/language/concurrency)

<h1 id="10">空安全</h1>

在没有空安全之前，空类型可以看作是所有类型的子类。<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405071924553.png)
在空安全之后，空类型是一个单独的类型。<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405071925831.png)
可空类型是该类型与空类型的超类。<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405071925353.png)
在空安全之后，函数参数的隐式类型转换被移除。<br>
在没有空安全之前，Dart 的类型系统中，Object 是顶层类型，Null 是底层类型。在空安全之后，Object? 是顶层类型，Never 是底层类型。<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405071927113.png)
throw 表达式的静态类型就是 Never 。<br>
```dart
    Never wrongType(String type, Object value) {
        throw ArgumentError('Expected $type, but was ${value.runtimeType}.');
    }
```
## late 关键字
`late` 关键字有多种语义。<br>
late 修饰符是“在运行时而非编译时对变量进行约束”。因此 late 修饰的变量允许延迟初始化。而加上了 late 修饰意味着“每次运行都要检查是否已经赋值”。<br>
