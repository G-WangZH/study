@[TOC]( )
#  前期背景
## TS是什么？
TS是JS的类型的超集，支持ES6语法，支持面向对象编程的概念，如类、接口、继承、泛型等。
## js和java区别
- JS：`解释型，弱类型，动态语言`
- Java：编译型，强类型，静态语言

解释：
- 解释型：`我们写的代码，无需进行编译，直接运行`，也需要一个翻译器，一遍翻译一边执行
- 编译型：人类写的是英语，机器不认识，需要一个编译器，将人们写的语言转换成机器识别的语言，这个过程叫编译
- 弱类型：`声明变量时无需指定类型`
- 强类型：声明变量时必须指定类型
- 动态语言：`在代码执行的过程中可以动态添加对象的属性`
- 静态语言：不允许在执行过程中随意添加属性

结论：js的特点是灵活、高效，很短的代码就能实现复杂功能，缺点`是没有代码提示，容易出错且编辑工具不会给任何提示`，而Java则相反。

`TS可以简单理解为将JavaScript转变为Java一类语言的过程`。

## 为什么学习TS
- 从编译语言的动静来区分，`TS属于静态类型的编程语言，JS属于动态类型的编程语言`。
	- 静态类型：`编译期做类型检查`
	- 动态类型：`执行期做类型检查`
- `对于JS来说，需要等到代码真正去执行的时候才能发现错误（晚）， 对于TS来说，在代码编译的时候（代码执行前）就可以发现错误（早）`；

并且，配合VSCode等开发工具，TS可以提前到在`编写代码的同时就发现代码中的错误`，减少找Bug，改Bug时间，另外还`支持代码提示`。

`Vue3的源码使用TS重写，Angular默认支持TS、React与TS完美配合`，TypeScript已经成为大中型前端项目的首选变成语言。
注意：`Vue2对TS的支持不好~`

## TypeSscript类型
类型注解总结：
- 将来不能将其他类型的值赋予这个变量
- 代码有提示，在变量后面` . `可以直接看到当前类型所支持的`所有属性和方法`

可以将`TS中的常用基础类型`细分为两类：
- `JS已有类型`
	- 原始类型，简单类型（number、string、boolean、null、undefined）
	- 复杂数据类型（数组、对象、函数等）
- `TS新增类型`
	- 联合类型
	- 自定义类型（类型别名）
	- 接口
	- 元组
	- 字面量类型
	- 枚举
	- void
	- any
	- never
	- 名字空间

&nbsp;
***
&nbsp;
# 1、基础类型
```typescript
// 字符串类型
let a:string = '123';

// 数字类型
let num: number = 123;

// 与 void 的区别是，undefined 和 null 是所有类型的子类型。
// 也就是说 undefined 类型的变量，可以赋值给 string 类型的变量：
//这样是没问题的
let test: null = null
let num2: string = "1"
num2 = test
 

// 在 TypeScript 中，任何类型都可以被归为 any 类型。
// 这让 any 类型成为了类型系统的顶级类型（也被称作全局超级类型）。
// 没有强制限定哪种类型，随时切换类型都可以 我们可以对 any 进行任何操作，不需要检查类型
let anys:any = 123
anys = '123'
anys = true

// unknown 类型只能被赋值给 any 类型和 unknown 类型本身。
let value: unknown;

let value1: unknown = value; // OK
let value2: any = value; // OK
let value3: boolean = value; // Error

```
# 2、数组类型
```typescript
let arr: number[] = [123]

//这样会报错定义了数字类型出现字符串是不允许的
let arr: number[] = [1,2,3,'1']

//数字类型的数组
var arr: number[] = [1, 2, 3];
//字符串类型的数组
var arr2: string[] = ["1", "2"];


// 数组泛型，规则 Array<类型>
let arr:Array<number> = [1, 2, 3, 4, 5];

// 多维数组
let data:number[][] = [[1,2], [3,4]];

//任意类型的数组
var arr3: any[] = [1, "2", true];

// 希望数组里面可以存数字或字符串
// 联合类型 |
let arr4:(number | string)[] = [1, 2, 4, '2323'];
// 注意：| 的优先级较低，需要用（）包裹提升优先级
// 一旦使用联合类型，说明 arr 中存储的既可能是number 也可能是 string，所以会丢失一部分提示信息

// 定时器？
```

# 3、类型别名
目标：`能够使用类型别名给类型起别名`。
使用场景：当同一类型（复杂）被多次使用时，可以通过` type `定义类型别名（推荐使用大写字母开头），简化该类型的使用。

```typescript
// 希望 N 个数组里面可以存数字或字符串
// 方法1：里面上面的联合类型
// 方法2：类型别名
type ArrType = (number | string)[]

let arr1: ArrType = ['1', 2, 4, '2323'];
let arr2: ArrType = [1, '2', 4, '2323'];
let arr3: ArrType = [1, 2, '4', '2323'];


// 灵活度很高，可以随意搭配组合使用
type ItemType = number | string
let arr4: ItemType[] = [1, 2, '3', '123'];
let arr5: ItemType = '23'

// 总结：将一组类型存储到【变量】里，用 type 来声明这个特殊的【变量】

// 玩花活
type s = string
type n = number

let str2: s = '123'
let num2: n = 123
```
# 4、函数类型
函数的类型实际上指的是：`函数参数`和`返回值`的类型
为函数指定类型的两种方式：
- 单独指定参数、返回值的类型
- 同时指定参数、返回值的类型

```typescript
// ts 要求我们必须给参数定义类型，而返回值它会自动推断
// 函数声明
function add(a: number, b: number) {
	return a + b;
}

// 箭头函数，ts中的箭头函数必须是小括号
const sub = (a: number, b: number): number => {
	return a + b;
}

//通过?表示该参数为可选参数
const fn = (name: string, age?:number): string => {
    return name + age
}
fn('张三')

// 函数参数的默认值
const fn = (name: string = "我是默认值"): string => {
    return name
}
fn()

// 函数的类型别名
type FnType = (a: number, b: number) => number

// 函数的类型别名通常是给箭头函数/函数表达式使用，不会给函数声明使用

const fn: FnType = function(a, b) {
  return a + b
}


函数重载

// 重载是方法名字相同，而参数不同，返回类型可以相同也可以不同。
//如果参数类型不同，则参数类型应设置为 any。
// 参数数量不同你可以将不同的参数设置为可选。

function fn(params: number): void
 
function fn(params: string, params2: number): void
 
function fn(params: any, params2?: any): void {
    console.log(params)
    console.log(params2)
}
 
fn(123)
fn('123',456)
```

# 5、void类型 && never类型
### void
`如果函数没有返回值，那么，在TS的类型中，函数返回值类型为void`
```typescript
const sayHello = (content: string): void => {
  console.log(content)
}

// 但，如果指定返回值类型为 undefined，
// 此时，函数体中必须显示的 return undefined 才可以
const addN = (): undefined => {
  return undefined
}
```
### never
never类型代表从不会出现的值，注意：never的变量只能被never类型所赋值。
never类型一般用来指定那些总是抛出异常、无线循环
```javascript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
	throw new Error(message);
}
```
# 6、可选参数
- 使用函数实现某个功能时，参数可以传也可以不传。这种情况下，在给函数参数指定类型时，就用到可选参数。在可选可不传的参数名称后面添加` ? `
- 注意：可选参数只能出现在参数列表的最后，也就是说可选参数后面不能再出现必选参数。

```typescript
const printP = (name: string, age?: number): void => {
  console.log(name, age)
}

printP('哆啦A梦', 18)
printP('波多野结衣')
```

# 7、对象

JS中的对象是由属性和方法构成的，而`TS对象的类型就是在描述对象的结构`（有什么类型的属性和方法）。
调用对象时可以直接提示有哪些属性和方法

```typescript
// ts 就像咋写注释，以前写的注释是给程序员看的，ts 写的类型是给编辑器看的，程序员也可以看
let obj: {
  name: string,
  age: number,
  sayHi: (content: string) => void
} = {
  name: '哆啦A梦',
  age: 18,
  sayHi(content) {
    console.log(content)
  }
}

// 类型别名用法

// 对象类型
type Person = {
  name: string,
  age: number,
  sayHi: (content: string) => void 
}

let obj1: Person = {
  name: '哆啦A梦',
  age: 18,
  sayHi(content) {
    console.log(content)
  }
}

```

方法类型的两种定义形式
```typescript
// 方法的类型也可以使用箭头函数形式
type Person = {
  greet: (name: string) => void
  // greet(name: string) => void
}
let person: Person = {
  greet(name) {
    console.log(name)
  }
}
```
# 8、接口interface
当一个对象类型多次使用时，一般会使用`接口（interface）`来描述对象的类型，`达到复用的目的`。作用：给对象约束属性和方法

在`typescript`中，我们`定义对象`的方式要用关键字`interface（接口）`，我的理解是使用`interface`来定义一种约束，让数据的结构满足约束的格式。定义方式如下：

```typescript
// 基础用法：
interface IPerson {
  name: string
  age: number
  sayHi: () => void
}

const p1: IPerson = {
  name: '苍老师',
  age: 18,
  sayHi() {
    console.log('i like')
  }
}
```
- 使用`interface`关键字声明接口
- 接口名称可以是任何合法的变量名称，推荐以` I `开头
- 声明接口后，直接使用接口名称作为变量的类型
- 因为每一行只有一个属性类型，因此，`属性类型后没有；（分号）`


```javascript
//这样写是会报错的 因为我们在person定义了a，b但是对象里面缺少b属性
//使用接口约束的时候不能多一个属性也不能少一个属性
//必须与接口保持一致
interface Person {
	b: string,
	a: string
}

const person:Person = {
	a: '213'
}


//重名interface  可以合并
interface A{name:string}
interface A{age:number}
var x:A={name:'xx', age:20}

//继承
interface A{
    name:string
}
 
interface B extends A{
    age:number
}
 
let obj:B = {
    age:18,
    name:"string"
}
```

### interface VS type
总结：
interface 和 type 的区别，`接口interface` 只能约束对象，而`类型别名 type` 不仅可以为对象指定类型，实际上可以为任何类型指定别名。
推荐：`能使用type就使用type`，更灵活，更简单

### 接口继承
如果两个接口之间有相同的属性和方法，可以将`公共的属性和方法抽离出来，通过继承来实现复用`。

```typescript
// 比如，这两个接口都有x、y两个属性，重复写两次，可以，但是很繁琐
interface Point2D {
  x: number
  y: number
}

interface Point3D {
  x: number
  y: number
  z: number
}

// 更好的方式
interface Point2D {
  x: number
  y: number
}

// 继承Point2D
interface Point3D extends Point2D {
  z: number
}
```
### type 如何和 interface 一样实现继承的效果？
```typescript
type Person = {
  usename: string
  age: number
  sayHi: () => void
}

//  & 与连接符：既要满足前面的也要满足后面的
//  | 或连接符：满足其中一个即可
type Student = {
  score: number
  sleep: () => void
} & Person

```
接口继承：可以实现让一个接口使用另一个接口的类型约束，实现接口的复用。

# 9、元组Tuple
限定`数组的固定类型和数据长度`，元组类型是另一种类型的数组，他确切地`知道包含多少个元素，以及特定索引对应的类型`。
```typescript
// 地图经纬度
let arr6: [number, number] = [13.334, 36.2323]

// 或者：
let arr6: [number, number];
arr6 = [13.334, 36.2323]
```
# 10、字面量
字面量类型就是把字面量当做类型来用。eg：
```typescript
let s1 = 'hello TS';
const s2 = 'hello TS';
```
通过TS类型推论机制，可以得到：s1的类型时string，s2的类型为 'hello TS'；
解释：s2是一个常量，它的值不能变化只能是 'hello TS'，所以，它的类型为 'hello TS'。此次 'hello TS'，就是一个`字面量类型`，也就是说`某个特定的字符串也可以作为TS中的类型`。

- 使用模式：字面量类型配合联合类型一起使用
- 使用场景：用来表示一组明确的可选值列表

比如：在贪吃蛇游戏中，游戏的方向的可选值只能是上、下、左、右中的任意一个

# 11、枚举类型enum
枚举的功能类似于` 字面量类型+联合类型组合 `的功能，也可以表示一组明确的可选值；
枚举：定义一组命名常量。它描述一个值，该值可以是这些命名常量中的一个
如果不设置值，默认从 0 开始

```typescript
// 创建枚举
enum Direction {
  Up,
  Dowm,
  Left,
  Right
}

// 使用枚举类型
function changeDirection(direction: Direction) {
  console.log(direction)
}
```
- 使用` enum `关键字定义枚举
- 约定枚举名称以`大写字母`开头
- 枚举中多个值之间通过` , `(逗号)分割
- 定义好枚举后，直接使用枚举名称作为类型注释

### 数字枚举
我们把枚举成员的值为数字的枚举，称为` 数字枚举 `。注意：`枚举成员是有值的，默认为：从0开始自增的数值`
```typescript
// 给枚举中的成员初始化值
enum Direction {
  Up = 10,
  Dowm,
  Left,
  Right
}
//  Dowm => 11, Left => 12, Right => 13
```
### 字符串枚举
枚举成员的值为字符串。字符串枚举没有自增长的行为，因此，字符串枚举的每个成员必须有初始值。

```typescript
enum Direction {
  Up = 'Up',
  Dowm = 'Dowm',
  Left = 'Left',
  Right = 'Right'
}
```
### 枚举实现原理
枚举是TS为数不多的非JavaScript类型级扩展的特性之一
其他类型仅仅被当做类型，而`枚举不仅用作类型，还提供值`（枚举成员都是有值的）
也就是说，其他的类型会在编译为JavaScript代码时自动移除，但是 枚举类型会被编译为JS 代码


```typescript
// 会被编译为以下代码：
(function (Direction) {
    Direction[Direction["Up"] = 0] = "Up";
    Direction[Direction["Dowm"] = 1] = "Dowm";
    Direction[Direction["Left"] = 2] = "Left";
    Direction[Direction["Right"] = 3] = "Right";
})(Direction || (Direction = {}));
```
说明：枚举与前面安静到的`字面量类型+联合类型`组合的功能类似，都用来表示一组明确的可选值列表。
一般情况下，推荐`字面量类型 + 联合类型`组合的方式，因为相比枚举，这种方式更加直观、简洁、高效。
# 12、any类型
any类型：`原则上不推荐使用`，只有在迫不得已的时候才可以用一下，否则会变成 AnyScript，失去 TS 类型保护机制

```typescript
let obj2: any = {x: 0}

const n: number = obj2;

```
尽可能的避免使用any类型，除非临时使用any来避免书写很长、很复杂的类型，其他隐式具有any类型的情况
- 声明变量不提供类型也不提供默认值
- 函数参数不加类型

注意：因为不推荐使用any，所以，这两种情况下都应该提供类型

# 13、类型断言 | 交叉类型
类型断言：`强行指定获取到的结果类型`
有时候你会比TS更加明确一个值的类型，此时，可以使用类型断言来指定更具体的类型，eg：
`const aLink = document.getElementById('link')`
注意：该方法返回值的类型是`HTMLElement`，该类型只包含所有标签公共的属性和方法，不包含a标签特有的href等属性，因此，这个`类型太宽泛`，无法操作href等a标签特有的属性和方法
解决办法：`这种情况下就需要使用类型断言指定更加具体的类型。`
```typescript
const aLink = document.getElementById('link') as HTMLAnchorElement
// aLink.href, 敲代码段的时候直接有提示
```
- 语法：使用` as `关键字实现类型断言
- 关键字` as `后面的类型时一个更加具体的类型 （HTMLAnchorElement）

另一种写法，使用<> 语法，不常用，知道即可
`const alink2 = <HTMLAnchorElement>document.getElementById('link')`

总结：当函数获取到的结果类型较为宽泛时，我们有知道具体类型，就可以使用断言强行指定类型。

### 交叉类型
```javascript
// 交叉类型: 多种类型的集合，联合对象将具有所联合类型的所有成员
interface People {
  age: number,
  height： number
}
interface Man{
  sex: string
}
const xiaoman = (man: People & Man) => {
  console.log(man.age)
  console.log(man.height)
  console.log(man.sex)
}
xiaoman({age: 18,height: 180,sex: 'male'});
```

# 14、泛型
`泛型是可以保证类型安全前提下，让函数等与多种类型一起工作，从而实现复用`，常用于：函数、接口、class中。
`泛型在保证类型安全（不丢失类型信息的同时，可以让函数等与多种不同的类型一起工作，灵活可复用。）`

```javascript
// 定义一个getId，传入一个值，返回这个值，可以传入任意类型
// any 可以解决问题，但也会带来问题：返回值也是any没有提示了
function getId(val: number | string | boolean) {
	return val
}
```
解决方案：
```typescript
// 期望：调用getId函数时来指定传入参数的类型
// <T> 在声明泛型
// (val: T) 使用泛型
// 调用函数时传入泛型指定的具体类型
function getId<T>(val: T) {
  return val
}

console.log(getId<number>(123))
console.log(getId<string>('123'))

// 简化泛型函数调用,  也可以使用，调用时不用加<>
console.log(getId(123))
```
上面的T和下面的Type是一样的，知识变量名称不同罢了

### 泛型约束
```typescript
// 如下代码：当传输string，正确，当输入number时，报错，所以需要对val进行约束，即：泛型约束
function getId<T>(val: T) {
  console.log(val.length)
  return val
}
```

添加约束（常见方法）
```javascript
// 定义接口
interface ILength {
  length: number
}

// 使用，添加约束，给泛型找爸爸
function getId<T extends ILength>(val: T) {
  console.log(val.length) // 输入val代码提示length属性
  return val
}
```
- 通过` extends `关键字使用该接口，为泛型添加约束
- 该约束表示：传入的类型必须具有length属性


### 多个类型变量
泛型的类型变量可以有多个，并且类型变量之间还可以约束（比如，第二个类型变量受第一个类型变量约束）
```typescript
// 需求，定义一个函数，传入一个对象，在传入一个字符串属性名，返回属性值
function getProp(obj: object, key: string) {
	return obj[key] // 这种方式会报错，不知道是否这个key
}
```

```typescript
function getProp<O, K extends keyof O>(obj: O, key: K) {
	return obj[key]
}

let person = {name: 'jack', age: 18}
getProp(person, 'name')
```
- 添加第二个类型变量key，两个类型变量之间使用 ，逗号分隔
- keyof 关键字接收一个对象类型，生成其键名称（可能是字符串或数字）的联合类型
- 本示例中 keyof O 实际上获取的是person对象所有键的联合类型，也就是：'name' | 'age'
- 类型变量key受O约束，可以理解为：key只能是O所有键中的任意一个，或者说只能访问对象中存在的属性


### 泛型接口

```typescript
interface Student {
	id: number
	name: string
	hobby: string[]
}
let s1: Student = {
	id: 123,
	name: '波多小姐',
	hobby:['瑜伽', '运动', '歌唱']
}

// 转化
interface Student<T> {
	id: number
	name: T
	hobby: T[]
}

let s1: Student<string> = {
	id: 123,
	name: '波多小姐',
	hobby:['瑜伽', '运动', '歌唱']
}
```


参考：
[视频链接](https://www.bilibili.com/video/BV1vX4y1s776?p=31&spm_id_from=pageDriver&vd_source=6223af80f0d3af90c7943b1db211f730)
[小满视频讲解](https://www.bilibili.com/video/BV1wR4y1377K?p=17&vd_source=6223af80f0d3af90c7943b1db211f730)
[type和interface的区别](https://github.com/SunshowerC/blog/issues/7)

# 知识总结参考
[TypeScript汇总](https://ts.xcatliu.com/introduction/what-is-typescript.html)

# 相关的面试题
### interface和type的区别
两者最大的区别在于，`interface只能用于定义对象类型，而 type 的声明方式除了对象之外还可以定义交叉、联合、原始类型等`，类型声明的方式适用范围显然更加广泛。


