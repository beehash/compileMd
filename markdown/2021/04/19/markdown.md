### javascript 之探秘 This



javascript 中的 this 是我们常常碰到的，但是，你真的够了解它嘛？

或许你经常会碰到一些尴尬的场面，设计程序时，因为对this不够了解，在程序设计中丢失掉原本预想的对象；在程序运行中，随着代码量的增加，在运行程序时，this却指向另一个位置；另外，this 也常常会出现在面试题中。

让我们来揭开它的神秘面纱 👉



##### 人工GPS导航仪
- this的误解
- 改变this指向
- 绑定优先级
- 小例子



## 1、this的误解
- 误解1：指向自身
- 误解2：它的作用域



### 1.1 误解1: 指向自身
<p style="font-size: 16px;">
让我们来看下这道题，它真的指向自身么？<br><br>
</p>

![这张图片](https://beehash.github.io/shared/images/21.4.15.1.png)


<p style="font-size: 16px;">打印输出：3</p>
<p style="font-size: 16px;">
这里输出 3 出自 arguments.length<br><br>
你可以看见 this 并非指向自身了<br><br>
因为 arguments[0] 这个表达式已经静悄悄地改变了this的指向为 arguments对象了。 
</p>


### 1.2 误解2：它的作用域
<p style="font-size: 16px; text-align: left;">
this指向的是当前对象所在的作用域。 <br><br>
</p>
事实是没有返回4，也推翻了这个假说


$\color{#f89d5e}{Note：}$ `var length = 4；` 这里实际上使用了 默认绑定。将  this 指向 global 作用域对象。

什么是默认绑定？下文会解释到


#### 小结
 事实上，javascript 有两个作用域， 一个是`词法作用域`，对象在定义的时候的作用域。另一个是 `动态作用域`，所指的是对象在被使用时的作用域。
 
 javascript 虽然是编译型语言，但是和大多数编译型语言不一样，它的执行作用域是在运行时指定的，不同的运行环境，他的执行作用域是有可能改变的。
 
 因此，this 指向问题，不能看this 的使用位置，而应该看调用它的作用域对象是谁。




## 2、 改变 this 的指向
有多少种方式可以改变this的指向？
- 1、new 绑定
- 2、call、apply 绑定
- 3、bind 绑定
- 4、隐式绑定
- 5、默认绑定

### 2.1 new 绑定
new 实例化一个函数时，经历了哪些过程？
- 1、创建一个对象object
- 2、设置原型链
- 3、让构造函数中的this指向object，并执行构造函数的方法体
- 4、返回一个对象

### 2.2 call、apply
```
func.call(this, [arguments]);
func.apply(object, arguments);
```

call 、apply 方式除了用法上有一些区别外，效果都是为 func 绑定 this 执行环境，并执行函数。

属于硬绑定，即强制改变 this 的上下文环境

### 2.3 bind 绑定
```
var function aaa () {
    resturn this.count;
}
var obj  = {
    count: 3,
};
var p = aaa.bind(obj;

p(); // 输出 3
```
与call 、apply 略有不同的是，bind 只是改变函数的 this 指向，不会执行该函数。

### 2.4 隐式绑定
隐式绑定是我们常见的绑定方式。
```
function test () {
  console.log(this.a);
}
var obj = {
  a: 3,
  test,
};
obj.test(); // 输出 3
```

foo 函数中的 this 被隐式绑定到了 obj 上。实质上是运用了 apply 方式绑定到了 obj 上

### 2.5 默认绑定
```
var a = 6;
```
打印输出：
- this.a   // 输出 6
在这里默认绑定全局作用域的对象global

$\color{#f89d5e}{Note：}$ 默认绑定在 es5 严格模式下， 没有效果

## 3、绑定优先级
上述绑定改变this指向的方法也有优先级。

$\color{red}{ bind 、 apply 、 call}$  >  $\color{orange}{new}$ > $\color{green}{隐式绑定}$ > $\color{blue}{默认绑定}$


## 4、小例子
```
function Foo(){
 // funcI
 getName = function() {
     alert(1);
 };

 return this;
}

// funcII
Foo.getName = function() {
    alert(2);
};

// funcIII
Foo.prototype.getName = function(){
    alert(3);
};

// funcIV
var getName = function(){
    alert(4);
};

// funcV
function getName(){
    alert(5);
}
```

> 请写出以下输出结果：

$\color{#f89d5e}{提示：}$ 这道题中定义了多种 getName 方法，解题关键在于分析中每一道题中的 getName 代指的哪一个.

- 1. Foo.getName();
> `funcII`
- 2. getName();
> `funcIV`
- 3. Foo().getName();
> `funcI`
- 4. getName();
> `funcI`
- 5. new Foo.getName(); // 2
> Foo.getName  `(funcII)` ->  new -> ()
- 6. new Foo().getName(); // 3
> Foo() -> new -> getName  `(funcIII)` -> ()
- 7. new new Foo().getName(); // 3
> Foo() -> new -> getName  `(funcIII)`  ->  new -> ()

### In the End

本期分享到此结束， 想看更多内容，请访问我的博客主页 https://blog.zswindfly.top[此地址不存在]， 后期加上。 > _ *
