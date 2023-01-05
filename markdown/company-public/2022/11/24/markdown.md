## 智慧树前端插件开发分享



### 开发 & 发布 插件 8 件套

- 准备一个 git 仓库
- npm int 初始化项目，生成 package.json 文件
- 创建一个入口文件 index.js
- 创建一个存放插件内容的目录 src
- 安装依赖包
- 开发插件
- 登录 npm 私库， npm login
- npm publish 上传项目
-  <span class="hljs-emphasis">奶茶<span>



### 项目管理组织模式


<p class="fragment" data-fragment-index="1" style="text-align: center"> Multirepo</p>
<p class="fragment" data-fragment-index="2" style="text-align: center"> Monorepo</p>


<p style="width: 600px;font-size: 14px; margin: 0 auto;line-height: 28px; text-align: justify;line-height: 28px;">
目前项目管理比较火热的就是 Multirepo 和 Monorepo。它们都属于管理组织代码的方式。
因为技术的不断更新以及项目规模的不断扩大。对于项目地管理也越来越迫切。
Monorepo 随着这些变化逐渐出现在公众地视野，比较出名的是 Babel 使用 Monorepo，
随后，越来越多的项目引入 Monorepo，如：Vue，Element，antd 等。
这种比较新颖的项目管理方式激起了很大的火花。
</p>



###  Multirepo （poly-repo）


#### 介绍
<p class="fragment" data-fragment-index="1"  style="font-size: 14px; width: 600px; margin: 0 auto;text-align: justify;line-height: 28px;">
  一个应用可能是由多个仓库构成，如前后端项目可分别存放在不同的仓库中。前后端的项目也可能拆分出不同的模块，
  分别由独立的仓库进行管理。
  这种多个独立的仓库管理一个应用，或者多个应用，多个仓库管理的组织管理方式，叫做 Multirepo，也称为 poly-repo。<br>（多元项目管理）
</p>


#### 优点
<p class="fragment" data-fragment-index="1" style="text-align: center; font-size: 18px;"> 利于 git 版本控制</p>
<p class="fragment" data-fragment-index="2" style="text-align: center; font-size: 18px;"> 利于项目多元化</p>
<p class="fragment" data-fragment-index="3" style="text-align: center; font-size: 18px;">方便权限管理</p>


#### 缺点
<p class="fragment" data-fragment-index="1" style="text-align: center; font-size: 18px;"> 项目不够聚合，分散的代码增加了沟通管理成本 </p>
<p class="fragment" data-fragment-index="2" style="text-align: center; font-size: 18px;">分离的仓库间代码，不利于代码整合优化</p>



### Monorepo


#### 介绍
<p style="font-size: 14px; width: 600px; margin: 0 auto;text-align: justify;line-height: 28px;"  class="fragment" data-fragment-index="1">
多个项目使用一个仓库进行管理，这种组织管理模式是 Monorepo。
相较于Mutirepo，monorepo 则是 规避了 mutirepo 的一些弊端。
这也是随着项目拆分，之后的一种整合模式，它的优势很明显，同时
能避免 mutirepo 所带来的一些问题。
<p>


#### Monorepo 举例
Babel:  [babel仓库](https://github.com/babel/babel)<br>
Vue3:  [vue3仓库](https://github.com/vuejs/core)<br>
ElementPlus:  [element-plus仓库](https://github.com/element-plus/element-plus)<br>
React:  [React仓库](https://github.com/facebook/react)<br>


#### 优点

<p class="fragment" data-fragment-index="1" style="text-align: center; font-size: 18px;"> 更大的复用 </p>
<p class="fragment" data-fragment-index="2" style="text-align: center; font-size: 18px;"> 同一版本，简化依赖 </p>
<p class="fragment" data-fragment-index="3" style="text-align: center; font-size: 18px;"> 统一管理，事半功倍 </p>


#### 缺点
<p class="fragment" data-fragment-index="1" style="text-align: center; font-size: 18px;"> 随着项目的发展，项目会越来越大，管理困难， 仓库太大，版本控制有挑战 </p>
<p class="fragment" data-fragment-index="2" style="text-align: center; font-size: 18px;"> 不利于项目多元化 </p>
<p class="fragment" data-fragment-index="3" style="text-align: center; font-size: 18px;"> 权限管理有挑战 </p>



### 应用

#### Monorepo VS Mutirepo

<p class="fragment" data-fragment-index="1" style="text-align: center; font-size: 18px;">什么样的场景使用 Monorepo，什么样的场景使用 Mutirepo？<p>


#### 场景讨论


##### 场景描述
<p style="font-size: 14px; width: 600px; margin: 0 auto;text-align: justify;line-height: 28px;"  class="fragment">
有一个应用，涉及上百个接口、10 多个业务模块、60 多个页面、并且之后还会不断地增加业务体积。<br>
首先，对于体量如此复杂的项目，需要做好前后端分离。<br>
作为前端、面对 10 多个业务模块、60 多个页面的情况下，需要考虑项目拆分的情况。<br><br>
假定通过以上方法分离出
<span class="hljs-emphasis"> 7 </span>个子项目、
其中<span class="hljs-emphasis"> 5 </span>个业务级别的项目、
<span class="hljs-emphasis"> 2 </span>个基础技术支持方面的项目。<br><br>
当然还有主应用, 以主应用为几个子项目的集结点，子项目当然也可以相互引用。<br>
那么如何来管理这些子项目？
</p>


#### 场景分析
<p style="font-size: 14px; width: 600px; margin: 0 auto;text-align: justify;line-height: 28px;"  class="fragment">
这就涉及了项目管理的内容。<br/>
前面提到有两种项目管理模式, 我们明白 Monorepo 和 Mutirepo 的主要用途，
Monorepo 主要关注项目间的聚合管理，而 Mutirepo 关注的是项目的分离管理。<br/>
结合这几个子项目，发现有 3 个项目在相互引用，他们都被上层项目引用。
</p>


#### 关系：
<p class="fragment" data-fragment-index="1" style="text-align: center; font-size: 18px;">proj1 -> proj2<p>
<p class="fragment" data-fragment-index="1" style="text-align: center; font-size: 18px;">proj3 -> proj2<p>
<p class="fragment" data-fragment-index="1" style="text-align: center; font-size: 18px;">proj4 -> proj1, proj2, proj3<p>


#### 特点分析 1：
<p style="font-size: 14px; width: 600px; margin: 0 auto;text-align: justify;line-height: 28px;"  class="fragment">
proj1、proj2、proj3 项目业务量少但很重要，且技术框架相似。<br/>
像这种情况，单独成立一个项目反而分离了项目、权限过度分散、项目过多造成了管理负担。<br>
这样的情况可以考虑采用 monorepo 方式来管理这几个小项目。
</p>


#### 特点分析 2：
<p style="font-size: 14px; width: 600px; margin: 0 auto;text-align: justify;line-height: 28px;"  class="fragment">
proj4 业务量较多、集结了proj1、proj2、proj3 的项目。前面也提过 monorepo 有它自己的缺点。<br/><br>
对于 monorepo 项目体积增大时，会造成分支过多，多分支合并代码冲突过多、分支权限难管理等问题。
并且 monorepo 支持单一的技术环境、不利于技术的 “多元化” 等等问题。
因此，对于类似 proj4 可以考虑 单独仓库管理项目、在这个项目我们可以采用不同的技术框架，如：react、vue3、vue2、angular、html+jquery等。<br/><br>
当然还有应用的主前端项目，采取的是 multirepo。
</p>



### 实践
[项目地址](http://git.zhihuishu.com/coder/zhs_component_library/tree/feature_react_plugins)




# THE END
# TK U
