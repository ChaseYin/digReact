# digReact
### 1月18日

React是比较关注用户界面（视图）的js库。（将**数据渲染为HTML视图**的开源JS库）。
* js的缺点
 + 使用js直接操作DOM会造成浏览器进行大量的重绘重排。
 + 原生JS没有**组件化**编码方案，代码复用率低。
* React的特点
 + 采用**组件化**模式，**声明式编码**（原生js是命令式编程），提高开发效率以及组件复用率。
 + 在React Native中可以使用React语法进行**移动端开发**（可以用熟悉的react语法去编写android和ios应用）。
 + 使用**虚拟DOM**（没放在页面上，放在电脑的内存里）+优秀的**Diffing算法**，尽量减少与真实DOM的交互
 + 数据 -> 虚拟DOM（进行虚拟DOM的新旧版本的**比较**，然后只更新新的数据，增加工作效率） -> 页面真实DOM
* 需要的前置知识
 + 判断this的指向
 + 对class有基本了解
 + ES6语法规范（箭头函数，解构赋值等）
 + npm包管理器
 + 原型、原型链
 + 数组常用方法（统计数组、遍历数组、过滤数组、条件筛选、条件求和）
 + js模块化
  
babel使用情况：ES6 ==> ES5; import; react写的是jsx，但是浏览器只认js，jsx ==> js  

* 关于虚拟DOM
  + 本质是Object类型的对象（一般对象）
  + 虚拟DOM比较轻量（属性较少），真实DOM较“重”。因为虚拟DOM只是React在用，所以不需要真实DOM那么多的属性
  + 虚拟DOM最终会被React转换为真实DOM，然后呈现在页面上。

* JSX = JavaScript + XML
  + XML早期用于存储和传输数据 

* jsx语法规则
  + 定义虚拟DOM时，不要写引号
  + 标签中混入JS表达式时要用{}
    - JS表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方。例如：a、a+b、demo（1）、arr.map（）、function test(){}
    - JS语句（JS代码）：if(){}、for(){}、switch(){case:xxxx}
  
    *如果不能令const x = 右边的值，就是代码。*
      
  + 样式的类名指定要用className而不是class
  + 内联样式，要用`style={{key:value}}`的形式去写
  + 虚拟DOM里只能有一个根标签（多个h1标签需要用div括起来)
  + 标签必须闭合：`<input type="text"/>`
  + 标签首字母
    - 若小写字母开头，则将该标签转换为html中同名的元素，若html中无该标签，则报错。
    - 若大写字母开头，react就去渲染对应的组件，若组件没有定义，则报错
  + 

* React无法遍历object
* 模块与组件、模块化与组件化的理解
  + 模块
    - index.js拆成 a.js, b.js, c.js。每个js文件负责某一部分的服务。
    - 作用：可以复用js，简化js的编写，提高js运行效率
  + 组件
    - 用来实现局部功能效果的代码和资源的集合（html/css/js/image等等）
    - 作用：复用编码，简化项目编码，提高运行、工作效率
  + 模块化
    - 当应用的js都以模块来编写
  + 组件化
    - 当应用是以多组件的方式来实现，这个应用就是一个组件化的应用

* 组件(组件的首字母**必须大写**)
  - 函数式组件（函数名就是组件名）
  - 类式组件（类名就是组件名）
* 严格模式：禁止自定义的函数里的this指向window（使用babel会自动开启严格模式）
  
* 执行`ReactDOM.render(<MyComponent/>)`之后发生了什么？
  + React解析组件标签，找到MyComponent组件
  + 发现组件是使用函数定义的，随后调用该函数，将返回的DOM转为真实DOM然后呈现在页面中
  + call函数是为了改变this指向，call的传参是谁就指向谁
  
* 1. 类中的构造器不是一定要写的，要对实例进行一些初始化操作的时候才写。
  2. 重复的传参要使用super调用父类的构造器（在子类的构造器中调用）
  3. 类中所有的方法都是放在了类的原型对象上，供实例使用
  
* 类式组件中的render是放在类的原型对象上，供实例（组件标签就可以理解为一个实例）使用
  + 若react发现该组件是用类定义的，则new一个该实例的对象，并通过该实例调用原型上的render（）方法
  + 将render（）返回的虚拟DOM转换为真实DOM

### 1月20日
* 复杂组件：有状态（state）的组件。组件的状态**驱动**着页面。状态是**组件实例对象**的而不是组件类的。
* 组件实例对象的三大核心属性
  + state
    - react官方要求this.state初始化为object：`this.state = {}`
    - react的绑定要写作`onClick`,给点击事件绑定函数要写作`onClick = {demo}`而不是`onClick = {demo()}`。加了括号会给onClick赋值（函数的返回值，一般是undefined）。
    - **状态state不可直接更改**（直接赋值），需要借助API：setState()
    - setState的操作是合并动作（只更新发生变化的属性，其余属性保持不变），状态修改完毕之后，react会帮助重新渲染
    - 类中可以直接写一行`a=1`来给obj加一行属性`a：1`
    - 状态初始化不需要constructor，可以直接state={}
    - 方法可以使用**赋值语句加箭头函数**：`demo=()=>{}`可以直接使得this指向当前类的**实例对象**
    - 组件被称为“状态机”，通过更新组件的state来更新对应页面的显示（重新渲染组件）
    - 组件的render的this指的是组件实例对象
    - 组件自定义的方法中的this指向为undefined，如何解决？
      a. 强制绑定this：通过函数对象的`bind（）`；`this.demo = this.demo.bind(this)`
      b. 赋值语句加箭头函数 `demo = () =>{}`
    - 状态数据不能直接更新或修改（只能使用setState）
  + props
    - 可以通过组件传递props` <Person name="tom" age="19"/>`
    - 给传递的props添加约束时：可以引用Prop-types库，然后定义`Object.propTypes = {name: Proptypes.string.isRequired}`(isRequired表示该属性必需)
    - 可以使用`Object.defaultProps={}`为组件添加默认值，当相关属性没有接收到参数时，则以默认值渲染
    - props是**只读的**，**不允许修改**
    - 如果在类组件中添加属性，需要使用`static`关键字。 `static propTypes = {}`
    - 类组件的构造器函数可以省略，如果不省略，则必须要有`constructor(props){super(props)}` 
    - 函数式组件使用props可以直接利用组件标签传参，然后`function fuc(props){}`
    
  + ref 
    - 使用字符串类型的ref可以替换原生的id，作为节点的标识（不推荐使用）
    - 回调函数形式的：`<input ref = {(currentNode)=>{this.input1 = currentNode}}>`(ref会返回当前节点DOM，然后便可以获取到这个node)
    - 箭头函数中的传参总是指向自己（或包含此函数的节点 ）
    - createRef()调用后可以返回一个容器，此容器可以存储被ref标识的节点,只能存一个节点，“专人专用” （react最**推荐**的ref使用形式）
    - 

    
* 回调函数：
  + 你定义的函数
  + 你没有调用
  + 这个函数最终执行了（别人在调）
