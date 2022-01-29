### 第六章 对象
1. `typeof null = Object`
2. 对象是可修改的，是按**引用**操作而不是按值操作的
3. 属性名可以是空字符串`"":true`
4. 使用`new`关键字创建对象
```let o = new Object();
    let a = new Array();
    let d = new Date();
    let r = new map();
```
5. 原型
* 使用`new Object()`初始化的对象继承自`Object.prototype`；通过`new Array（）`创建的对象继承自`Array.prototype`
  
6. `Object.create()`创建一个新对象，其第一个参数为**新对象的原型**（新对象继承第一个参数）。
```
let o1 = Object.create({x:1,y:2}) //o1继承属性x和y
o1.x + o1.y                       // => 3

let o2 = Object.create(Object.prototype) //创建一个普通的空对象
```
  
7. 查询和设置属性(属性名可以理解为inde、key，属性值为value)
```
Object.property = a;
Object["property"] = a;    //使用方括号时，方括号内必须为属性名的字符串形式（加""）
```
   
8. 查询对象中的属性时，可以使用条件属性访问(?.)
```
expression ?. identifier   //?.会先查看expression是否为null或undefined
expression ?. ["identifier"]  //同上
```
  
9. 删除属性（delete）
使用**delete**关键字可以删除属性，但是只能删除自有属性，无法操作继承属性。
  
10. 测试属性是否存在
* 使用`Obj.hasOwnProperty("属性名")`这个API来进行查询，根据返回的bool值进行判断
* 使用"in"（左边为属性名，右边为对象名）
```
let o = {x:1, y: 2}
"x" in o       // => true
"z" in o       // => false    
```
注意： `in`可以判断自有属性以及**继承属性**的有无。hasOwnProperty只能判断自有属性。
  
11. 扩展(复制)对象
* `Object.assign("目标对象","源对象")`
* 使用`Object.keys`、`Object.values`、`Object.entries`遍历对象
```
let o = {x:1, y:2, z:3}
let keys = ""
for(let k of Object.keys(o)){
    keys += k
}
keys  // => "xyz"
```
* 使用操作符`...`
```
let obj1 = {x:1, y:2, z:3};
let obj2 = {a:1, b:2}
let sum = {...obj1, ...obj2}
sum      //=>{x:1, y:2, z:3, a:1, b:2} 
```
注意：如果有同名属性，则以第二个传入的对象里的为准（第二个的同名属性的值直接覆盖第一个的）

### 第七章 数组
* js中数组是动态的，他会自动地按照需要增大或者缩小。其中数组也可以是**稀疏**的，即并非每个index都对应一个value。
* 扩展操作符（...）
  + 扩展操作符是创建数组浅副本的一种方式（只修改数组的值，不改变原来数组的值）
```
    let original = [1,2,3]
    let copy = [...original]
    let copy[0] = 'test'       //这里不会修改original数组的值
```
* 初始化Array
  + 指定数组长度：`let a  = new Array(10);      //可以指定新创建的数组的长度为10`
  + 指定初始化元素： `let a = Array.of(1,2,3)`
* `Array.from()`： 可以操作一个**可迭代对象**（字符串？）或**类数组对象**，并返回一个真正的数组对象。`let trueArray = Array.from(similiarArray)`
* 读取数组元素的两种方式(a = [1,2,3])：
  + `a[0]`
  + `a["0"]   //"0"会被当作属性名，然后转化为index去查找值`
* 查询数组中是否有指定值：
  + `let a = [1,2,3,4]`
    `4 in a     // => false，数组a没有索引为4的对应的值`
    `1 in a     // => true`
* **添加**和**删除**数组元素
  + `push()`：在数组末尾添加一个元素
  + `pop()`：删除数组末尾的一个元素
  + `shift()`：删除数组的首部元素
  + `unshift()`：在数组的首部添加元素
* **循环**数组
  + `for/of`循环
    - ```
        let letters = [1,2,3]
        let string = ""
        for(let letter of letters){
            string += letter
        }
        
        //也可以循环Array.entries（），会返回相应的index和value
        
        let letters = [1,2,3]
        let string = ""
        for(let [index, value] of letters.entries()){
            string += index + value
        }
        
      ```
  + `forEach()`顺序迭代数组（会传入一个函数，之后数组中每个元素都会调用一次这个函数）
    - ```
      let letters = [..."Hello World"]
      let upperCase = "";
      letters.forEach((letter)=>{
        upperCase += letter.toUpperCase();
      })
      ```
* 数组方法汇总
  + 迭代器方法
    - `forEach()`: 如上，没有break机制（不能提前中断循环）
    - `map()`: 返回的是**新数组**，不会修改原来的数组，箭头函数内部绝对不能加花括号
      ```
        let a = [1,2,3]
        a.map(x => x*x)  // => [1,4,9]
      ```
    - `filter()`: 返回的数组始终是**稠密**的，会跳过数组中缺失的元素
      ```
      let a = [1,2,3,4,5]
      a.filter(x => x<3)  //=> [1,2]
      a.filter((x,i) => i%2 === 0)   //x代表元素的值，i代表元素的索引
      ```
    - `find()`和`findInde()`: 遍历数组，找到第一个符合的即停止查找
      ```
      let a = [1,2,3,4,5]
      a.findIndex(x => x===3)  //找到索引值为3的元素
      a.find(x => x%5===0)     //找到第一个是5的倍数的元素
      ```
    - `every()`与`some()`: `every()`代表“∀”，即任意的；`some()`代表“∃”，即存在
      ```
      let a = [1,2,3,4,5]
      a.every(x => x<10)  //true
      a.some(x => x>6)    //false
      ```
    - `reduce()`和`reduceRight()`: 箭头函数内不能加花括号
      ```
      let a = [1,2,3,4,5]
      a.reduce((x,y)=> x+y, 0)  //x+y代表给数组求和，0代表传入函数的初始值
      a.reduce((x,y)=> x*y, 1)  //x*y表示给所有元素累乘，初始值不能为0，不然结果怎么都是0
      ```
    - 使用`flat()`和`flatMap()`打平数组: `flat()`默认打平一层，可以通过传参改变需要打平的层数；`flatMap()`方法与`map()`方法类似，只不过返回的数组会被自动打平（效率要远高于调用`a.map.flat()`方法）
    - 使用`concat()`添加数组：给已有数组里添加数组（添加到数组末尾），同时会自动打平一层数组嵌套；concat()不会修改调用它的数组，而是返回一个新数组（成本较高）
      ```
      let a = [1,2,3]
      a.concat(4,5)          //=>[1,2,3,4,5]
      a.concat([4,5],[6,7])  //=>[1,2,3,4,5,6,7]
      a.concat([4,5],[5,[6,7]])  //=>[1,2,3,4,5,[6,7]]
      a                      //=>[1,2,3],原始数组并没有被改变
      ```
  + 栈和队列方法
    - 通过`push()`, `pop()`, `shift()`和 `unshift()`实现栈和队列操作。使用类似`a.push(...values)`可以显式地打平数组。
    ```
    let stack = [];
    stack.push(1,2);
    stack.pop();
    ```
  + 子数组方法: 使用`slice()`, `splice()`, `fill()`, 和`copyWithin()`
    - `slice()`: `slice(a,b)`返回a<index<b的子数组；此操作不会修改调用它的数组
    - `splice()`: **会修改调用它的数组**;第一个参数指定插入或删除的初始位置，第二个参数指定要从数组中删除（切割）出来的元素个数（默认一直切到数组末尾)，后面还可以接受参数指定要添加到数组中的元素
      ```
      let a = [1,2,3,4,5,6,7,8]
      a.splice(4)    //=>[5,6,7,8],a现在被切割后变成了[1,2,3,4]
      a.splice(1,2)  //=>[2,3]被切割出来(从索引1开始切，切两个元素)，a变成了[1,4]
      
      //切割并添加元素
      let a = [1,2,3,4,5]
      a.splice(2,0,"a","b")    //=>返回空数组[],a变成了[1,2,"a","b",3,4,5]
      a.splice(2,2,[1,2],3)    //=>["a","b"],a变成了[1,2,[1,2],3,3,4,5]
      ```
    - `fill()`: 接受三个参数，第一个参数是要把数组元素设置成的值，第二个（optional）参数指定起始索引，第三个（optional）参数指定终止索引。`fill()`会**操作原始数组**（直接操作统一引用上的数组）。
      ```
      let a = new Array(5);    //创建一个长度为5的空数组
      a.fill(9);               //=>[9,9,9,9,9]
      a.fill(9,1);             //=>[0,9,9,9,9]
      a.fill(8,2,-1);          //=>[0,9,8,8,9]
      ```
    
    - `copyWithin()`:会修改原始数组，但不会改变原始数组的长度。第一个参数指定要把第一个元素复制到的目的索引，第二个参数指定要复制的第一个元素的索引，第三个参数指定要复制的元素切片的终止索引。(此方法对于定型数组会特别高效)
      ```
      let a = [1,2,3,4,5];
      a.copyWithin(1);        //=>[1,1,2,3,4]:把整个数组复制到索引1及之后
      a.copyWithin(2,3,5);    //=>[1,1,3,4,4]:把最后两个元素复制到索引2及之后
      ```
  + 搜索和排序方法
    - `indexOf()`和`lastIndexOf()`:从数组中搜索指定的值并返回第一个找到的元素的索引，如果没有则返回-1。`indexOf()`从左往右找，`lastIndexOf()`从右往左找。**字符串**也有`indexOf()`和`lastIndexOf()`方法。
      ```
      let a = [0,1,2,1,0];
      a.indexOf(1);         //=>1: a[1]===1
      a.lastIndexOf[0];     //=>4: a[4]===0
      ```
    - `includes()`:只接收一个参数，如果数组当中存在这个值，则返回true。
      ```
      let a = [1,3]
      a.includes(3);    //=> true
      ```
    - `sort()`:对数组就地排序并返回排序后的数组(**会修改原数组**)。在不传参调用时，`sort()`按字母顺序对数组元素排序。如果对数组元素执行非字母的排序，则必须给`sort()`方法传入一个比较函数作为参数。(a-b是由小到大，b-a是由大到小)
      ```
      let a = [33,4,1111,222]
      a.sort();           // a == [1111,222,33,4],按字母顺序排
      a.sort((a,b) => a-b);   // a == [4,33,222,1111]
      a.sort((a,b) => b-a);   // a == [1111,222,33,4]
      ```
    - `reverse()`:反转数组中元素的顺序，并返回反序后的数组(**会修改原始数组**)。
      ```
      let a = [1,2,3];
      a.reverse();        //=> [3,2,1]    
      ```
* `join(）`: 把数组中的所有元素全部转换为字符串，然后把他们拼接起来并返回结果字符串。接收一个参数可以指定每个字符之间的分隔符，默认为逗号。
  ```
  let a = [1,2,3];
  a.join();         //=> "1,2,3"
  a.join(" ");      //=> "1 2 3"
  a.join("");       //=> "123"
  ```
* `toString()`:数组的`toString()`方法可以将数组转换为字符串的同时，**自动打平数组**。
  ```
  [1,2,3].toString();      //=> "1,2,3"
  ["a","b","c"].toString();    //=>"a,b,c"
  [1,[2."c"]].toString();      //=>"1,2,c"
  ```
* `Array.isArray()`用于确定一个未知值是否为一个数组
  ```
  Array.isArray([]);       //=>true
  Array.isArray({});       //=>false
  ```
* 数组的长度会自动更新，将原始长度为5的数组设置长度为3，会自动截断后面的数组。