学习笔记

### Grammar
#### Tree vs Priority
- +-
- */
- ()

#### Expressions
- member
    - a.b
    - a[b]
    - foo`string`
    - super.b
    - super['b']
    - new.target
    - new Foo()
- New
    - new Foo
- Call
    - foo()
    - super()
    - foo()['b']
    - foo().b
    - foo()`abc`
- update
    - a++
    - a--
    - --a
    - ++a
- Unary
    - delete a.b
    - void foo()
    - typeof a
    - +a
    - -a
    - ~a
    - !a
    - await a
- Exponential
    - ** (右结合)
- multiplicative
    - * / %
- Additive
    - + -
- Shift
    - << >> >>>
- Relationship
    - < > <= >= instanceof in
- Equality
    - ==
    - !=
    - ===
    - !==
- Bitwise
    - &^|
- Logical
    - &&
    - ||
- Conditional
    - ?:

Reference
非7种标准类型中的类型

两个部分
- Object
- Key

基本方法
- delete
- assign

Left handside & right handside
- a.b = c
- a+b = c
left handside 只能放到等号左边

#### Type convertion
- a + b
- 'false' == false
- a[o] = 1

Unboxing
- ToPremitive
- toString vs valueOf
- Symbol.toPrimitive

Boxing
- Number: new Nnumber(1)
- String: new String('a')
- Boolean: new Boolean(true)
- Symbol: new Object(Symbol('a'))

### 运行时的概念
Grammar
- 简单语句
- 组合语句
- 声明

Runtime
- Completion record
- Lexical environment

Completion record
```javascript
if (x==1) 
    return 10;  // 有可能 return，也有可能不return
```

需要有一个数据结构来描述语句的执行结果，是否返回了，返回值是什么

组成
- [[type]]: normal, break, continue, return, or throw
- [[value]]: 基本类型
- [[target]]: label

简单语句和复合语句

简单语句就是语句里面不会再容纳其他语句的语句

- ExpressionStatement
- EmptyStatement
- DebuggerStatement
- ThrowStatement
- ContinueStatement
- BreakStatement
- ReturnStatement

复合语句
- BlockStatement
- IfStatement
- SwitchStatement
- IterationStatement
- WithStatement
- LabelledStatement
- TryStatement

BlockStatement
- type: normal
- value: --
- target: --

BreakStatement | ContinueStatement
- type: break / continue
- value: --
- target: label

TryStatement
- type: return
- value: --
- target: label

声明
- FunctionDeclaration
- GeneratorDeclaration
- AsyncFunctionDeclaration
- AsyncGeneratorDeclaration
- VariableStatement
- ClassDeclaration
- LexicalDeclaration

作用范围只认 function body
- function
- frunction *
- async function
- async function *
- var

新特性
在声明之前使用就会报错
鼓励使用
- class
- const
- let

预处理 pre-processing
执行之前会对代码本身做一次预先处理
预处理会提前找到 var 声明的变量，并让它生效在函数级别，即使在 return 之后

所有的声明都是有预处理的，会把声明的变量变成局部变量，区别在于 const 在声明之前使用会报错，而且可以被 try catch

作用域
作用域链是一个早期的概念
var + function 的体系下声明的作用范围都是函数体，不管写在哪，套在什么东西里面。
const/let 作用域就在花括号，block 语句，但是每个循环不产生新的

#### 结构化
宏任务和微任务

执行粒度（运行时）
- 宏任务 // 传给 js 引擎的任务
- 微任务(Promise) // js 引擎内的任务
- 函数调用(Execution Context) // 
- 语句/声明(Completion Record)
- 表达式 (Reference)
- 直接量/变量/this ...

一段代码给 js 引擎执行是一个宏任务，其中可能包含多个因为 Promise 产生的微任务

事件循环 (独立线程)
- get code
- execute
- wait

#### 函数调用
- Execution context stack
    - ... Execution context
    - Running execution context

Execution context
- code evaluation state
- Function
- Script Module
- Genrator
- Realm
- LexicalEnvironment
- VariableEnvironment

LexicalEnvironment
- this
- new.target
- super
- 变量

VariableEnvironment
- VariableEnvironment 是个历史遗留的包袱，仅仅用于处理 var 声明

Environment record
- Declarative environment records
- Global environment records
- Object environment records
- Function environment records
- module environment records

Function - closure
每个函数都会生成一个闭包
- 代码部分
- 环境部分 (Environment record)

2018 以后 scope chain 的说法也没有了
箭头函数会导致 this 也被保存下来

Realm
在 js 中，函数表达式和对象直接量均会创建对象
使用.做隐式转换也会创建对象
这些对象是有原型的，如果我们没有 Realm，就不知道他们的原型是什么

蚂蚁前端的 G6 可以做对象可视化