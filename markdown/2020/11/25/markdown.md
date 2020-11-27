# vue3 操作指南



### vue3中值得大家注意的点：

- 数据拦截机制的变更 <!-- .element: class="fragment" data-fragment-index="1" -->
- 组合API的引入 <!-- .element: class="fragment" data-fragment-index="2" -->
- setup函数带来的一系列惊喜 <!-- .element: class="fragment" data-fragment-index="3" -->
- 响应式API的应用 <!-- .element: class="fragment" data-fragment-index="4" -->



###  一、数据拦截机制的变更


#### 概述
<p class="fragment" data-fragment-index="1" style="font-size: 16px;">
vue2 通过 Object.defineProperty() 方式来截持数据的变更。</p>

<p class="fragment" data-fragment-index="2" style="font-size: 16px;">
而在升级为 vue3 之后，作者不再借助 Object.defineProperty 来劫持数据<br>
而是通过 es6 的新特性 proxy 来劫持数据。</p>


<p class="fragment" data-fragment-index="1" style="font-size: 16px;">
因为在 vue2 中，响应式数据需要通过 data() 初始化预先定义好</p>
<p class="fragment" data-fragment-index="2" style="font-size: 16px;">
但是在 vue3 之后，data 中的数据不再进行响应式追踪
<br>而是将它转化为 proxy 代理再进行追踪更新。</p>

![code](https://beehash.github.io/shared/images/10.54.38.png)<!-- .element: class="fragment" data-fragment-index="3"-->
![view](https://beehash.github.io/shared/images/10.28.23.png)<!-- .element: class="fragment" data-fragment-index="3"-->


注意： <!-- .element: class="fragment" class="hljs-emphasis" style="font-size: 20px;"-->
<p class="fragment" data-fragment-index="1" style="font-size: 16px;">
在vue3中，它将任何在data()函数中定义的数据，会使用带有getter和setter的存取器程序遍历所有的property，将其转化成proxy代理。</p>



###  二、组合API的引入


###### vue3中最大的改变是compositionApi的引入，这是vue3中的最核心的部分。


#### 包括以下几方面：

<p class="fragment" data-fragment-index="1" style="font-size: 16px;">
a、setup函数<br>
    下一节有详细介绍，这里就不赘述了。</p>

<p class="fragment" data-fragment-index="2" style="font-size: 16px;">
b、生命周期钩子<br>
    vue3抛出了全局的API，通过引入 [onX] 的形式<br>
    使setup函数中支持钩子函数的调用。</p>

<p class="fragment" data-fragment-index="3" style="font-size: 16px;">
c、provide/Inject</p>

<p class="fragment" data-fragment-index="4" style="font-size: 16px;">
d、getCurrentInstance</p>


#### provide/Inject
<p style="font-size: 16px;">vue3 中也支持在 setup 函数中使用 provide/Inject，
  只要在对应的位置引入即可：
</p>

```
	import { provide } from 'vue';
    setup(){
    	provide(name, 'hello!');
    }
   
    import {inject} from 'vue';
    setup(){
    	inject('name', [default-value]: string);
    }
```


#### getCurrentInstance
<p style="font-size: 16px;">支持在setup函数中访问自己的实例</p>
	
    import { getCurrentInstance } from 'vue';
	setup() {
    	const internalInstance = getCurrentInstance();
    }



### 三、setup函数带来的一系列惊喜


<p class="fragment" data-fragment-index="1" style="font-size: 16px;">
在vue3中，setup函数的引入<br>
这种函数式的开发方式，很好地将代码整合到一起。</p>

<p class="fragment" data-fragment-index="2" style="font-size: 16px;">
在vue2中，data数据需要在使用的时候调用this访问<br>
现在vue3中使用reactive，ref来设置响应式数据</p>


```
// before: 
data() {
    return {
        name: 'aaa',
    };
}

// after: 
const name = ref('aaa');  
就像简单的定义变量一样，使用ref就可以为变量初始化
```


<p class="fragment" style="font-size: 16px;">
setup函数取代了methods的函数定义方式<br><br>
可以选择性地对外暴露我们定义的函数<br><br>
而写在methods的函数，都统统绑定到了this上<br><br>
<span class="hljs-emphasis" style="font-size: 14px;">this得有多累啊 (> 3 >)</span><br><br>
很好地保护了我们自己定义的私有变量</p>


```
methods: {
    function unkouwnFunc() {
        console.log(3333);
    }
},
```
```
setup () {
    function unkownFunc() {
        console.log(3333);
    }

    return {
        // return之后，该函数绑定到了组件实例上，可以在模版中直接使用
        unkownFunc, 
    };
}
```


<p style="font-size: 16px;">关于setup函数中使用emit事件</p>

```
import { SetupContext } from 'vue';
  	
emits: ['eventName'],
setup(props, ctx: SetupContext){
    ctx.emit('event-name', value);
}
    
```


<p style="font-size: 16px;">另外作者为了支持setup函数，向外抛出了全局的生命周期钩子，
只在setup函数中使用有效。</p>


```
import { 
  onBeforeMount,
  onMounted,
  onBeforeUpdate,
  onUpdated,
  onBeforeUnmount,
  onUnmounted 
} from 'vue';

setup() {
    onBeforeMount() {
        your code
    }
    onMounted () {
        your code
    }
    onBeforeUpdate() {
        your code
    }
    onUpdated() {
        your code
    }
    onBeforeUnmount() { 
        your code
    }
    onUnmounted() {
        your code
    }
}
```



### 四、响应式API的应用


##### Ref Vs Reactive
  <p class="fragment" data-fragment-index="1" style="font-size: 16px;">ref 是一种基本数据类型的创建方式，返回的是一个 RefImpl 对象。value 指向其绑定的值。<br>如果用它来创建对像的话，会将它的  value 返回一个 reactive 对象。</p>
  <p class="fragment" data-fragment-index="2" style="font-size: 16px;">reactive 对象的返回的是一个 proxy 代理<br>他在创建的时候，就会一一遍历每一个属性，并将它创建为 es6 代理方式。</p>
  


  <p style="font-size: 16px;">这里我们做一个示例证明一下</p>

  ![code](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c150342a35d1414a9b86f02aa6425385~tplv-k3u1fbpfcp-watermark.image)


  ![view](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b7da85d66e0c4d2e8ec2b21058828745~tplv-k3u1fbpfcp-watermark.image)
  <p style="font-size: 16px;">从打印的数据我们可以看出<br>ref数据创建的对象类型的.value跟reactive的数据结构是一致的。<br>也就是说，在ref创建对象类型的数据时，对自动转化为reactive数据。</p>


Ref 和 reactive API带来的便利
<p class="fragment" data-fragment-index="1" style="font-size: 16px;">
  vue3中的 ref 和 reactive 非常非常的好用，为什么这么说？
<p class="fragment" data-fragment-index="2" style="font-size: 16px;">
  因为vue在这里直接给出了他的全局API。
</p>
<p class="fragment" data-fragment-index="3" style="font-size: 16px;">
  不管在任何场景下，我们都可以直接引入，并创建这个响应式的数据。通过导入，导出，实现代码的 <span class="hljs-emphasis">集中化管理</span> 。
</p>
<p class="fragment" data-fragment-index="4" style="font-size: 16px;">
  所谓 <span class="hljs-emphasis">集中化管理</span> 在这里指的是代码的运用灵活度提升。</p>
<p class="fragment" data-fragment-index="4" style="font-size: 16px;">
  vue2中，使用store来集中管理状态，而现在，我们可以直接真正抛弃store了。因为vue3的响应式API 给我们带来了非常nice的体验。</p>


```
>> state.js 
  store = {
    state: reactive({
      count: 0,
    })
  };

  export default store;
```
```
import store from 'state.js';
setup() {
  // set count
  store.count = 2;
  // get count
}
```


关于computed和watch在setup函数中的使用

```
import { computed, watch } from 'vue';

const aaa = computed(() => {
  return 333;
})
```


```
// watch 实现了$watch全部的功能
// 单个数据源侦听
watch((bbb, (current, prev)) => {
  Your Code
});

// 多个数据源的侦听
watch([aaa, bbb], ([current, prev], [current2, prev2]) => {
  Your Code
})
```
```
import {watchEffect} from 'vue';

// watchEffect 只追踪依赖项的变更，在变化时，会执行回调函数
const aaa = ref(0);
watchEffect(() => {
  console.log(aaa.value);
});
```


## watch VS watchEffect

  <p class="fragment" data-fragment-index="1" style="font-size: 16px;">
    1、惰性侦听
  </p>
  <p class="fragment" data-fragment-index="1" style="font-size: 16px;">
  watchEffect 只具备追踪变更的功能，并且在追踪依赖项的时候就会执行回调函数<br>
  watch 相比，会惰性侦听依赖项，在追踪依赖项的时候，不会执行回调函数。
  </p>

  <p class="fragment" data-fragment-index="2" style="font-size: 16px;">
    2、支持访问当前和之前的状态
  </p>
  <p class="fragment" data-fragment-index="3" style="font-size: 16px;">
    3、更具体地说明什么情况下应该触发对调函数
  </p>
  <p class="fragment" data-fragment-index="3" style="font-size: 16px;">
    应该是watch相对于watchEffect给出了更多的选项。<br>
      比如，获取之前和现在的状态，通过状态的变化，我们可以判段是否执行对应的回调。
  </p>



# END
# TK U