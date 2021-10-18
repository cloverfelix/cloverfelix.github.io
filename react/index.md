# React


React

## 注意事项:

1. 使用脚手架创建react项目后，运行项目只认名为src目录，如果修改了src的目录文件名，那么运行时就找不到src目录。
2. react中内联样式的写法：style={{xxx:xxx}}，注意不是变量的要加引号
3. react中标签上的class用的是className
4. react中不能直接渲染一个对象到页面，可以展示对象的某个属性
5. github搜索案例中的index.html引入了第三方样式bootstrap.css，所以如果运行其他例子的时候，需要讲那一行代码注释掉，否则会出现样式问题
6. 修改react脚手架项目的src文件名前，记得先终止项目运行

## 1.定义

是将**数据**渲染为**html视图**的开源js库

## 2.特点

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002100556.png)

解释react的虚拟dom应用：

- 原来的纯js，每次更新数据，之前所有的数据全部报废，然后又重新加载一遍全部数据。比如原来往真实dom中存放100个人，后来又新加了一个人，相当于原来100个人全作废，又重新加载一遍这101个人，存入真实dom

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002100611.png)

- react，虚拟dom会进行**新旧虚拟dom比较**（比较的最小力度是节点），如果是相同的，那么在真实dom中直接采用，而不会重新加载一遍，不用所有数据都重来，解决js重排重绘问题

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002100625.png)

## 3.上手

### 3.1 依赖包

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002100659.png)

- babel：

  1. 将ES6转为ES5
  2. 将jsx转为js（react写的是jsx）

- react.development：

  核心库，一旦引入就有一个对象React

- react.dom.development

  扩展库：支持react操作dom，一旦引入就有一个ReactDOM对象

### 3.2 第一个项目

1. 准备好一个容器

   ```html
   <div id="test"></div>
   ```

2. 引入库

   顺序要求：核心库要比扩展库先引入

   ```html
   <script type="text/javascript" src="../js/react.development.js"></script>
       <script type="text/javascript" src="../js/react-dom.development.js"></script>
       <script type="text/javascript" src="../js/babel.min.js"></script>
   ```
   
3. 创建虚拟dom

   ~~~html
   // 步骤三：创建虚拟dom 
   // 此处一定不要写引号，因为不是字符串，jsx中标签和js可以混写
   const VDOM=<h1>hello,react</h1>
   ~~~

4. 渲染虚拟dom到页面

   render：是替换，不是追加，所以如果再渲染一个虚拟DOM，那么之前的会被替换
   
   ```html
   // 步骤四：渲染虚拟dom到页面中，第一个参数是虚拟dom，第二个参数是容器。由于react不支持选择器的写法，所以需要操作手动获取结点
   
   ReactDOM.render(VDOM,document.getElementById('test'))
   ```
   
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002100821.png)

### 3.3 创建虚拟dom的两种方式

1. js创建

   ```html
   <body>
       <!-- 步骤一：创建一个容器 -->
       <div id="test"></div>
       <!-- 步骤二：引入库，不需要babel库了，因为写的是js不是jsx，不需要转换 -->
       <script type="text/javascript" src="../js/react.development.js"></script>
       <script type="text/javascript" src="../js/react-dom.development.js"></script>
       <!-- 表示写的是js -->
       <script type="text/javascript">
           // 步骤三：创建虚拟dom 
           const VDOM=React.createElement('h1',{id:'title'},'hello,react,02')
           // 步骤四：渲染虚拟dom到页面中,第一个参数是虚拟dom，第二个参数是容器
           ReactDOM.render(VDOM,document.getElementById('test'))
       </script>
   </body>
   ```

  ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002100857.png)

2. jsx创建

   ```html
   <body>
       <!-- 步骤一：创建一个容器 -->
       <div id="test"></div>
       <!-- 步骤二：引入库 -->
       <script type="text/javascript" src="../js/react.development.js"></script>
       <script type="text/javascript" src="../js/react-dom.development.js"></script>
       <script type="text/javascript" src="../js/babel.min.js"></script>
       <!-- 表示写的是jsx，用babel转换 -->
       <script type="text/babel">
           // 步骤三：创建虚拟dom 
           // 此处一定不要写引号，因为不是字符串，如果要将html代码以格式化的方式写，就加一个()
           const VDOM=(
               <h1>
                   <span id="test">hello,react,02</span>
               </h1>
               )
           // 步骤四：渲染虚拟dom到页面中,第一个参数是虚拟dom，第二个参数是容器
           ReactDOM.render(VDOM,document.getElementById('test'))
       </script>
   </body>
   ```

  ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002100916.png)
   
   jsx是解决用原生js创建虚拟dom太繁琐的问题，jsx是js的语法糖，babel就是将jsx翻译成浏览器认识的js

### 3.4 什么是虚拟dom

```html
 <!-- 步骤一：创建一个容器 -->
     <div id="test"></div>
     <div id="demo"></div>
     <!-- 步骤二：引入库 -->
     <script type="text/javascript" src="../js/react.development.js"></script>
     <script type="text/javascript" src="../js/react-dom.development.js"></script>
     <script type="text/javascript" src="../js/babel.min.js"></script>
     <!-- 表示写的是jsx，用babel转换 -->
     <script type="text/babel">
         // 步骤三：创建虚拟dom 
         // 此处一定不要写引号，因为不是字符串
         const VDOM=<h1>hello,react</h1>
         // 步骤四：渲染虚拟dom到页面中,第一个参数是虚拟dom，第二个参数是容器
         ReactDOM.render(VDOM,document.getElementById('test'))



         //什么是vdom和tdom
         const TDOM=document.getElementById('demo')
         console.log('VDOM',VDOM)
         console.log("TDOM",TDOM)
     </script>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002100949.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101030.png)

### 3.5 jsx

xml和json写法

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101044.png)

jsx

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101101.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101115.png)

### 3.6 jsx语法规则

1. 定义虚拟dom时不要加引号，直接写html标签

2. 如果标签中嵌套**js表达式**（不是js语句、代码），需要用{}引起来

   - js语句（代码）：if(){}、for(){}、switch(){case:}

   - js表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方，如：a、a+b、fun(1)、arr.map()。包括function test (){}也算表达式，因为用const a=function test (){}返回的就是一个函数，也有返回值。即表达式的结果一定是一个值

3. 样式的类名不要用class，而用className。因为ES6中：class是关键字（外部样式中）

4. 内联样式：需要用style={{key:value,key:value}}的形式去写

   - 外面的大括号表示里面写的是js，里面的大括号表示写的是一个对象

   - key需要遵守驼峰原则，比如js中的font-size，在jsx中就是fontSize。

   - value要用‘ ’引起来，因为不是一个变量

5. 虚拟DOM必须只有一个根标签

6. 标签必须闭合

7. 标签首字母：

   - 若小写字母开头，则将改标签转为html中同名元素，若html中无同名元素，就报错
   - 若是大写字母开头，react则去渲染对应的组件，若组件没有定义，则报错

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>jsx语法规则</title>
</head>
<body>
    <!-- 步骤一：创建一个容器 -->
    <div id="test"></div>

    <style>
        .hhh{background:peru;}
    </style>

    <!-- 步骤二：引入库 -->
    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <!-- 表示写的是jsx，用babel转换 -->
    <script type="text/babel">
    
        const id="tESt"
        const child="HeLlo,rEacT,02"

        // 步骤三：创建虚拟dom 
        // 此处一定不要写引号，因为不是字符串，如果要将html代码格式化，就加一个()
        const VDOM=(
            <div>
                <h1>
                    <span className="hhh" style={{fontSize:'30px',color:'white'}} id={id.toLowerCase()}>{child.toLowerCase()}</span>
                </h1>
                <input type="text" />
            </div>
            )

        // 步骤四：渲染虚拟dom到页面中,第一个参数是虚拟dom，第二个参数是容器
        ReactDOM.render(VDOM,document.getElementById('test'))
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101135.png)

### 3.7 jsx小练习（遍历）

注意：

1. {}中只能写js表达式，即有返回值的。不能写for这种js语句

2. 页面中遍历出来的每一个child结点都必须要有一个唯一key属性，如：如果单个单个写出的li，浏览器会自动给加上。但是如果是遍历出来的li，就必须显示为每个结点添加唯一key属性

 ```html
 <!DOCTYPE html>
 <html lang="en">
 <head>
     <meta charset="UTF-8">
     <meta http-equiv="X-UA-Compatible" content="IE=edge">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>example</title>
 </head>
 <body>
     <!-- 步骤一：创建一个容器 -->
     <div id="test"></div>
 
     <style>
         .dd{
             font-size:20px;
         }
     </style>
     <!-- 步骤二：引入库 -->
     <script type="text/javascript" src="../js/react.development.js"></script>
     <script type="text/javascript" src="../js/react-dom.development.js"></script>
     <script type="text/javascript" src="../js/babel.min.js"></script>
     <!-- 表示写的是jsx，用babel转换 -->
     <script type="text/babel">
         const dt="前端js框架列表"
         const dd=["vue","react","angular"]
         const VDOM=(
             <div>
                 <h1>{dt}</h1>
                 <ul>
                     {
                         // 由于{}中只能是js表达式，所以这里用处理数组的方式遍历dd数组
                         dd.map((item,index)=>{
                             // 给li设置key的值为每一个item的index，注意取值时加{}
                             return <li key={index}>{item}</li>;
                         })
                     }
                 </ul>
             </div>
         )
         ReactDOM.render(VDOM,document.getElementById('test'))
     </script>
 </body>
 </html>
 ```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101148.png)

### 3.8 复习

#### 1.类的基本知识

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101204.png)

- 类中定义的方法都是放在了类的原型对象上，所有类实例共用的。但是类中构造器中的属性或方法，是实例自身的，不是公用的

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>类的基本知识</title>
    </head>
    <body>
        <!-- 步骤二：引入库 -->
        <script type="text/javascript" src="../js/react.development.js"></script>
        <script type="text/javascript" src="../js/react-dom.development.js"></script>
        <script type="text/javascript">
            class Person {

                // 构造器方法
                constructor(name,age){
                    // this指的是类的实例对象
                    this.name=name;
                    this.age=age;
                }

                // 一般方法
                speak(){
                    //通过Person实例调用的，那么this就指向Person实例
                    console.log('我叫：'+this.name+' 我的年龄是：'+this.age);
                }
            }

            class Student extends Person{
                //要么不写构造器，那么会自动继承父类的构造器
                constructor(name,age,grade){
                    //如果写了构造器，必须要用super()继承父类的构造器，并且super顺序一定要排在第一
                    super(name,age);
                    this.grade=grade;
                    // 这表示每一个实例对象中都有一个属性school，不需要传参，默认有
                    this.school='尚硅谷';
                }

                // 重写父类中的speak方法
                speak(){
                    console.log('我叫：'+this.name+' 我的年龄是：'+this.age+' 我读的年级是：'+this.grade);
                }

                //子类自己的方法
                study(){
                    console.log('我很爱学习');
                }
            }

            // 创建一个Person的实例对象
            const p1=new Person('憨憨',12);
            console.log(p1); //输出的格式是：类名{属性：属性值，属性：属性值}
            p1.speak(); //p1对象调用类中的speak()方法，那么方法中的this指向的是p1对象

            //创建一个Student的实例对象
            const s1=new Student('小学生',18,'一年级');
            console.log(s1);
            s1.speak();
            s1.study();
            
            
             // -------------------直接在类中写赋值语句-----------------------------------
            class Car{
                constructor(){
                    // 表示往每一个对象中添加一个固定的属性wheel，且固定为4
                    this.wheel=4;
                }
                // 也表示往每一个对象中添加一个固定的属性a，且值为1。应用场景：当要往每一个对象中添加一个固定值的属性时，可以直接用这种赋值语句，不需要写构造函数
                a=1
            }

            const c1=new Car();
            console.log(c1);
        </script>
    </body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101221.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101233.png)

#### 2.原生js的事件绑定

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>原生js的事件绑定</title>
</head>
<body>

    <button onclick="fun()">按钮1</button>
    
    <script type="text/javascript">
        function fun(){
            alert("按钮被点击了");
        }
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101249.png)

#### 3.类中的方法this

类中的方法，在js中默认开启了局部严格模式。所以类中的方法:

- 如果以对象实例调用的，那么this就指向对象实例。

- 如果是以函数形式调用的，那么this原本应该指向window，但是由于类中的方法默认开启了严格模式，严格模式不允许函数形式调用的方法中的this指向window，所以如果类中的方法是以函数形式调用的，this就是undefined

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>类中的方法this指向</title>
</head>
<body>
    <script type="text/javascript">
        class People{
            constructor(name,age){
                this.name=name;
                this.age=age;
            }
            speak(){
                console.log(this);
            }
        }

        const p1=new People('哈哈',12);
        // 由对象实例调用的speak()方法，那么speak()方法中的this指向类实例对象
        p1.speak();

        // 将p1对象的speak方法赋值给x，那么x()就表示以函数的方式调用的，那么this就指向window
        // 但是js默认在类中的方法中开启局部严格模式，严格模式是不允许函数形式调用的方法中的this指向window，所以这里的this是undefined
        const x=p1.speak;
        x();

        function test1() {
            console.log(this);
        }
        // js中的函数没有开启严格模式，所以这里的this指向的是window
        test1();

        function test2() {
            // 手动开启严格模式
            'use strict'
            console.log(this);
        }
        // 由于严格模式中不允许函数形式调用的方法中的this指向window，所以这里的this是undefined
        test2();
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101308.png)

#### 4.函数的bind方法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>函数的bind方法</title>
</head>
<body>
    <script type="text/javascript">
        function demo() {
            console.log(this);
        }

        // 非严格模式下，函数调用的方法中的this指向window
        demo();

        // 意思是：调用demo函数的bind方法，返回一个新函数，并且指定这个新函数的this指向的是{a:1,b:2}这个对象（函数体还是跟demo一样，但是this 指向不一样了）
        const a=demo.bind({a:1,b:2});
        // 调用这个新函数
        a();
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101323.png)

#### ５.展开运算符

1. 展开数组：...数组
2. 连接数组：[...数组1,...数组2]
3. 函数传参，批量接收参数，将所有参数封装在一个数组中：function(...参数){}
4. 以字面量形式复制对象：{...对象}

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>展开运算符</title>
</head>
<body>
    <script type="text/javascript">
        // 1.展开运算符展开数组
        const arr1=[1,2,3];
        console.log(...arr1);

        // 2.展开运算符可用于连接两个数组
        const arr2=[4,5,6];
        const arr3=[...arr1,...arr2];
        console.log(arr3);

        // 3.作为函数传参，批量接收参数，将所有参数封装在一个数组中
        function sum(...arr) {
            // 数组中的求和方法：reduce
            return arr.reduce((preValue,currentValue)=>{
                return preValue+currentValue
            })
        }
        console.log(sum(1,2,3,4));

        // 4.展开运算符不能展开对象，但是能以字面量的方式复制一个对象，能达到深度复制的效果。局限是：只能深度复制一层，如果对象中还嵌套了对象，那么复制不到第二层
        // 注意一定要在外面加{}，这是ES6的语法规范。如果不加，那么 ...对象 表示的是展开一个对象，而展开运算符是不能展开对象的。
        class People{
            constructor(name,age){
                this.name=name;
                this.age=age;
            }
        }
        const p1=new People("憨憨","18");

        // 4.1 深度复制对象方法一：以字面量的形式复制对象。局限是：只能深度复制一层，如果对象中还嵌套了对象，那么复制不到第二层
        const p2={...p1};
        // 深度复制对象方法二：先将对象转为字符串，再转为对象
        // const p2=JSON.parse(JSON.stringify(p1));
        p1.name="修改";
        console.log(p1); // p1被修改
        console.log(p2); // p2没有被同步修改

        // 4.2 复制的同时修改属性(同名覆盖，不同名就添加)
        const p3={...p1,name:"对象3",address:"大道"};
        console.log(p3);
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101339.png)

#### 6.scrollHeight

只有子元素比父元素高，讨论滚动条才有意义

scrollHeight：代表包括当前不可见部分的元素的高度

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101352.png)

clientHeight：当前可见的高度

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101404.png)

scrollTop：**代表在有滚动条时**，滚动条向下滚动的距离也就是**元素顶部被遮住部分的高度**。在没有滚动条时`scrollTop==0`恒成立。

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101415.png)

offsetTop：与有无滚动条无关。 当前元素顶部距离最近父元素顶部的距离,和有没有滚动条没有关系。

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101427.png)


#### 7.修改对象属性，返回新对象

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>7_修改对象属性，返回新对象</title>
</head>
<body>
    <div id="test1"></div>
    <script  type="text/javascript">
    // 修改b的值为3
        const p1={a:1,b:2};
        const p2={...p1,b:3};
        console.log(p1);
        console.log(p2);
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101445.png)

#### 8.reduce数组条件计数

（10.8用到）

reduce的第一个参数：函数

- 该函数的第一个参数pre：上一次调用函数的返回值
- 该函数的第二个参数current：当前遍历到的数组项
- 该函数的返回值：自动赋值给下一次调用函数时的pre，即下一次调用函数的参数pre的值为上一次调用函数的返回值

reduce的第二个参数：pre的初始值,，如果不指定则默认为数组中的第一个值，而current就从第二个值开始

reduce返回值：最终累积的pre值

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>8_reduce数组条件计数</title>
</head>
<body>
    <div id="test1"></div>
    <script  type="text/javascript">
        // 求数组项中对象属性a的值为2的对象个数
        const arr=[{a:1,b:2},{a:2,b:2},{a:2,b:2},{a:3,b:2}];
        const count=arr.reduce((pre,current)=>{
            // 切记：二目运算符中使用三目运算符，是需要加括号的。赋值语句中使用三目运算符，不需要加括号
            return pre + (current.a===2?1:0);
        },0)
        console.log(‘数量为’,count);
        
        // 如果不指定pre初始值，则默认为数组中的第一个值，而current就从第二个值开始
        function sum(...arr) {
            // 数组中的求和方法：reduce
            return arr.reduce((preValue,currentValue)=>{
                return preValue+currentValue
            })
        }
        console.log(sum(1,2,3,4));
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101458.png)


#### 9.多选框checked和defaultChecked

当多选框没有添加onChange事件时，并且此时的checked值取决于一个变量，那么就没有途径去修改这个变量从而达到修改checked值。所以当没有添加onChange事件时，也不能在标签上设置checked属性

但是可以设置标签的defaultChecked属性：但是该属性的值为它取决的那个变量的初始值，后面无论怎么修改那个变量的值，都不会改变defaultChecked的值。

#### 10.结构赋值

(12.2用到)

结构赋值和连续结构赋值

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>9_结构赋值</title>
</head>
<body>
    <div id="test1"></div>
    <script  type="text/javascript">
        const p1={a:1,b:2};
        // 一般获取p对象中的a属性
        console.log(p1.a);
        // 结构赋值
        const {a}=p1;
        console.log(a);

        // 连续结构赋值
        const p2={a:{b:{c:1}}};
        // 一般拿到p2对象中的c属性
        console.log(p2.a.b.c);
        // 连续结构赋值拿到p2对象中的c属性
        const {a:{b:{c}}}=p2;
        // 但是注意这样写只能拿到c，不能拿到a和b，所以要输出a和b，还是需要p.a  p.a.b
        console.log(c);

        // 连续结构赋值并给拿到的属性取别名
        const p3={a:{b:1}};
        // 表示拿到了p2中的a对象的b属性，并给b属性取了给别名
        const {a:{b:hahaha}}=p3;
        // 取了个别名
        console.log(hahaha);
        // 原属性还在
        console.log(p3.a.b);
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101515.png)


#### 11.单引号和反引号的区别

相当于$ ()–命令替换

反引号中可以解析变量：

如：表示在反引号内取变量keywords

```js
`https://api.github.com/search/users?q=${keywords}`
```

而单引号里面只能是字面量字符串

#### 12 qs：键值对与urlencoded之间的转换

要先安装querystring包，如果是react脚手架项目，react脚手架已经自动帮你安装好这个包了，只需要导入即可

```js
const p1={name:"tom",age:11}
        const str1=qs.stringify(p1)
        console.log(str1)

        const str2="name=haha&age=23"
        const p2=qs.parse(str2)
        console.log(p2);
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101528.png)


### 3.8 模块与组件

模块：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101541.png)


组件：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101553.png)


模块化：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101604.png)


组件化：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101618.png)


#### 1.函数式组件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>函数式组件</title>
</head>
<body>
     <!-- 步骤一：创建一个容器 -->
     <div id="test"></div>

     <!-- 步骤二：引入库 -->
     <script type="text/javascript" src="../js/react.development.js"></script>
     <script type="text/javascript" src="../js/react-dom.development.js"></script>
     <script type="text/javascript" src="../js/babel.min.js"></script>

     <!-- 表示写的是jsx，用babel转换 -->
     <script type="text/babel">
        // 创建函数式组件
        // 注意这里的函数名首字母必须大写，因为组件名必须是首字母大写的，如果是小写的就会被当成标签，去html库中查找。
        function Demo(){
            console.log(this); //这里的this为undefined，不是指向window，因为babel解析代码会转换为严格模式，严格模式是不允许自定义函数中的this指向window的
            return <h2>我是函数定义的组件</h2>
        }
        
        // 渲染组件到页面ReactDOM.render(组件标签，容器)，注意：这里的标签记得闭合
ReactDOM.render(<Demo/>,document.getElementById("test"))
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101637.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101649.png)

#### 2.类式组件

要求：

- 必须继承React.Component类
- 类中必须写render(){return xxx}
- 不要求写构造函数

知识点：

- 用类定义组件中的render(){}方法是放在改类的原型对象中的，供实例使用。
- 类中的this指的是组件实例对象，即用类定义的组件中，类的实例对象

```html
 <!DOCTYPE html>
 <html lang="en">
 <head>
     <meta charset="UTF-8">
     <meta http-equiv="X-UA-Compatible" content="IE=edge">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>类式组件</title>
 </head>
 <body>
     
     <!-- 步骤一：创建一个容器 -->
     <div id="test"></div>

     <!-- 步骤二：引入库 -->
     <script type="text/javascript" src="../js/react.development.js"></script>
     <script type="text/javascript" src="../js/react-dom.development.js"></script>
     <script type="text/javascript" src="../js/babel.min.js"></script>

     <!-- 表示写的是jsx，用babel转换 -->
     <script type="text/babel">
        // 类定义组件必须继承React.Component,而且要写render(){return xxx}
        class MyComponent extends React.Component{
            // render是放在MyComponent的原型对象上，供实例使用
            render(){
                // 这里的this指的是组件实例对象，即用类定义的组件中，类的实例对象
                console.log(this);
                
                return <h2>我是类定义的组件（一般用于复杂组件）</h2>
            }
        }

        // 将组件渲染到容器中
        ReactDOM.render(<MyComponent/>,document.getElementById('test'));
     </script>
 </body>
 </html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101841.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101853.png)

### 3.9  **组件实例**的三大核心属性

#### 3.9.1 state

- **组件的数据存放在状态里，状态的改变驱动着页面的改变**。是由组件实例对象生成的，因为state存放在类中的this对象里，而this表示的是组件实例对象
- 组件就是一个“状态机”，通过更新组件的state来更新对应的页面显示（重新渲染组件）

##### 1.初始化state

初始化state要求：

- 实例中要用到state属性，所以必须要写构造函数初始化state属性

- state值必须是一个对象或者为null

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>state状态</title>
  </head>
  <body>
      <!-- 步骤一：创建一个容器 -->
      <div id="test"></div>
  
      <!-- 步骤二：引入库 -->
      <script type="text/javascript" src="../js/react.development.js"></script>
      <script type="text/javascript" src="../js/react-dom.development.js"></script>
      <script type="text/javascript" src="../js/babel.min.js"></script>
  
      <!-- 表示写的是jsx，用babel转换 -->
      <script type="text/babel">
          class Weather extends React.Component{
              // 实例中要用到state属性，所以必须要写构造函数初始化state属性
              constructor(props){
                      // 只要继承了某个类，并且写了构造函数，就必须要写super()
                      super(props);
                      // state必须是一个对象，或者为null
                      this.state={'isHot':true};
                  }
  
              render(){
                  return <h2>今天天气真{this.state.isHot?'炎热':'寒冷'}</h2>;
              }
          }
  
          // 将组件渲染到页面中
          ReactDOM.render(<Weather/>,document.getElementById('test'));
      </script>
  </body>
  </html>
  ```

##### 2.react事件绑定

事件绑定要求：

- react中的onxxx事件，都必须用驼峰命名。如原生的js中的onclick，在react中就得用onClick

- 事件值只需要函数名，且必须为函数，而不是一个字符串，所以要用{}取

- 事件值中不要加()，函数名()：表示调用函数后的返回值

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>state状态</title>
  </head>
  <body>
      <!-- 步骤一：创建一个容器 -->
      <div id="test"></div>
  
      <!-- 步骤二：引入库 -->
      <script type="text/javascript" src="../js/react.development.js"></script>
      <script type="text/javascript" src="../js/react-dom.development.js"></script>
      <script type="text/javascript" src="../js/babel.min.js"></script>
  
      <!-- 表示写的是jsx，用babel转换 -->
      <script type="text/babel">
          class Weather extends React.Component{
              // 实例中要用到state属性，所以必须要写构造函数初始化state属性
              constructor(props){
                      // 只要继承了某个类，并且写了构造函数，就必须要写super()
                      super(props);
                      // state必须是一个对象，或者为null
                      this.state={'isHot':true};
                  }
                  
              render(){
                  // 1.注意这里的事件名要改为驼峰。2.事件值只需要函数名，且必须为函数，而不是一个字符串，所以要用{}取。3.事件值中不要加()，函数名()：表示调用函数后的返回值
                  return <h2 onClick={demo}>今天天气真{this.state.isHot?'炎热':'寒冷'}</h2>;
              }
          }
  
          function demo() {
              console.log("按钮被点击");
          }
  
          // 将组件渲染到页面中
          ReactDOM.render(<Weather/>,document.getElementById('test'));
      </script>
  </body>
  </html>
  ```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101913.png)


##### 3.类组件中方法中的this

- babel解析，也会开启严格模式，所以由函数形式调用的方法中的this都不允许指向window，都为undefined
- js中类中自定义方法中也会默认开启局部严格模式，所以由函数形式调用的方法中的this，就指向undefined。但是如果由实例对象调用的方法中的this，就指向对象实例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>类方法中的this</title>
</head>
<body>
    <!-- 步骤一：创建一个容器 -->
    <div id="test"></div>

    <!-- 步骤二：引入库 -->
    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <!-- 表示写的是jsx，用babel转换 -->
    <script type="text/babel">
        class Weather extends React.Component{
            // constructor中的this一定是指向实例对象的
            // 实例中要用到state属性，所以必须要写构造函数初始化state属性
            constructor(props){
                    // 只要继承了某个类，并且写了构造函数，就必须要写super()
                    super(props);
                    // state必须是一个对象，或者为null
                    this.state={'isHot':true};
                }

            // render方法中的this指的是类实例对象，因为组件在渲染的时候，自动创建了类实例，并且用类实例调用了render方法
            render(){
                // 注意：3.如果这里写this.demo，意思是：将实例对象中的demo方法，作为onClick的回调函数去调用，不是由实例对象调用，而是点击这个结点后由react自动调用，属于函数调用，所以demo方法中的this为undefined
                return <h2 onClick={this.demo}>今天天气真{this.state.isHot?'炎热':'寒冷'}</h2>;
            }

            // 将方法写在类里面
            demo() {
                // 1.babel解析，也会开启严格模式，所以由函数形式调用的方法中的this都不允许指向window，都为undefined
                // 2.类中自定义方法中也会默认开启局部严格模式，所以由函数形式调用的方法中的this，就指向undefined。但是如果由实例对象调用的方法中的this，就指向对象实例
                console.log(this);
            }
        }

        // 将组件渲染到页面中
        // Weather类的实例对象是由react内部创建的
        ReactDOM.render(<Weather/>,document.getElementById('test'));

        // 测试由对象实例调用类组件中的方法(这样写是不规范的，因为类组件的实例对象是由react创建的，不是由我们手动创建)
        const w1=new Weather();
        w1.demo();
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101930.png)

##### 4.用bind解决类组件中方法中的this指向问题

用函数的bind，强制修改函数中this的指向，不管该函数是以什么方式调用，this都是指向你指定的东西

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>解决类方法中的this指向</title>
</head>
<body>
    <!-- 步骤一：创建一个容器 -->
    <div id="test"></div>

    <!-- 步骤二：引入库 -->
    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <!-- 表示写的是jsx，用babel转换 -->
    <script type="text/babel">
        class Weather extends React.Component{
            // constructor中的this一定是指向实例对象的
            // 实例中要用到state属性，所以必须要写构造函数初始化state属性
            constructor(props){
                    // 只要继承了某个类，并且写了构造函数，就必须要写super()
                    super(props);
                    // state必须是一个对象，或者为null
                    this.state={'isHot':true};

                    //构造函数中的this一定是指向实例对象的
					// 调用实例对象中 demo 函数的bind方法，将类中 demo 函数中的this修改为指向实例对象
					 // 再返回新函数，将它添加到每一个实例对象中。这样每个实例对象中都有一个 demo 函数，Weather 实例对象中有一个 demo 函数
                    this.demo1=this.demo.bind(this);
                }

            // render方法中的this指的是类实例对象，因为组件在渲染的时候，自动创建了类实例，并且用类实例调用了render方法
            render(){
                // 3.注意：那么这里的this.demo1表示的将对象实例中的demo1方法作为onClick方法的回调，尽管还是以函数方式调用的，但是用bind强制修改了demo方法的this为实例对象并作为一个新的方法：demo1，所以demo1中的this指向实例对象
                return <h2 onClick={this.demo1}>今天天气真{this.state.isHot?'炎热':'寒冷'}</h2>;
            }

            // 将方法写在类里面
            demo() {
                // 1.babel解析，也会开启严格模式，所以由函数形式调用的方法中的this都不允许指向window，都为undefined
                // 2.类中自定义方法中也会默认开启局部严格模式，所以由函数形式调用的方法中的this，就指向undefined。但是如果由实例对象调用的方法中的this，就指向对象实例
                console.log(this); 
            }
        }

        // 将组件渲染到页面中
        // Weather类的实例对象是由react内部创建的
        ReactDOM.render(<Weather/>,document.getElementById('test'));
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002101946.png)

##### 5.setState的使用

- 状态里的数据不能直接更改，即直接取到state属性，直接改变它的state属性值。要借助一个内置的API去更改。

- setState更新state数据，不是替换，而是合并，重名的属性就替换，不重名的属性依然保留
- 渲染时，标签用到几次，那么react就创建了几个类组件对象，构造器就相应的调用了几次，下面这个例子constructor就调用了一次
- render调用了1+n次，1表示初始化的那次，最开始渲染页面的那次。n表示状态state切换的次数，状态的改变驱动着页面的改变。所以每次改变state，render都会重新被调用一次
- 下面例子中的demo调用的次数：就是点击的次数。因为demo中修改了state的值，每次调用完demo后，react会自动调用render重新渲染页面。
- render做的最多的事情就是从状态里面读东西，然后根据状态里面的数据的不同，重新改变页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>setState的使用</title>
</head>
<body>
    <!-- 步骤一：创建一个容器 -->
    <div id="test"></div>

    <!-- 步骤二：引入库 -->
    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <!-- 表示写的是jsx，用babel转换 -->
    <script type="text/babel">
        class Weather extends React.Component{
            // constructor中的this一定是指向实例对象的
            // 实例中要用到state属性，所以必须要写构造函数初始化state属性
            constructor(props){
                    // 只要继承了某个类，并且写了构造函数，就必须要写super()
                    super(props);
                    // state必须是一个对象，或者为null
                    this.state={isHot:true};

                    //构造函数中的this一定是指向实例对象的
                    //调用实例对象中的demo函数的bind方法，将类中的demo函数中的this修改为指向实例对象，再将返回新函数，将它添加到每一个实例对象中。这样每个实例对象中都有一个demo1函数，Weather原型对象中有一个demo函数
                    this.demo1=this.demo.bind(this);
                }

            // render方法中的this指的是类实例对象，因为组件在渲染的时候，自动创建了类实例，并且用类实例调用了render方法
            render(){
                // 表示取this.state这个对象中的isHot属性，那么isHot就代表this.state.isHot
                const {isHot}=this.state;
                // 3.注意：那么这里的this.demo1表示的将对象实例中的demo1方法作为onClick方法的回调，尽管还是以函数方式调用的，但是用bind强制修改了此方法的this为实例对象，所以demo1中的this指向实例对象
                return <h2 onClick={this.demo1}>今天天气真{isHot?'炎热':'寒冷'}</h2>;
            }

            // 将方法写在类里面
            demo() {
                // 1.babel解析，也会开启严格模式，所以由函数形式调用的方法中的this都不允许指向window，都为undefined
                // 2.类中自定义方法中也会默认开启局部严格模式，所以由函数形式调用的方法中的this，就指向undefined。但是如果由实例对象调用的方法中的this，就指向对象实例
                // this.state.isHot=!this.state.isHot; 这样的写法是错误的，因为状态里的数据不能直接更改，要借助内置的api去改：类原型对象的上一级对象中的setState方法，通过this就能拿到
                console.log(this);
                this.setState({isHot:!this.state.isHot});
            }
        }

        // 将组件渲染到页面中
        // Weather类的实例对象是由react内部创建的
        ReactDOM.render(<Weather/>,document.getElementById('test'));
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102001.png)

##### 6.优化代码（箭头函数）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>setState的使用</title>
</head>
<body>
    <!-- 步骤一：创建一个容器 -->
    <div id="test"></div>

    <!-- 步骤二：引入库 -->
    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <!-- 表示写的是jsx，用babel转换 -->
    <script type="text/babel">
        class Weather extends React.Component{
            // 直接用赋值语句，往每一个实例对象中添加属性或者函数，如果属性和函数原本就存在，那么就是修改，不需要再写构造器
            state={isHot:true};

            render(){
                const {isHot}=this.state;
                return <h2 onClick={this.demo}>今天天气真{isHot?'炎热':'寒冷'}</h2>;
            }

			// 直接赋值，表示往所有实例对象中添加一个箭头函数demo
            // 箭头函数本身没有this，如果在箭头函数中，用到了this，那么箭头函数就会去它的外面找this，将外面的this作为自己的this。
            // 由于没办法在类中直接console出this，这里我们就不纠结箭头函数外面：也就是类中的this是什么，只用知道用了箭头函数后，类中自定义方法中的this就指向了实例对象
            // 所以不管以什么方法调用的demo，demo中的this永远指向实例对象
            demo = ()=>{
                this.setState({isHot:!this.state.isHot});
            }
        }

        ReactDOM.render(<Weather/>,document.getElementById('test'));
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102015.png)

##### 7.state总结

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102031.png)

#### 3.9.2 props

props属性存在于每一个类组件的实例对象中，可以**在类外部定义属性及属性值，通过标签属性传参，供类内部调用**

##### 1.props基本使用

- 每一个类组件的实例对象中都有一个props属性
- 组件标签中的所有属性都保存在props中

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>props基本使用</title>
</head>
<body>
    <div id="test1"></div>
    <div id="test2"></div>
    <div id="test3"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class MyComponent extends React.Component{
            render(){
                const {name,age,sex}=this.props;
                return (
                    <ul>
                        <li>姓名：{name}</li>
                        <li>年龄：{age}</li>
                        <li>性别：{sex}</li>
                    </ul>
                )
            }
        }

        // 可以用过标签往组件中的props对象中添加属性和属性值，属性名作为props中的键，属性值作为props的值
        ReactDOM.render(<MyComponent name="小红" age="18" sex="男"/>,document.getElementById("test1"));
        ReactDOM.render(<MyComponent name="小蓝" age="18" sex="女"/>,document.getElementById("test2"));
        ReactDOM.render(<MyComponent name="小绿" age="18" sex="男"/>,document.getElementById("test3"));
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102046.png)

##### 2.批量传递props

- 在babel和引入的React前提下，支持展开运算符...展开一个对象，但仅限于在标签中使用
- 语法：{...对象}，外面的{}仅表示分隔符，而不是表示深度复制

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>props基本使用</title>
</head>
<body>
    <div id="test1"></div>
    <div id="test2"></div>
    <div id="test3"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class MyComponent extends React.Component{
            render(){
                const {name,age,sex}=this.props;
                return (
                    <ul>
                        <li>姓名：{name}</li>
                        <li>年龄：{age}</li>
                        <li>性别：{sex}</li>
                    </ul>
                )
            }
        }

        // 可以用过标签往组件中的props对象中添加属性和属性值，属性名作为props中的键，属性值作为props的值
        ReactDOM.render(<MyComponent name="小红" age="18" sex="男"/>,document.getElementById("test1"));

        // 简写的方法：构造字面量对象时展开语法
        // 用{...对象}，这里的{}只表示分隔符的作用，...对象表示展开一个对象。在js中是不允许...展开一个对象的。但是babel+引入的react允许在react中展开一个对象，但是仅限于标签中展开一个对象，其他地方不能使用...展开一个对象
        const p={name:"小蓝",age:"18",sex:"女"};
        ReactDOM.render(<MyComponent {...p}/>,document.getElementById("test2"));
        
        ReactDOM.render(<MyComponent name="小绿" age="18" sex="男"/>,document.getElementById("test3"));
    </script>
</body>
</html>
```

##### 3.对props属性进行限制

- 只要给**组件自身**加了特定属性，就可以对props属性值进行限制，这里的组件是类式组件，所以需要给类加一些特定的属性

- 要导入专门限制props属性的包
- 要注意区分大小写

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>对props属性进行限制</title>
</head>
<body>
    <div id="test1"></div>
    <div id="test2"></div>
    <div id="test3"></div>
    <div id="test4"></div>
    <div id="test5"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>
    <!-- 导入专门限制props属性的包，因为如果props放在React对象中，那么React对象就会变得很繁重，所以单独对限制props属性创建一个包 -->
    <!-- 引入此包后，全局中就有一个PropTypes对象 -->
    <script type="text/javascript" src="../js/prop-types.js"></script>

    <script type="text/babel">
        class MyComponent extends React.Component{
            render(){
                // 获得props对象中的三个属性
                const {name,age,sex,speak}=this.props;
                return (
                    <ul>
                        <li>姓名：{name}</li>
                        <li>年龄：{age+1}</li>
                        <li>性别：{sex}</li>
                    </ul>
                )
            }
        }

        // 给类中添加propTypes对props属性进行类型、必要性的限制
        MyComponent.propTypes={
            // 这里的PropTypes首字母要大写，string的首字母要小写，为了避免和关键字String冲突
            name:PropTypes.string.isRequired, // 限制为字符串，且必填
            age:PropTypes.number, // 限制为数字
            sex:PropTypes.string, // 限制为字符串
            speak:PropTypes.func  // 由于不能跟function关键字冲突，所以这里的语法规范是func
        }

        // 给类中添加defaultProps对props属性进行默认值的限制
        MyComponent.defaultProps={
            age:18,  // 默认为18
            sex:'男'  // 默认为男
        }

        // 测试正常数据
        const p1={name:'憨憨1',age:19,sex:'女'};
        ReactDOM.render(<MyComponent {...p1}/>,document.getElementById("test1"));

        // 测试不填写年龄、性别
        const p2={name:"憨憨2"};
        ReactDOM.render(<MyComponent {...p2}/>,document.getElementById("test2"));

        // 测试不填写姓名
        const p3={age:19,sex:'女'};
        ReactDOM.render(<MyComponent {...p3}/>,document.getElementById("test3"));

        // 测试将姓名填写成一个数字
        const p4={name:10,age:19,sex:'女'};
        ReactDOM.render(<MyComponent {...p4}/>,document.getElementById("test4"));

        // 测试限制一个属性值必须为函数
        function speakfun() {
            console.log("我在说话");
        }
        const p5={name:"憨憨5",age:80,sex:'男',speak:speakfun};
        ReactDOM.render(<MyComponent {...p5}/>,document.getElementById("test5"));

    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102105.png)

##### 4.props简写

将对类组件中的状态初始化、对象属性的限制都放在类里面去写。而对象属性的值可以在类外面灵活定义

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>props简写</title>
</head>
<body>
    <div id="test1"></div>
    <div id="test2"></div>
    <div id="test3"></div>
    <div id="test4"></div>
    <div id="test5"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>
    <!-- 导入专门限制props属性的包，因为如果props放在React对象中，那么React对象就会变得很繁大，所以单独对限制props属性创建一个包 -->
    <!-- 引入此包后，全局中就有一个PropTypes对象 -->
    <script type="text/javascript" src="../js/prop-types.js"></script>

    <script type="text/babel">
        class MyComponent extends React.Component{
            render(){
                // 获得props对象中的三个属性
                const {name,age,sex,speak}=this.props;
                return (
                    <ul>
                        <li>姓名：{name}</li>
                        <li>年龄：{age+1}</li>
                        <li>性别：{sex}</li>
                    </ul>
                )
            }

            // 给类中添加propTypes对props属性进行类型、必要性的限制
            static propTypes={
                // 这里的PropTypes首字母要大写，string的首字母要小写，为了避免和关键字String冲突
                name:PropTypes.string.isRequired, // 限制为字符串，且必填
                age:PropTypes.number, // 限制为数字
                sex:PropTypes.string, // 限制为字符串
                speak:PropTypes.func  // 由于不能跟function关键字冲突，所以这里的语法规范是func
            }

            // 给类中添加defaultProps对props属性进行默认值的限制
            static defaultProps={
                age:18,  // 默认为18
                sex:'男'  // 默认为男
            }
        }

        // 测试正常数据
        const p1={name:'憨憨1',age:19,sex:'女'};
        ReactDOM.render(<MyComponent {...p1}/>,document.getElementById("test1"));

        // 测试不填写年龄、性别
        const p2={name:"憨憨2"};
        ReactDOM.render(<MyComponent {...p2}/>,document.getElementById("test2"));

        // 测试不填写姓名
        const p3={age:19,sex:'女'};
        ReactDOM.render(<MyComponent {...p3}/>,document.getElementById("test3"));

        // 测试将姓名填写成一个数字
        const p4={name:10,age:19,sex:'女'};
        ReactDOM.render(<MyComponent {...p4}/>,document.getElementById("test4"));

        // 测试限制一个属性值必须为函数
        function speakfun() {
            console.log("我在说话");
        }
        const p5={name:"憨憨5",age:80,sex:'男',speak:speakfun};
        ReactDOM.render(<MyComponent {...p5}/>,document.getElementById("test5"));

    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102120.png)


##### 5，构造函数与props

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102130.png)

如果需要在构造函数中使用this.props，那么一定要在构造函数中接收props并且super(props)，不然this.props很可能会出现undefined

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>构造函数与props</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>
    <!-- 导入专门限制props属性的包，因为如果props放在React对象中，那么React对象就会变得很繁大，所以单独对限制props属性创建一个包 -->
    <!-- 引入此包后，全局中就有一个PropTypes对象 -->
    <script type="text/javascript" src="../js/prop-types.js"></script>

    <script type="text/babel">
        class MyComponent extends React.Component{
            constructor(){
                super();
                console.log(this.props);
            }
            render(){
                // 获得props对象中的三个属性
                const {name,age,sex,speak}=this.props;
                return (
                    <ul>
                        <li>姓名：{name}</li>
                        <li>年龄：{age+1}</li>
                        <li>性别：{sex}</li>
                    </ul>
                )
            }

            // 给类中添加propTypes对props属性进行类型、必要性的限制
            static propTypes={
                name:PropTypes.string.isRequired, // 限制为字符串，且必填
                age:PropTypes.number, // 限制为数字
                sex:PropTypes.string, // 限制为字符串
                speak:PropTypes.func  // 由于不能跟function关键字冲突，所以这里的语法规范是func
            }

            // 给类中添加defaultProps对props属性进行默认值的限制
            static defaultProps={
                age:18,  // 默认为18
                sex:'男'  // 默认为男
            }
        }

        const p1={name:'憨憨1',age:19,sex:'女'};
        ReactDOM.render(<MyComponent {...p1}/>,document.getElementById("test1"));

    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102145.png)

##### 6_函数式组件使用props

- 函数式组件只能使用props，因为函数可以接收参数。不能用state、ref属性

- 将props作为参数传入函数，那么在函数中就可以拿到props的所有属性值
- 如果在函数式组件要对props属性值进行限制，那么直接往组件中添加特定属性就可以

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>函数式组件使用props</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>
    <!-- 导入专门限制props属性的包，因为如果props放在React对象中，那么React对象就会变得很繁大，所以单独对限制props属性创建一个包 -->
    <!-- 引入此包后，全局中就有一个PropTypes对象 -->
    <script type="text/javascript" src="../js/prop-types.js"></script>

    <script type="text/babel">
        function MyComponent(props){
            const {name,age,sex} = props;
            return (
                    <ul>
                        <li>姓名：{name}</li>
                        <li>年龄：{age+1}</li>
                        <li>性别：{sex}</li>
                    </ul>
                )
        }
        
		// 函数式组件对props限制，只能写在函数外侧
        // 给函数组件中添加propTypes对props属性进行类型、必要性的限制
        MyComponent.propTypes={
            name:PropTypes.string.isRequired, // 限制为字符串，且必填
            age:PropTypes.number, // 限制为数字
            sex:PropTypes.string, // 限制为字符串
        }

        // 给类中添加defaultProps对props属性进行默认值的限制
        MyComponent.defaultProps={
            age:18,  // 默认为18
            sex:'男'  // 默认为男
        }
        
        const p1={name:'憨憨1'};
        ReactDOM.render(<MyComponent {...p1}/>,document.getElementById("test1"));

    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102200.png)

##### 7_props总结

- 在组件内部不能修改props属性，因为props是只读的。可以对props的某个属性值进行运算，但是不能修改，比如在组件内部：this.props.age=this.props.age+1； 这样就表示修改。<li>年龄：{age+1}</li>  这样就表示运算。

#### 3.9.3 refs

作用：获取任意标签结点

##### 1.字符串形式的ref

- ref属性值为字符串
- ref相当于标签的标识，每一个标签上的ref都以键值对形式被封装在实例对象中的refs对象，key为ref的属性值，value为当前标签结点
- 不建议使用，因为写多了字符串形式的ref，存在效率问题，而且未来版本可能会删掉字符串形式的ref

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>1_字符串形式的ref</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class MyComponent extends React.Component{
            
            // 直接从实例对象中的refs属性中拿到相应标签所在的结点，直接获取输入框结点的value值
            // 这里取的是类组件创建的虚拟DOM变为真实DOM后的结点，不是直接取的虚拟DOM的结点
            textAlert1=()=>{
                alert(this.refs.text1.value);
            }

            loseBlurAlert=()=>{
                alert(this.refs.text2.value);
            }

            render(){
                return (
                    // ref相当于标签的标识，每一个标签上的ref都以键值对形式被封装在实例对象中的refs对象，key为ref的属性值，value为当前标签结点
                    <div>
                        <input ref="text1" type="text" placeholder="点击按钮提示数据"/>
                        <button onClick={this.textAlert1}>点我提示左侧的数据</button>
                        <input ref="text2" onBlur={this.loseBlurAlert} type="text" placeholder="失去焦点提示数据"/>
                    </div>
                )
            }
        }

        ReactDOM.render(<MyComponent/>,document.getElementById("test1"));

    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102215.png)

##### **2.回调函数形式的ref**

只要render中的标签上用到了回调函数形式的ref那么react会自动帮你调用这个回调函数

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2_回调函数形式的ref</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class MyComponent extends React.Component{
            
            // 直接从实例对象中的refs属性中拿到相应标签所在的结点，直接获取输入框结点的value值
            textAlert1=()=>{
                alert(this.text1.value);
            }

            loseBlurAlert=()=>{
                alert(this.text2.value);
            }

            render(){
                return (
                    // 这里用回调函数ref，会自动将当前标签所在结点作为参数传入回调函数中，这时，我们在实例对象中定义一个属性去接收它，这样直接获取实例对象中的那个属性就相当于获取当前标签所在结点
                    // currentNode名字可以任意取
                    // 箭头函数没有this，去外面找，也就是render中，render中的this是实例对象，所以this.text1=currentNode表示将结点赋值给实例对象中的text1属性
                    // 箭头函数的参数只有一个时，可以省略()，函数体只有一句话时，可以省略{}
                    <div>
                        <input ref={(currentNode) => {this.text1=currentNode}} type="text" placeholder="点击按钮提示数据"/>
                        <button onClick={this.textAlert1}>点我提示左侧的数据</button>
                        <input ref={currentNode => this.text2=currentNode} onBlur={this.loseBlurAlert} type="text" placeholder="失去焦点提示数据"/>
                    </div>
                )
            }
        }

        ReactDOM.render(<MyComponent/>,document.getElementById("test1"));

    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102230.png)

##### 3_回调ref中回调函数调用次数的问题

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102243.png)



- 刚进页面会调一次，因为使用类组件对页面进行渲染，会自动调用实例对象的render方法，render放法中的ref是回调形式的，所以react自动调用一次这个回调函数。
- state状态每改变一次，就会驱动页面改变，就会重新渲染页面。但是回调函数被调用两次，且第一次被调用时，当前结点为null。因为state状态改变，重新调用render，那么内联函数将被当作一个新函数，既然是新函数，就要清空上一次旧函数的值，所以传递一个null参数，然后再将当前结点作为参数传递给函数。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3_回调ref中回调函数调用次数</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class MyComponent extends React.Component{
            
            state={isHot:true};

            textAlert1=()=>{
                alert(this.text1.value);
            }

            changeWeather=()=>{
                this.setState({isHot:!this.state.isHot})
            }

            render(){
                return (
                    <div>
                        <input ref={(currentNode) => {this.text1=currentNode;console.log('@',currentNode);}} type="text" placeholder="点击按钮提示数据"/>
                        <button onClick={this.textAlert1}>点我提示左侧数据</button>
                        <h2 onClick={this.changeWeather}>今天天气很{this.state.isHot?'炎热':'凉爽'}</h2>
                    </div>
                )
            }
        }

        ReactDOM.render(<MyComponent/>,document.getElementById("test1"));

    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102256.png)

解决：将回调函数不直接写在标签上，而是作为一个箭头函数放进实例对象中

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>4_解决回调ref调用次数问题</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class MyComponent extends React.Component{
            
            state={isHot:true};

            textAlert1=()=>{
                alert(this.cNode.value);
            }

            changeWeather=()=>{
                this.setState({isHot:!this.state.isHot})
            }

            getNode=(currentNode)=>{
                this.cNode=currentNode;
                console.log(this.cNode);
            }

            render(){
                return (
                    <div>
                        <input ref={this.getNode} type="text" placeholder="点击按钮提示数据"/>
                        <button onClick={this.textAlert1}>点我提示左侧数据</button>
                        <h2 onClick={this.changeWeather}>今天天气很{this.state.isHot?'炎热':'凉爽'}</h2>
                    </div>
                )
            }
        }

        ReactDOM.render(<MyComponent/>,document.getElementById("test1"));

    </script>
</body>
</html>
```

现在不管怎么改变状态，都不会调用回调函数了，**因为它不是内联函数，每次渲染时，不再是一个新函数需要被清空了**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102311.png)

##### 4.createRef

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>5_createRef</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class MyComponent extends React.Component{
            

            // 调用React.createRef表示生成一个空间，可以用来存储一个标签结点，key为current（固定的），value为结点
            // 局限：只能存储一个结点，要是还想存储，那么就要再调用一次
            myRef=React.createRef();
            myRef2=React.createRef();
            
            // 直接从实例对象中的refs属性中拿到相应标签所在的结点，直接获取输入框结点的value值
            textAlert1=()=>{
                // current是myRef中的结点的key值
                alert(this.myRef.current.value);
            }

            loseBlurAlert=()=>{
                alert(this.myRef2.current.value);
            }

            render(){
                return (
                    // 表示将结点存储在对象属性的myRef中
                    <div>
                        <input ref={this.myRef} type="text" placeholder="点击按钮提示数据"/>
                        <button onClick={this.textAlert1}>点我提示左侧的数据</button>
                        <input ref={this.myRef2} onBlur={this.loseBlurAlert} type="text" placeholder="失去焦点提示数据"/>
                    </div>
                )
            }
        }

        ReactDOM.render(<MyComponent/>,document.getElementById("test1"));

    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102329.png)

##### 5.event.target  省略ref

当为一个结点绑定事件时，该结点事件触发的回调函数的参数，默认就是event

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>6_省略ref</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class MyComponent extends React.Component{
            
            myRef=React.createRef();
    
            textAlert1=()=>{
                alert(this.myRef.current.value);
            }

            loseBlurAlert=(event)=>{
                // 事件中的event.target表示的是事件源对象，即触发事件的结点对象，此例中就是第二个输入框对象
                alert(event.target.value);
            }

            render(){
                return (
                    // 第二个输入框，触发事件的结点和要获取结点的标签是同一个，这时可以用event.target对象获取结点，从而拿到结点上的数据
                    <div>
                        <input ref={this.myRef} type="text" placeholder="点击按钮提示数据"/>
                        <button onClick={this.textAlert1}>点我提示左侧的数据</button>
                        <input ref={this.myRef2} onBlur={this.loseBlurAlert} type="text" placeholder="失去焦点提示数据"/>
                    </div>
                )
            }
        }

        ReactDOM.render(<MyComponent/>,document.getElementById("test1"));

    </script>
</body>
</html>
```

### 3.10 react中的事件绑定

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102345.png)

### 3.11 react中收集表单数据

#### 1.非受控组件

- 表单中的数据现用现取。填完之后，从表单中取出来直接用的，为非受控组件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>1_非受控组件</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class MyComponent extends React.Component{

            handleSubmit=(event)=>{
                // 阻止表单提交,即提交表单后，不进行页面刷新和跳转，做到让用户在本页面中就能处理表单提交的感觉
                event.preventDefault();

                // 获得并使用两个表单项结点的数据
                alert("你的用户名为："+this.usernameNode.value+"；你的密码为："+this.passwordNode.value);
            }

            render(){
                return (
                    // onSubmit：表单提交时触发
                    <form action="" onSubmit={this.handleSubmit}>
                        <input ref={c=>this.usernameNode=c} type="text" name="username"/>
                        <input ref={c=>this.passwordNode=c} type="password" name="password"/>
                        <button>确定</button>
                    </form>
                )
            }
        }

        ReactDOM.render(<MyComponent/>,document.getElementById("test1"));

    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102403.png)

#### 2.受控组件

- 将表单中的数据存到state中，要用的时候，从state中取出来，叫做受控组件（变相的双向绑定）

- 使用react中的onChange事件（改变时触发）实时拿到表单中的数据，并将表单中的数据存到state中，用的时候直接从state中取即可（state建议最好先初始化，再setState）
- 建议使用受控组件，因为react官网建议少用ref

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2_受控组件</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class MyComponent extends React.Component{

            // 初始化state
            state={
                username:'',
                password:''
            }

            // 用户名输入框值发生改变时触发，event.target表示事件源对象
            saveName=(event)=>{
                this.setState({username:event.target.value});
            }

            // 用户名输入框值发生改变时触发，event.target表示事件源对象
            savePass=(event)=>{
                this.setState({password:event.target.value});
            }

            // 表单提交的回调
            handleSubmit=(event)=>{
                // 阻止表单提交,即提交表单后，不进行页面刷新和跳转，做到让用户在本页面中就能处理表单提交的感觉
                event.preventDefault();

                alert("你的用户名为："+this.state.username+";你的密码为："+this.state.password);
            }

            render(){
                return (
                    // onSubmit：表单提交时触发
                    <form action="" onSubmit={this.handleSubmit}>
                        <input onChange={this.saveName} type="text" name="username"/>
                        <input onChange={this.savePass} type="password" name="password"/>
                        <button>确定</button>
                    </form>
                )
            }
        }

        ReactDOM.render(<MyComponent/>,document.getElementById("test1"));

    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102425.png)

### 3.12 高阶函数_函数柯里化

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102436.png)

#### 1.用函数柯里化

- 如果将一个**函数调用后的返回值（函数的执行结果）**作为某个**事件触发的回调函数**。那么在一进页面，这个函数就被执行一遍，得到一个返回值，将返回值作为事件触发的回调，那么后面无论怎么触发事件，这个函数都不会再被调用，因为**事件的回调**是这个**函数的返回值**

- 如果不仅将一个函数调用后的返回值作为事件回调，还给这函数传了参，那么这个函数的默认参数就不再是event事件源，而是你自己传的参数

- 函数调用后的返回值如果是一个回调函数，那么表示：一进页面，react就调用这个函数，得到返回值，将返回值作为事件触发的回调，这个返回值又恰好是一个回调函数，所以事件的回调还是一个回调函数。那么每次触发事件时，react都会调用那个返回值回调函数（但是不会再调用之前那个函数了）

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>2_受控组件</title>
  </head>
  <body>
      <div id="test1"></div>
  
      <script type="text/javascript" src="../js/react.development.js"></script>
      <script type="text/javascript" src="../js/react-dom.development.js"></script>
      <script type="text/javascript" src="../js/babel.min.js"></script>
  
      <script type="text/babel">
          class MyComponent extends React.Component{
  
              // 初始化state
              state={
                  username:'',
                  password:''
              }
  
              // 绑定了事件的表单项的值发生改变时触发，参数为你自己传的参数（在一进页面，react就发现onChange事件的回调是一个函数的调用，那么react就直接先调用了这个函数，并且后面再触发onChange事件后，这个函数都不会再被调用。）
              saveForm=(formType)=>{
                  // 该事件函数的返回值为一个回调函数，将这个回调函数作为onChange的触发事件，那么在每次触发事件时，react都会调用这个回调函数。这个回调函数被传参数，所以参数默认为event
                  return (event)=>{
                      // 在对象中读取一个变量，需要将变量用[]括起来
                      this.setState({[formType]:event.target.value});
                  };
              }
  
              // 表单提交的回调
              handleSubmit=(event)=>{
                  
                  event.preventDefault();
  
                  alert("你的用户名为："+this.state.username+";你的密码为："+this.state.password);
              }
  
              render(){
                  return (
                      // 事件的回调应该传的是一个函数，每触发一次事件，函数就被react调用一次。但是这里是将一个函数的执行结果的返回值作为onChange的回调，那么结果是：在一进页面，react就发现onChange事件的回调是一个函数的调用，那么react就直接先调用了这个函数，并且后面再触发onChange事件后，这个函数都不会再被调用。这函数还传了实参，那么箭头函数默认的参数不再是事件源event，而是你自己传的那个参数
                      // 将这个函数调用的返回值设置为一个回调函数，在返回值回调函数中，写在触发onChange后你真正想做的事，返回值回调函数没有传参，所以参数默认就是event了，即事件源对象。每次触发事件，都会调用这个回调函数
                      <form action="" onSubmit={this.handleSubmit}>
                          <input onChange={this.saveForm('username')} type="text" name="username"/>
                          <input onChange={this.saveForm('password')} type="password" name="password"/>
                          <button>确定</button>
                      </form>
                  )
              }
          }
  
          ReactDOM.render(<MyComponent/>,document.getElementById("test1"));
  
      </script>
  </body>
  </html>
  ```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102515.png)

#### 2.不用函数柯里化

统一传参：

将箭头函数直接以内联的方式作为事件触发的回调函数，将事件源作为箭头函数的参数直接传入箭头函数中。在箭头函数中调用另一个箭头函数，并将得到的事件源event作为参数传入。那么另一个箭头函数也能拿到事件源对象了

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2_不用函数柯里化</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class MyComponent extends React.Component{

            // 初始化state
            state={
                username:'',
                password:''
            }

            // 绑定了事件的表单项的值发生改变时触发，参数为你自己传的参数
            // 此时的event，为事件触发回调时，拿到的事件源，再将event作为参数传到这里，所以这里的event也为事件源
            saveForm=(formType,event)=>{
                this.setState({[formType]:event.target.value});
            }

            // 表单提交的回调
            handleSubmit=(event)=>{
                
                event.preventDefault();

                alert("你的用户名为："+this.state.username+";你的密码为："+this.state.password);
            }

            render(){
                return (
                    // 直接将一个回调函数以内联的方式作为事件触发的回调，那么在每次触发事件时，都会执行这个回调函数，此回调函数的参数默认为当前事件源。
                    // 在这个回调函数体里是一另一个回调函数。那么事件每次触发时，react先会调用外侧的回调函数，外侧的回调函数被调用的时候，就执行函数体里面的回调函数，达到表单值一改变时，就将当前表单值存入state状态中
                    <form action="" onSubmit={this.handleSubmit}>
                        <input onChange={event => this.saveForm('username',event)} type="text" name="username"/>
                        <input onChange={event => this.saveForm('password',event) } type="password" name="password"/>
                        <button>确定</button>
                    </form>
                )
            }
        }

        ReactDOM.render(<MyComponent/>,document.getElementById("test1"));

    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102541.png)

### 3.13 组件的生命周期（旧）

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102601.png)

#### 1.引出生命周期(错误示范)

做一个效果：1.点击按钮，页面中的组件就消失 2.一进页面（即组件刚挂载完后）：页面中的第一行文字，不透明度逐渐减小，等到透明时，自动变为1，再继续减小

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>1_引出生命周期</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class MyComponent extends React.Component{

            // 初始化state
            state={
                // 最开始不透明度为1，后来慢慢减少
                opacity: 1
            }

            // ReactDOM中的unmountComponentAtNode方法表示卸载，将一个组件从页面中卸载
            dead = () => {
                ReactDOM.unmountComponentAtNode(document.getElementById('test1'));
            }

            render(){
                // 由于类中不能写函数，所以将函数放在render函数里面
                // 这样带来的问题是：第一次页面渲染时，react创建类的实例对象并调用render方法时，调用了这个setInterval定时器，但是在定时器中，修改了state状态，只要状态改变，render就会被重新调用，那么随着render的重新调用，又开了一个定时器。以此类推，无限循环，开启了无数个定时器
                setInterval(()=>{
                    // 获得当前不透明度。这里要用let定义op变量，因为const定义的话，是不能修改变量值的
                    let op=this.state.opacity;
                    op -= 0.1;

                    // 将当前状态中的不透明度减去0.1
                    this.setState({opacity:op});

                    // 如果不透明度小于或等于0了（完全透明了），就将他重新设置为1
                    if(op <= 0){
                        this.setState({opacity:1});
                    }
                },200);
                return (
                    // 注意内联样式的写法：以key，value的形式写属性
                    <div>
                        <h2 style={{opacity:this.state.opacity}}>开始变得透明</h2>
                        <button onClick={this.dead}>点我全部消失</button>
                    </div>
                )
            }
        }

        ReactDOM.render(<MyComponent/>,document.getElementById("test1"));

    </script>
</body>
</html>
```

结果是：因为开了很多个定时器，导致越来越快。一直在新开定时器

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102619.png)

#### 2.引出生命周期

- componentDidMount做到在组件挂载完毕后：页面中的那行文字就开始变透明，一直循环变。
- 一点击按钮，页面内的组件就消失
- componentWillUnmount在组件消失之前，关闭定时器。因为如果在组件卸载后，定时器还开着话，定时器内又一直在改变state，就会报错。Can't perform a React state update on an unmounted component.（在一个卸载的组件中改变状态）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2_引出生命周期</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class MyComponent extends React.Component{

            // 初始化state
            state={
                // 最开始不透明度为1，后来慢慢减少
                opacity: 1
            }

            // ReactDOM中的unmountComponentAtNode方法表示卸载，将一个组件从页面中卸载
            dead = () => {
                ReactDOM.unmountComponentAtNode(document.getElementById('test1'));
            }

            // 调用时机：组件卸载之前
            componentWillUnmount(){
                // 在卸载组件前，清除定时器，this.timer为定时器的id
                clearInterval(this.timer);
            }

            // 调用时机：组件挂载完毕后，即第一次渲染页面完毕后执行，且只执行这一次
            componentDidMount(){
                // 将定时器放在componentDidMount方法中，页面挂载完毕后，调用一次。那么做到了只开启一个定时器
                this.timer = setInterval(()=>{
                    // 获得当前不透明度。这里要用let定义op变量，因为const定义的话，是不能修改变量值的
                    let op=this.state.opacity;
                    op -= 0.1;

                    // 将当前状态中的不透明度减去0.1
                    this.setState({opacity:op});

                    // 如果不透明度小于或等于0了（完全透明了），就将他重新设置为1
                    if(op <= 0){
                        this.setState({opacity:1});
                    }
                },200);
            }

            // 调用时机：1.页面第一次渲染 2.状态改变
            render(){
                return (
                    // 注意内联样式的写法：以key，value的形式写属性
                    <div>
                        <h2 style={{opacity:this.state.opacity}}>开始变得透明</h2>
                        <button onClick={this.dead}>点我全部消失</button>
                    </div>
                )
            }
        }

        ReactDOM.render(<MyComponent/>,document.getElementById("test1"));

    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102634.png)

#### 3.组件挂载流程

挂载时：

1. construstor：构造器

2. componentWillMount：组件将要挂载

3. render：组件挂载

4. componentDidMount：组件挂载完毕

   ---------------------------------------------------

5. componentWillUnmount：组件卸载前（unmountComponentByNode(组件) 卸载组件的方法）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3_组件挂载流程</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class Count extends React.Component{

            // 由于要展示生命周期各个时间段，所以这里写构造器
            // 第一阶段：Count----构造器
            constructor(props){
                console.log("构造器");
                super(props);
                this.state={count:0};
            }

            // 卸载组件按钮的回调
            dead = () => {
                // 卸载组件
                ReactDOM.unmountComponentAtNode(document.getElementById('test1'));
            }

            // 第二阶段：Count----组件将要挂载
            componentWillMount(){
                console.log("组件将要挂载");
            }

            // 第四阶段：Count----组件刚挂载完毕
            componentDidMount(){
                console.log("组件刚挂载完毕");
            }

            // 第五阶段：Count----组件卸载之前
            componentWillUnmount(){
                console.log("组件卸载之前");
            }

            // 第三阶段：Count----组件挂载
            render(){
                console.log("组件挂载");
                return (
                    <div>
                        <button onClick={this.dead}>卸载组件</button>  
                    </div>
                )
            }
        }

        // 渲染组件
        ReactDOM.render(<Count/>,document.getElementById('test1'));
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102647.png)

#### 4.组件正常更新流程

更新时：

1. 更改状态时（setState）：

   - shouldComponentUpdate：组件是否能被更新。

     返回值必须是bool值

     如果直接不写这个函数，那么底层也会默认写出来，且默认为true。

     为true才能往下走下去。相当于控制组件是否能更新的阀门

   - componentWillUpdate：组件更新前

   - render：组件更新

   - componentDidUpdate：组件更新完毕

     ----------------------------------------------------

   - componentWillUnmount：组件卸载前

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>4_组件正常更新流程</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class Count extends React.Component{

            // 由于要展示生命周期各个时间段，所以这里写构造器
            // 第一阶段：Count----构造器
            constructor(props){
                console.log("构造器");
                super(props);
                this.state={count:0};
            }

            // 卸载组件按钮的回调
            dead = () => {
                // 卸载组件
                ReactDOM.unmountComponentAtNode(document.getElementById('test1'));
            }

            // +1按钮的回调
            // 更改state
            add = () => {
                this.setState({count:this.state.count+1});
            }

            // 第一阶段：组件是否能更新的阀门
            shouldComponentUpdate(){
                // 返回true才能更新组件
                console.log("组件是否能更新？");
                return true;
            }

            // 第二阶段：组件更新前
            componentWillUpdate(){
                console.log("组件更新前");
            }

            // 第四阶段：组件更新完毕
            componentDidUpdate(){
                console.log("组件更新完毕");
            }

            // 第五阶段：Count----组件卸载之前
            componentWillUnmount(){
                console.log("组件卸载之前");
            }

            // 第三阶段：Count----组件挂载/组件更新
            render(){
                console.log("组件挂载/更新");
                return (
                    <div>
                        <h2>当前数字为：{this.state.count}</h2>
                        <button onClick={this.add}>点我+1</button>
                        <button onClick={this.dead}>卸载组件</button>  
                    </div>
                )
            }
        }

        // 渲染组件
        ReactDOM.render(<Count/>,document.getElementById('test1'));
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102659.png)


#### 5.组件强制更新流程

1. 强制更新（ this.forceUpdate() ）：

   没有更改状态，组件也能更新。

   强制更新不受shouldComponentUpdate阀门的限制，直接往下走

   - componentWillUpdate：组件更新前
   - render：组件更新
   - componentDidUpdate：组件更新完毕

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>5_组件强制更新流程</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class Count extends React.Component{

            force = () =>{
                // this.forceUpdate()方法：不修改状态，也能更新组件
                this.forceUpdate();
            }

            // 第一阶段：组件是否能更新的阀门
            shouldComponentUpdate(){
                // 返回true才能更新组件
                console.log("组件是否能更新？");
                return true;
            }

            // 第二阶段：组件更新前
            componentWillUpdate(){
                console.log("组件更新前");
            }

            // 第四阶段：组件更新完毕
            componentDidUpdate(){
                console.log("组件更新完毕");
            }

            // 第三阶段：Count----组件挂载/组件更新
            render(){
                console.log("组件挂载/更新");
                return (
                    <div>
                        <button onClick={this.force}>不修改状态，强制更新组件</button>  
                    </div>
                )
            }
        }

        // 渲染组件
        ReactDOM.render(<Count/>,document.getElementById('test1'));
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102746.png)

#### 6.父组件render流程

一般是将父组件state对象中的属性，传给子组件的props

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>6_父组件render</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
        class Father extends React.Component{

            state={car:'奥迪'}

            changeCar = () =>{
                this.setState({car:'奔驰'})
            }

            render(){
                return (
                    // 将state对象中的属性，传给子组件的props
                    <div>
                        <h2>我是Father组件</h2>
                        <button onClick={this.changeCar}>点我换车</button>
                        <Son car={this.state.car}/>
                    </div>
                )
            }
        }

        class Son extends React.Component{

            // 第一阶段：组件将要接收到新的props参数钩子，此钩子可以接收参数props。但是此钩子有一个坑：第一次接收的不算，即第一次接收props参数，此函数不调用，必须得接收新的props参数才开始调用。
            componentWillReceiveProps(props){
                console.log("组件将要接收到props参数");
                console.log(props);
            }

            // 第二阶段：组件是否能更新的阀门
            shouldComponentUpdate(){
                // 返回true才能更新组件
                console.log("组件是否能更新？");
                return true;
            }

            // 第三阶段：组件更新前
            componentWillUpdate(){
                console.log("组件更新前");
            }

            // 第五阶段：组件更新完毕
            componentDidUpdate(){
                console.log("组件更新完毕");
            }

            // 第四阶段：渲染组件/更新组件
            render(){
                console.log("渲染/更新");
                // 接收到的car参数封装到了this.props对象中，直接拿取
                return (
                    <div>
                        <h2>我是Son组件,我接收到的车是：{this.props.car}</h2>
                    </div>
                )
            }
        }

        // 渲染组件
        ReactDOM.render(<Father/>,document.getElementById('test1'));
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102801.png)

#### 7.总结

常用：

render(){}

componentDIdMount(){}

- 初始化的事情
- 开启定时器
- 发送网络请求
- 订阅消息

componentWillUnmount(){}

- 收尾的事情
- 关闭定时器
- 取消订阅

### 3.14 组件的生命周期（新）

新的生命周期中：componentWillMount、componentWillReceiveProps、componentWillUpdate使用时都需要加上前缀UNSAFE_。

因为这三个钩子函数被人乱用，可能在React的未来版本造成不安全的隐患。所以要使用，必须加上前缀才能使用，目前的版本中如果不加前缀，会有警告，未来的React版本，可能会直接报废弃的错，不能不加前缀使用

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102813.png)



![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102836.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>1_新旧区别</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/新版本/react.development.js"></script>
    <script type="text/javascript" src="../js/新版本/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/新版本/babel.min.js"></script>

    <script type="text/babel">
        class Count extends React.Component{

            constructor(props){
                console.log("构造器");
                super(props);
                this.state={count:0};
            }

            dead = () => {
                ReactDOM.unmountComponentAtNode(document.getElementById('test1'));
            }

            add = () => {
                this.setState({count:this.state.count+1});
            }

            force = () =>{
                this.forceUpdate();
            }

            UNSAFE_componentWillMount(){
                console.log("Count————组件将要挂载");
            }

            componentDidMount(){
                console.log("Count————组件刚挂载完毕");
            }

            componentWillUnmount(){
                console.log("Count————组件卸载之前");
            }

            // 第一阶段：组件是否能更新的阀门
            shouldComponentUpdate(){
                // 返回true才能更新组件
                console.log("Count————组件是否能更新？");
                return true;
            }

            // 第二阶段：组件更新前
            UNSAFE_componentWillUpdate(){
                console.log("Count————组件更新前");
            }

            // 第四阶段：组件更新完毕
            componentDidUpdate(){
                console.log("Count————组件更新完毕");
            }

            render(){
                console.log("Count————组件挂载/更新");
                return (
                    <div>
                        <button onClick={this.dead}>卸载组件</button>
                        <button onClick={this.add}>点我+1</button>
                        <button onClick={this.force}>不修改状态，强制更新组件</button>  
                    </div>
                )
            }
        }

        class Father extends React.Component{

            state={car:'奥迪'}

            changeCar = () =>{
                this.setState({car:'奔驰'})
            }

            render(){
                return (
                    <div>
                        <h2>我是Father组件</h2>
                        <button onClick={this.changeCar}>点我换车</button>
                        <Son car={this.state.car}/>
                    </div>
                )
            }
        }

        class Son extends React.Component{

            // 第一阶段：组件将要接收到props参数钩子，此钩子可以接收参数props。但是此钩子有一个坑：第一次接收的不算，即第一次接收props参数，此函数不调用，必须得接收新的props参数才开始调用。
            UNSAFE_componentWillReceiveProps(props){
                console.log("组件将要接收到props参数");
                console.log(props);
            }

            // 第二阶段：组件是否能更新的阀门
            shouldComponentUpdate(){
                // 返回true才能更新组件
                console.log("组件是否能更新？");
                return true;
            }

            // 第三阶段：组件更新前
            UNSAFE_componentWillUpdate(){
                console.log("组件更新前");
            }

            // 第五阶段：组件更新完毕
            componentDidUpdate(){
                console.log("组件更新完毕");
            }

            // 第四阶段：渲染组件/更新组件
            render(){
                console.log("渲染/更新");
                // 接收到的car参数封装到了this.props对象中，直接拿取
                return (
                    <div>
                        <h2>我是Son组件,我接收到的车是：{this.props.car}</h2>
                    </div>
                )
            }
        }

        ReactDOM.render(<Count/>,document.getElementById('test1'));
    </script>
</body>
</html>
```

就不会报错

#### 1.getDerivedStateFromProps

从props中得到衍生状态。意思是：

- 首先该函数可以接收props参数，也能接收state参数，但是这个state参数是指当前state
- 返回值只能是null和状态对象
  - 如果该函数的返回参数是null，那么不影响state
  - 如果函数返回值是一个状态对象，即跟状态中的属性有关的对象，比如{name:'憨憨'}，或者直接返回props对象，那么相当于将返回的这个对象作为state的初始值，并且不能通过setState修改状态了，**因为setState和forceState也会触发getDerivedStateFromProps方法，所以用过setState修改state后，又触发getDerivedStateFromProps，那么就又将state值更新为props了，所以本质上用setState修改相当于没改。**此后的state改变完全取决于返回的这个对象。
- getDerivedStateFromProps必须为一个静态函数

- 应用场景：state的值完全取决于props时，即接收props参数，并且返回值也为props时

比如：之前的点击按钮+1，是通过setState实现的，但是如果调用了这个函数，且返回值不为null

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2_getDerivedStateFromProps</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/新版本/react.development.js"></script>
    <script type="text/javascript" src="../js/新版本/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/新版本/babel.min.js"></script>

    <script type="text/babel">
        class Count extends React.Component{

            constructor(props){
                console.log("constructor————构造器");
                super(props);
                this.state={count:0};
            }

            add = () => {
                this.setState({count:this.state.count+1});
            }

            // 记得getDerivedStateFromProps必须为一个静态函数
            static getDerivedStateFromProps(props,state){
                // 接收props
                console.log(props);
                // 这里是当前state
                console.log(state);
                console.log("getDerivedStateFromProps————从props中得到衍生状态");
                // 将返回值作为一个衍生的state，替换掉在这之前的state。并且后面再用setState不能修改state了，因为setState和forceState也会触发getDerivedStateFromProps方法，所以用过setState修改state后，又触发getDerivedStateFromProps，那么就又将state值更新为props了，所以本质上用setState修改相当于没改。state的改变完全跟props绑定在一起了。
                // 当然这里也可以不返回props，可以返回一个与状态有关的对象即可{count:100}。
                return props;
            }

            componentDidMount(){
                console.log("componentDidMount————组件刚挂载完毕");
            }

            componentWillUnmount(){
                console.log("Count————组件卸载之前");
            }

            shouldComponentUpdate(){
                console.log("Count————组件是否能更新？");
                return true;
            }

            componentDidUpdate(){
                console.log("Count————组件更新完毕");
            }

            render(){
                console.log("Count————组件挂载/更新");
                return (
                    <div>
                        <h2>当前数字为：{this.state.count}</h2>
                        <button onClick={this.add}>点我+1</button>
                    </div>
                )
            }
        }

        // 向props中传递count:100
        ReactDOM.render(<Count count={100}/>,document.getElementById('test1'));
    </script>
</body>
</html>
```

#### 2.getSnapshotBeforeUpdate

组件更新（render）到组件更新完毕（componentDidUpdate）之间，能获取一些**组件更改前的信息**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102858.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3_getSnapshotBeforeUpdate</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/新版本/react.development.js"></script>
    <script type="text/javascript" src="../js/新版本/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/新版本/babel.min.js"></script>

    <script type="text/babel">
        class Count extends React.Component{

            constructor(props){
                console.log("constructor————构造器");
                super(props);
                this.state={count:0};
            }

            add = () => {
                this.setState({count:this.state.count+1});
            }

            static getDerivedStateFromProps(props,state){
                console.log(props);
                // 这里是当前state
                console.log(state);
                console.log("getDerivedStateFromProps————从props中得到衍生状态");
                return null;
            }

            componentDidMount(){
                console.log("componentDidMount————组件刚挂载完毕");
            }

            shouldComponentUpdate(){
                console.log("shouldComponentUpdate————组件是否能更新？");
                return true;
            }

            // 组件更新render到组件更新完毕componentDidUpdate之间。作用：在更新之前，获取快照，返回值可以传给componentDidUpdate
            getSnapshotBeforeUpdate(){
                console.log("getSnapshotBeforeUpdate————组件更新到渲染组件到页面之间");
                return '快照值';
            }

            // 能接收两个参数，在这一次组件更新（调用setState或者使用getDerivedStateFromProps更新状态）前的props，这一次组件更新前的state
            componentDidUpdate(preProps,preState,snapshot){
                console.log(preProps);
                console.log(preState);
                console.log(snapshot);
                console.log("componentDidUpdate————组件更新完毕");
            }

            componentWillUnmount(){
                console.log("componentWillUnmount————组件卸载之前");
            }

            render(){
                console.log("render————组件挂载/更新");
                return (
                    <div>
                        <h2>当前数字为：{this.state.count}</h2>
                        <button onClick={this.add}>点我+1</button>
                    </div>
                )
            }
        }

        // 向props中传递count:100
        ReactDOM.render(<Count count={100}/>,document.getElementById('test1'));
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102912.png)

#### 3.getSnapshotBeforeUpdate场景

实时新增新闻，并保持滚动条位置不随新闻数的增加而改变

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>4_getSnapshotBeforeUpdate使用场景</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/新版本/react.development.js"></script>
    <script type="text/javascript" src="../js/新版本/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/新版本/babel.min.js"></script>

    <script type="text/babel">
        class Count extends React.Component{

            
            state={newsArr:[]};

            static getDerivedStateFromProps(props,state){
                console.log("getDerivedStateFromProps————从props中得到衍生状态");
                return null;
            }

            componentDidMount(){
                setInterval(()=>{
                    // 每200毫秒创建一个新闻，并且不丢弃之前的新闻，将新创建的和之前的封装成一个数组设置为newsArr的值
                    const news=['新闻'+(this.state.newsArr.length+1),...this.state.newsArr];
                    this.setState({newsArr:news});
                },200);
                console.log("componentDidMount————组件刚挂载完毕");
            }

            shouldComponentUpdate(){
                console.log("shouldComponentUpdate————组件是否能更新？");
                return true;
            }

            // 组件更新render到组件更新完毕componentDidUpdate之间。作用：在更新之前，获取快照，返回值可以传给componentDidUpdate
            getSnapshotBeforeUpdate(){
                console.log("getSnapshotBeforeUpdate————组件更新到渲染组件到页面之间");
                // 在这一次组件更新渲染之前，能获取到渲染完毕前的scrollHeight，将之传给componentDidUpdate
                return this.refs.list.scrollHeight;
            }

            // 能接收两个参数，在这一次组件更新（调用setState或者使用getDerivedStateFromProps更新状态）前的props，这一次组件更新前的state
            componentDidUpdate(preProps,preState,scrollHeight){
                // this.refs.list.scrollHeight-scrollHeight表示每次更新后，子元素增加的height
                // 想让滚动条一直保持在某个地方，就必须将scrollTop设置为当前scrollTop+新增的height，就可以保持滚动条位置不变
                this.refs.list.scrollTop+=this.refs.list.scrollHeight-scrollHeight;
                console.log("componentDidUpdate————组件更新完毕");
            }

            render(){
                console.log("render————组件挂载/更新");
                return (
                    <div className='list' ref='list'>
                        {
                            // 遍历渲染出来的节点，都需要指定key值
                            this.state.newsArr.map((item,index)=>{
                                return <div className='news' key={index}>{item}</div>
                            })
                        }
                    </div>
                )
            }
        }

        ReactDOM.render(<Count/>,document.getElementById('test1'));
    </script>

    <style>
        .list{height:150px; width:100px; background:rgb(226, 123, 123); overflow: auto;};
        .list .news{
            height:30px;
        }
    </style>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102926.png)

### 4.总结

常用：跟旧版本的一样

render(){}

componentDIdMount(){}

- 初始化的事情
- 开启定时器
- 发送网络请求
- 订阅消息

componentWillUnmount(){}

- 收尾的事情
- 关闭定时器
- 取消订阅

### 3.15 Dom的ditting算法

#### 1.引出ditting

页面开始：

        1.生成虚拟DOM
        2.赋给真实DOM，渲染到页面
        3.更新状态
        4.生成新的虚拟DOM
        5.将新的虚拟DOM中节点的key与旧的虚拟DOM中节点的key比较：
            1.key不同：将新虚拟DOM中的节点添加到真实DOM中
            2.key相同：
                (1)如果新虚拟DOM中的内容为文本，则直接进行替换
                (2)如果相同的key对应的节点内容也相同，那么就直接采用真实DOM中的这个节点
                (3)如果相同的key对应的节点的内容不同，那么就将新的虚拟DOM中的这个节点覆盖真实DOM中的这个节点
                (4)如果相同的key中存在子节点，那么子节点则继续进行如上规则的判断

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>1_引出ditting</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/新版本/react.development.js"></script>
    <script type="text/javascript" src="../js/新版本/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/新版本/babel.min.js"></script>

    <!-- 
        页面开始：
        1.生成虚拟DOM
        2.赋给真实DOM，渲染到页面
        3.更新状态
        4.生成新的虚拟DOM
        5.将新的虚拟DOM中节点的key与旧的虚拟DOM中节点的key比较：
            1.key不同：将新虚拟DOM中的节点添加到真实DOM中
            2.key相同：
                (1)如果新虚拟DOM中的内容为文本，则直接进行替换
                (2)如果相同的key对应的节点内容也相同，那么就直接采用真实DOM中的这个节点
                (3)如果相同的key对应的节点的内容不同，那么就将新的虚拟DOM中的这个节点覆盖真实DOM中的这个节点
                (4)如果相同的key中存在子节点，那么子节点则继续进行如上规则的判断
     -->
    <script type="text/babel">
        class Showdate extends React.Component{
            state={currentDate:new Date().toLocaleString()};

            componentDidMount(){
                // 每一秒钟，更新状态为当前时间
                setInterval(() => {
                this.setState({currentDate:new Date().toLocaleString()});
            }, 1000);
            }

            render(){
                return (
                    // 状态的改变，会重新生成一个新的虚拟DOM，这时再将新的虚拟DOM中节点的key与旧的虚拟DOM中节点的key作比较。
                    // 1.key相同：（1）如果相同的key对应的节点内容也相同，那么就直接采用真实DOM中的这个节点 (2)如果相同的key对应的节点的内容不用，那么就将新的虚拟DOM中的这个节点渲染到真实DOM中
                    // 2.key不同：直接将新的虚拟DOM中的这个节点渲染到真实DOM中

                    // 下面外层中的input节点和h1节点，包括span中的input节点，都是直接采用的真实DOM中的，因为每一次状态更新都没影响到这两个节点的内容，所以新旧虚拟DOM中，这两个节点key值相同，且内容相同，不用重新渲染到真实DOM，直接采用真实DOM中的这两个节点
                    // 而span节点中除了input子节点之外的文本节点部分，就直接用新虚拟DOM覆盖真实DOM，{this.state.currentDate}由于内容不一样，所以也是每次都更新真实DOM
                    <div>
                        <input type="text"/>
                        <h1>哈哈哈</h1>
                        <span>
                            当前时间为：{this.state.currentDate}
                            <input type="text"/>
                        </span>
                    </div>
                )
            }
        }

        ReactDOM.render(<Showdate/>,document.getElementById('test1'));
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102939.png)

#### 2.key的作用(1)

页面开始：

        1.生成虚拟DOM
        2.赋给真实DOM，渲染到页面
        3.更新状态
        4.生成新的虚拟DOM
        5.将新的虚拟DOM中节点的key与旧的虚拟DOM中节点的key比较：
            1.key不同：先将新虚拟DOM中的节点添加到真实DOM中，再将旧虚拟DOM中存在，但新虚拟DOM中不存在的节点删除
            2.key相同：
                (1)如果新虚拟DOM中的内容为文本，则直接进行替换
                (2)如果相同的key对应的节点内容也相同，那么就直接采用真实DOM中的这个节点
                (3)如果相同的key对应的节点的内容不同，那么就将新的虚拟DOM中的这个节点覆盖真实DOM中的这个节点
                (4)如果相同的key中存在子节点，那么子节点则继续进行如上规则的判断

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2_key的作用</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/新版本/react.development.js"></script>
    <script type="text/javascript" src="../js/新版本/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/新版本/babel.min.js"></script>

    <!-- 
        页面开始：
        1.生成虚拟DOM
        2.赋给真实DOM，渲染到页面
        3.更新状态
        4.生成新的虚拟DOM
        5.将新的虚拟DOM中节点的key与旧的虚拟DOM中节点的key比较：
            1.key不同：先将新虚拟DOM中的节点添加到真实DOM中，再将旧虚拟DOM中存在，但新虚拟DOM中不存在的节点删除
            2.key相同：
                (1)如果新虚拟DOM中的内容为文本，则直接进行替换
                (2)如果相同的key对应的节点内容也相同，那么就直接采用真实DOM中的这个节点
                (3)如果相同的key对应的节点的内容不同，那么就将新的虚拟DOM中的这个节点覆盖真实DOM中的这个节点
                (4)如果相同的key中存在子节点，那么子节点则继续进行如上规则的判断
     -->

     <!-- 
        页面开始：如果以index下标作为key的话
        1.生成虚拟DOM
            <li key={0}>张三---------18</li>
            <li key={1}>李四---------19</li>
        2.赋给真实DOM，渲染到页面
        3.点击按钮更新状态
        4.生成新的虚拟DOM
            <li key={0}>王五---------20</li>
            <li key={1}>张三---------18</li>
            <li key={2}>李四---------19</li>
        5.比较新虚拟DOM和旧虚拟DOM
            key为0：旧虚拟DOM：张三---------18，新虚拟DOM：王五---------20。内容不一样，所以将新虚拟DOM的 王五---------20 添加进真实DOM并渲染到页面
            key为1：旧虚拟DOM：李四---------19，新虚拟DOM：张三---------18。内容不一样，所以将新虚拟DOM的 张三---------18 添加进真实DOM并渲染到页面
            key为2：旧虚拟DOM中无，所以直接将新虚拟DOM的 李四---------19 添加进真实DOM并渲染到页面
        
        发现问题：张三---------18、李四---------19 在之前已经添加进真实DOM了，却因为key的值为index下标，由于在数组中的顺序改变了，导致index下标也
                 发生大变化，所以影响到key发生变化，导致 张三---------18 、李四---------19 因为key值与之前不用，被当作内容不一样的节点，又重新再一次添加进真实DOM
      -->

      <!-- 
        页面开始：如果以id(每条数据的唯一标识)作为key的话
        1.生成虚拟DOM
            <li key={1}>张三---------18</li>
            <li key={2}>李四---------19</li>
        2.赋给真实DOM，渲染到页面
        3.点击按钮更新状态
        4.生成新的虚拟DOM
            <li key={3}>王五---------20</li>
            <li key={1}>张三---------18</li>
            <li key={2}>李四---------19</li>
        5.比较新虚拟DOM和旧虚拟DOM
            key为1：旧虚拟DOM：张三---------18，新虚拟DOM：张三---------18。内容一样，所以直接采用真实DOM中的这个节点
            key为2：旧虚拟DOM：李四---------19，新虚拟DOM：李四---------19。内容一样，所以直接采用真实DOM中的这个节点
            key为3：旧虚拟DOM中无，所以直接将新虚拟DOM的王五---------20添加进真实DOM并渲染到页面
        
        效率比原本用index作为key高的多，之前已经添加进真实DOM中的节点不用再重复添加
      -->
    <script type="text/babel">
        class Showperson extends React.Component{

            state={person:[{id:1,name:'张三',age:18,sex:'男'},{id:2,name:'李四',age:19,sex:'女'}]};

            addPerson=()=>{
                const person3={id:this.state.person.length+1,name:'王五',age:20,sex:'男'};
                this.setState({person:[person3,...this.state.person]});
            }

            render(){
                return (
                    <div>
                        <button onClick={this.addPerson}>点击新增人</button>
                        <h2>使用index（索引值）作为key</h2>
                        {
                            this.state.person.map((item,index)=>{
                                return <li key={index}>{item.name+'---------'+item.age}</li>
                            })
                        }
                        <hr/>
                        <h2>使用id（每条数据的唯一标识）作为key</h2>
                        {
                            this.state.person.map((item)=>{
                                return <li key={item.id}>{item.name+'---------'+item.age}</li>
                            })
                        }
                    </div>
                )
            }
        }

        ReactDOM.render(<Showperson/>,document.getElementById('test1'));
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002102955.png)

#### 3.key的作用(2)

页面开始：

        1.生成虚拟DOM
        2.赋给真实DOM，渲染到页面
        3.更新状态
        4.生成新的虚拟DOM
        5.将新的虚拟DOM中节点的key与旧的虚拟DOM中节点的key比较：
            1.key不同：先将新虚拟DOM中的节点添加到真实DOM中，再将旧虚拟DOM中存在，但新虚拟DOM中不存在的节点删除
            2.key相同：
                (1)如果新虚拟DOM中的内容为文本，则直接进行替换
                (2)如果相同的key对应的节点内容也相同，那么就直接采用真实DOM中的这个节点
                (3)如果相同的key对应的节点的内容不同，那么就将新的虚拟DOM中的这个节点覆盖真实DOM中的这个节点
                (4)如果相同的key中存在子节点，那么子节点则继续进行如上规则的判断

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3_key的作用(2)</title>
</head>
<body>
    <div id="test1"></div>

    <script type="text/javascript" src="../js/新版本/react.development.js"></script>
    <script type="text/javascript" src="../js/新版本/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/新版本/babel.min.js"></script>

    <!-- 
        页面开始：
        1.生成虚拟DOM
        2.赋给真实DOM，渲染到页面
        3.更新状态
        4.生成新的虚拟DOM
        5.将新的虚拟DOM中节点的key与旧的虚拟DOM中节点的key比较：
            1.key不同：先将新虚拟DOM中的节点添加到真实DOM中，再将旧虚拟DOM中存在，但新虚拟DOM中不存在的节点删除
            2.key相同：
                (1)如果新虚拟DOM中的内容为文本，则直接进行替换
                (2)如果相同的key对应的节点内容也相同，那么就直接采用真实DOM中的这个节点
                (3)如果相同的key对应的节点的内容不同，那么就将新的虚拟DOM中的这个节点覆盖真实DOM中的这个节点
                (4)如果相同的key中存在子节点，那么子节点则继续进行如上规则的判断
     -->

     <!-- 
        页面开始：如果以index下标作为key的话（input输入框中的value值只在真实DOM中存在，虚拟DOM中不存在value，因为只有渲染到页面上，页面上输入了值，才有value）
        1.生成虚拟DOM
            <li key={0}>张三---------18 <input type="text"/> </li>
            <li key={1}>李四---------19 <input type="text"/> </li>
        2.赋给真实DOM，渲染到页面
        3.点击按钮更新状态
        4.生成新的虚拟DOM
            <li key={0}>王五---------20 <input type="text"/> </li>
            <li key={1}>张三---------18 <input type="text"/> </li>
            <li key={2}>李四---------19 <input type="text"/> </li>
        5.比较新虚拟DOM和旧虚拟DOM
            key为0：旧虚拟DOM：张三---------18，新虚拟DOM：王五---------20。内容不一样，所以将新虚拟DOM的 王五---------20 添加进真实DOM并渲染到页面。再比较子节点input输入框，发现子节点input输入框的标签名和属性值都是一样的，所以将旧的虚拟DOM中的input子节点直接给新的虚拟DOM用，那么造成之前输入的“张三“也被一并挪过来了
            key为1：旧虚拟DOM：李四---------19，新虚拟DOM：张三---------18。内容不一样，所以将新虚拟DOM的 张三---------18 添加进真实DOM并渲染到页面。再比较子节点input输入框，发现子节点input输入框的标签名和属性值都是一样的，所以将旧的虚拟DOM中的input子节点直接给新的虚拟DOM用，那么造成之前输入的“李四“也被一并挪过来了
            key为2：旧虚拟DOM中无，所以直接将新虚拟DOM的 李四---------19 以及子input节点，添加进真实DOM并渲染到页面。
        
        发现问题：张三---------18、李四---------19 在之前已经添加进真实DOM了，却因为key的值为index下标，由于在数组中的顺序改变了，导致index下标也
                 发生大变化，所以影响到key发生变化，导致 张三---------18 、李四---------19 因为key值与之前不用，被当作内容不一样的节点，又重新再一次添加进真实DOM。
                 且输入框错位问题。
      -->

      <!-- 
        页面开始：如果以id(每条数据的唯一标识)作为key的话
        1.生成虚拟DOM
            <li key={1}>张三---------18 <input type="text"/> </li>
            <li key={2}>李四---------19 <input type="text"/> </li>
        2.赋给真实DOM，渲染到页面
        3.点击按钮更新状态
        4.生成新的虚拟DOM
            <li key={3}>王五---------20 <input type="text"/> </li>
            <li key={1}>张三---------18 <input type="text"/> </li>
            <li key={2}>李四---------19 <input type="text"/> </li>
        5.比较新虚拟DOM和旧虚拟DOM
            key为1：旧虚拟DOM：张三---------18，新虚拟DOM：张三---------18。内容一样，所以直接采用真实DOM中的这个节点。子节点input也一样，所以新虚拟DOM直接采用旧虚拟DOM的子节点。
            key为2：旧虚拟DOM：李四---------19，新虚拟DOM：李四---------19。内容一样，所以直接采用真实DOM中的这个节点。子节点input也一样，所以新虚拟DOM直接采用旧虚拟DOM的子节点。
            key为3：旧虚拟DOM中无，所以直接将新虚拟DOM的 王五---------20以及子input节点 添加进真实DOM并渲染到页面
        
        效率比原本用index作为key高的多，之前已经添加进真实DOM中的节点不用再重复添加，也不会发生子input节点错位的情况
      -->
    <script type="text/babel">
        class Showperson extends React.Component{

            state={person:[{id:1,name:'张三',age:18,sex:'男'},{id:2,name:'李四',age:19,sex:'女'}]};

            addPerson=()=>{
                const person3={id:this.state.person.length+1,name:'王五',age:20,sex:'男'};
                this.setState({person:[person3,...this.state.person]});
            }

            render(){
                return (
                    <div>
                        <button onClick={this.addPerson}>点击新增人</button>
                        <h2>使用index（索引值）作为key</h2>
                        {
                            this.state.person.map((item,index)=>{
                                return <li key={index}>{item.name+'---------'+item.age} <input type="text"/> </li>
                            })
                        }
                        <hr/>
                        <h2>使用id（每条数据的唯一标识）作为key</h2>
                        {
                            this.state.person.map((item)=>{
                                return <li key={item.id}>{item.name+'---------'+item.age} <input type="text"/> </li>
                            })
                        }
                    </div>
                )
            }
        }

        ReactDOM.render(<Showperson/>,document.getElementById('test1'));
    </script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103008.png)

#### 4.总结

用index作为key可能引发的问题：

注意：输入类的包括，单选框、多选框啥的

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103018.png)

开发时如何选择key![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103029.png)

## 4.初始化react脚手架

### 1.什么叫脚手架

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103044.png)

### 2.初始化脚手架

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103054.png)

使用脚手架创建react项目成功后：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103107.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103118.png)

再打开项目文件，启动项目

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103130.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103141.png)

## 5.react脚手架项目结构

1. public：静态资源文件夹

- react和vue都是spa单页面程序，只有一个主页面，所有的子页面都包含在这里面，整个项目只有一个html文件

- 应用加壳：如果你写了一个web应用，但是想让它在手机上也能运行。一般的做法是，让专门的移动端人员用移动端的语法重新写出这个应用。但是应用加壳的意思是：在这个web应用外层加一层安卓壳（或IOS壳）。让它在移动端看上去运行的是一个移动端的应用，但实际上内部的代码是一个web应用

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103151.png)



index.html中

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103202.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103215.png)

2. src：源码文件夹

- **App.css**：作用于App.js的外部样式

- **App.js**：实际上是定义了一个函数式组件，函数名叫做App，该组件被默认暴露，在index.js中被渲染到index.html中的div节点中。（注意：index.js只把这一个组件渲染到index.html中，其他的组件全部为App.js的子组件）

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103227.png)

- **App.test.js**：专门测试App.js的测试文件

- **index.css**：作用于index.js的样式，可以在index.js中引入，也可以在index.html中引入

- **index.js**：入口文件

  - 引入React
  - 引入ReactDOM
  - 引入样式
  - 引入组件（.js和.jsx文件的后缀可以省略）

  - 引入介绍页面性能的文件

  

  - 渲染App.js组件到index.html的div节点中

  ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103244.png)

- reportWebVitals.js：用于介绍页面性能的，靠的是web-vitals这个库

  ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103256.png)

- setupTests.js：用于做应用的整体测试的，组件测试

3. 项目执行顺序：

   一来就执行index.js，将App组件渲染到index.html页面

## 6.分别暴露和默认暴露

默认暴露：直接暴露一整个组件，你可以在另一个页面先引入这个组件  **import 组件别名 from 路径**  ，然后可以用  **const {组件里的东西}=组件**  的形式从组件中获取东西，这里的{}表示结构化赋值。

分别暴露：暴露组件内的某个东西，比如方法、属性等等，引入： **import {名字} from 路径**  ,但是这里的{}不是结构化赋值，这里表示如果是获得分别暴露的东西，那么取的时候就需要加上{}

如果一个出现`import 组件别名,{名字} from ‘路径’` 这种引入方式，则表示这个组件内有多种暴露方式

## 7.一个简单的hello组件

react中区分组件js文件和普通js文件：

1. 组件js文件的文件名中首字母大写
2. 组件js文件其实可以命名为xxx.jsx，那么.jsx文件就代表组件js，.js文件就代表普通js文件

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103308.png)

- Hello.js子组件

  ```js
  // 引入默认暴露的React组件 和 分别暴露的React组件中的Component类
  import React,{Component} from 'react';
  // 引入样式
  import './Hello.css';
  
  // export default直接写在这里，简便
  // 这里直接写的Component，是因为已经引入了Component，可以不用加React前缀
  export default class Hello extends Component{
      render(){
          return (
              <div className="hello">
                  hello!!!
              </div>
          )
      }
  }
  ```

  Hello.css

  ```css
  .hello{
      background-color: aquamarine;
  }
  ```

- Wellcom.js子组件

  ```js
  // 引入默认暴露的React组件 和 分别暴露的React组件中的Component类
  import React,{Component} from 'react';
  // 引入样式
  import './Wellcom.css';
  
  // export default直接写在这里，简便
  // 这里直接写的Component，是因为已经引入了Component，可以不用加React前缀
  export default class Wellcom extends Component{
      render(){
          return (
              <div className='wellcom'>
                  Wellcom!!!
              </div>
          )
      }
  }
  ```

  Wellcom.css

  ```css
  .wellcom{
      background-color: rebeccapurple;
  }
  ```

- App.js主组件：包含所有子组件

  ```js
  // 引入React组件
  import React from 'react';
  // 引入Hello组件,react中引入时可以省略后缀.js或者是.jsx
  import Hello from './component/Hello/Hello'
  // 引入Wellcom组件
  import Wellcom from './component/Wellcom/Wellcom'
  
  
  const {Component}=React
  // 这里可以直接写Component是因为用结构赋值将Component从React中取出来了
  class App extends Component{
      render(){
          return (
              // App.js中一般是引入子组件,而不是自己去写虚拟DOM结构
              <div>
                  <Hello/>
                  <Wellcom/>
              </div>
          )
      }
  }
  
  // 默认暴露App组件
  export default App;
  ```

- index.html：主页面

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>主页面</title>
  </head>
  <body>
      <div id="root"></div>
  </body>
  </html>
  ```

- index.js入口：将App组件渲染到页面

  ```js
  // 引入React
  import React from 'react';
  // 引入ReactDOM
  import ReactDOM from 'react-dom';
  // 引入App组件，.js可以省略
  import App from './App';
  
  
  // 渲染App组件到index.html页面
  ReactDOM.render(<App/>,document.getElementById('root'));
  ```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103324.png)

## 8.样式模块化

真正项目开发中一般每个子组件对应的文件夹下的js和css都命名为index.js和index.css。

不管用不用index命名，如果每个组件内都没有与别的组件内的节点的className名发生重名的话，那就没有任何问题。但是如果存在某个组件内与另一个组件内的某个节点的className同名，那么会造成一个问题：

比如两个组件内都有一个className为hello的节点，并且两个组件对应的样式表都对这个节点进行了修改，都为.hello{}。那么最终子组件被引入到App主组件时，主组件采用的永远是后引入的子组件对应的重名样式

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103343.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103353.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103404.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103414.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103424.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103433.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103444.png)

解决方法：

1. 用less样式时，样式前面都会加上父节点的 .class名 这样就即使节点同名了也没事，因为父节点不一样

2. 实现样式模块化：
   - 给每个组件的css文件名从 文件名.css 修改为 文件名.module.css 
   - 引入的时候就可以把css当成一个模块来引入
   - 那么组件中的节点的className名就为 xxx.title

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103457.png)

- Hello/index.js

  ```js
  // 引入默认暴露的React组件 和 分别暴露的React组件中的Component类
  import React,{Component} from 'react';
  // 引入模块的方式引入样式,hello为自定义名字
  import hello from './index.module.css';
  
  // export default直接写在这里，简便
  // 这里直接写的Component，是因为已经引入了Component，可以不用加React前缀
  export default class Hello extends Component{
      render(){
          return (
              // 这里就可以将hello当成一个模块,直接拿取模块里面的值
              <div className={hello.title}>
                  hello!!!
              </div>
          )
      }
  }
  ```

- Wellcom/index.js

  ```js
  // 引入默认暴露的React组件 和 分别暴露的React组件中的Component类
  import React,{Component} from 'react';
  // 引入模块的方式引入样式,hello为自定义名字
  import wellcom from './index.module.css';
  
  // export default直接写在这里，简便
  // 这里直接写的Component，是因为已经引入了Component，可以不用加React前缀
  export default class Wellcom extends Component{
      render(){
          return (
              // 这里就可以将hello当成一个模块,直接拿取模块里面的值
              <div className={wellcom.title}>
                  Wellcom!!!
              </div>
          )
      }
  }
  ```

- App.js

  ```js
  // 引入React组件
  import React from 'react';
  // 引入Hello组件,react中引入时可以省略后缀.js或者是.jsx
  import Hello from './component/Hello'
  // 引入Wellcom组件
  import Wellcom from './component/Wellcom'
  
  
  const {Component}=React
  // 这里可以直接写Component是因为用结构赋值将Component从React中取出来了
  class App extends Component{
      render(){
          return (
              // App.js中一般是引入子组件,而不是自己去写虚拟DOM结构
              <div>
                  <Hello/>
                  <Wellcom/>
              </div>
          )
      }
  }
  
  // 默认暴露App组件
  export default App;
  ```


## 9. react插件及快捷键

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103512.png)

安装后，在.js文件中：

rcc+回车：快速生成类式组件模板

rfc+回车：快速生成函数式组件模板

## 9.功能界面的组件化编码流程

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103522.png)

## 10.TodoList案例

效果：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103532.png)

1. 输入任务名称，点击回车，自动将此任务添加进下面列表
2. 鼠标放在列表中的某项上，某项就显示高亮，并且该行右侧出现删除按钮
3. 可以选中要清楚的任务，再点击清除已完成任务按钮，即可从列表中消失并且在左下角的有实时数字显示完成的条数

### 1.静态组件

- 拆分组件：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103545.png)

- 实现静态页面（不包含事件）

注意事项：

1. 如果组件中有使用子组件，那么记得导入
2. 记得在主组件App.js中导入所有子组件
3. 记得在每个子组件中导入css样式
4. 记得将主组件渲染到主页面
5. 记得className
6. 记得style={{xxx:xxx}}

### 2.动态添加列表

所有的东西应该都放在父组件App.js中，因为方便传递给不同的子组件，也能暂时避免兄弟组件之间的数据传递。

注意事项：

1. 遍历出来的组件和标签都要加上key
2. 所有东西都放在父组件的state中，通过prop的方式传递给子组件。

### 3.添加todo

注意事项：

1. **父组件给子组件传数据**：props
2. **子组件给父组件传数据**：
   - 父组件给子组件通过props传一个函数，子组件调用函数时用实参给父组件传数据
3. 生成全世界唯一的字符串的一个工具叫做：nanoid
   - 先npm install nanoid
   - 再在用到的页面导入import {nanoid} from 'nanoid'，因为它里面用分别暴露的形式暴露了一个nanoid函数
   - 使用时调用nanoid()，即可得到一个全球唯一的字符串

### 4.鼠标移入效果

注意事项：

1. 触发事件后执行的，必须是一个函数，如果在赋值的时候，函数带了参数的话，那么它就是一个函数调用，而不是一个函数。解决方法是：让此函数的返回值为一个回调函数
2. 内联style中第一个花括号表示里面写的是js代码，第二个花括号表示这是一个对象

### 5.勾选时改变状态中的对象属性值

注意事项：

1. 如果是孙组件要给祖组件传数据，那么祖组件需要传一个函数给父组件，父组件再传一个函数给子组件，即要一级一级的传递
2. 可以使用{...对象，属性：新属性值}的方式修改对象中某一属性的属性值，**并直接返回一个新对象**
3. type为checkbox的input标签，得到它是否被勾选的属性是checked，checked为true，则被勾选，否则没被勾选

### 6.对props限制

注意事项：

1. 先install i prop-types
2. 记得导入包：import PropTypes from 'prop-types' //引入能对props进行限制的包
3. 记得是在接收props参数的类中进行限制

### 7.删除一个todo

注意事项：

1. 将删除的id传给App组件，在App组件中用filter进行删除
2. 删除前用confirm进行确认，注意：在react中confirm前要加window.，即window.confirm

### 8.实现底部功能

**1.实现底部计数功能：**

注意事项：

1. 用reduce对数组进行条件计数，切记：二目运算符中使用三目运算符，是需要加括号的。赋值语句中使用三目运算符，不需要加括号

**2.实现全选框选中时，所有选项全被选中。取消时：所有选择框全部被取消**

1. 通过map修改done的值，判断checked，当全选框的checked属性值为true时，修改所有选项的done为true，当全选框的checked属性值为false时，修改所有选项的done为false
2. 状态在哪，修改状态的方法就在哪里，所以修改状态应该放在App组件

**3.实现当所有选框都被选中时，全选框被选中，取消一个时，全选框被取消**

1. 通过设置标签上checked属性值，判断如果全部完成的数量===所有事项的数量，则将全选框的checked属性设置为true

### 9.todolist总结

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103559.png)

## 11.脚手架配置代理

### 1.理解

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103613.png)

### 2.例子

**先安装axios包，再导入包**

有一个小型的服务器，能返回固定学生数据，当服务器启动后，网址是：http://localhost:5000/students

现写一个页面：点击按钮，则请求到http://localhost:5000/students这个页面

**结果**：报错，因为当前页面所在网址是http://localhost:3000，不能跨域到http://localhost:5000/students。请求发送成功了，但是不能返回数据，因为客户端有Ajax引擎，Ajax引擎拒绝接收跟客户端端口不一致的服务器数据

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103625.png)

**解决**：配置代理

**中间代理的端口号要和客户端一样**，中间代理能接收不同端口号的服务器数据，因为它里面没有Ajax请求，它是由请求转发实现的

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103636.png)

### 3.配置代理

**方法一：**

- 在前端的package.json加一个"proxy":"代理服务器地址"，表示：所有发给3000的请求，全部转发给了5000。**注意：修改了配置文件后，必须重启一下项目**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103647.png)

- 设置一下请求的地址，改为3000，因为中间代理的端口号要和客户端的一致

```js
import React, { Component } from 'react'
import Axios from 'axios'

export default class App extends Component {

    getStudent=()=>{
        // 请求先发给中间代理，中间代理的端口号与客户端一致，由于在package.json中设置了proxy为5000，表示：所有发给3000的请求都会被转发到5000，所以这里发给中间代理3000的请求，又会被转发给5000
        Axios.get('http://localhost:3000/students').then(
            response=>{console.log("成功了",response.data);},
            error=>{console.log("失败了",error);}
        )
    }

    render() {
        return (
            <div>
                <button onClick={this.getStudent}>点我获取学生信息</button>
            </div>
        )
    }
}

```

- 启动服务器，运行程序，前端即可请求到端口不一致的服务器的数据

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103700.png)



但是并不是所有的3000请求都会被转发到5000，如果是3000中存在的资源，就不会被转发到5000，直接在3000中拿取。3000没有的资源，就会被转发到5000

```js
getStudent=()=>{
        // 请求先发给中间代理，中间代理的端口号与客户端一致，由于在package.json中设置了proxy为5000，表示：所有发给3000的请求都会被转发到5000，所以这里发给中间代理3000的请求，又会被转发给5000
        Axios.get('http://localhost:3000/students').then(
            response=>{console.log("成功了",response.data);},
            error=>{console.log("失败了",error);}
        )
        // 设置proxy为5000，并不是代表所有请求都被转发到了5000，如果是3000中存在的资源，就不会被转发到5000，直接在3000中拿取
        // 比如这里的index.html是3000中存在的，那么就不会去到服务器5000拿
        Axios.get('http://localhost:3000/index.html').then(
            response=>{console.log("成功了",response.data);},
            error=>{console.log("失败了",error);}
        )
    }
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103711.png)

方法一的局限：3000没有的资源只会找5000要，只能转发到一个服务器

**方法二：**

建一个名为setupProxy.js的配置文件，文件名不能改，react会自动识别这个配置文件。配置文件中不能用ES6的语法，而要用CIS的语法去写，因为setupProxy.js不是给前端运行的，react会识别这个配置文件，并将它加到webpack配置中，webpack中用的就是CJS语法

**增加了文件后，一定要重启项目**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103721.png)

setupProxy.js

```js

// 此包在安装react脚手架时就安装了
const proxy = require('http-proxy-middleware')

module.exports = function(app){
    /** proxy第一个参数：请求的前缀；第二个参数：转发详情
     *      1.proxy：表示将带有某个前缀的请求转发到另一个网址
     *      2.changeOrigin：控制服务器收到的请求头中Host字段的值，Host：标识着本次请求从哪里发出的
     *          为false（默认）：Host值为真正发出请求的来源，此例中即为3000
     *          为true：Host值为转发后的网址，此例中为5000或5001。最好改为true，因为避免服务器知道你实际上是3000发出的请求，就会对你有一些限制
     *      3.pathRewrite：修改请求前缀
     */

    // 表示只要是3000，并且带了/api1前缀的请求，即前面有http://localhost:3000/api1的请求，就会被转发到http://localhost:5000/api1。这时我们需要去掉前缀，所以用pathRewrite去掉前缀。
    // 所以只要有http://localhost:3000/api1的请求，就会被转发到http://localhost:5000
    // 同理只要有http://localhost:3000/api2的请求，就会被转发到http://localhost:5001
    app.use(
        proxy('/api1',{
            target:'http://localhost:5000',
            changeOrigin:true,
            pathRewrite:{'^/api1':''}
        }),
        proxy('/api2',{
            target:'http://localhost:5001',
            changeOrigin:true,
            pathRewrite:{'^/api2':''}
        })
    )
}
```

App.js

```js
import React, { Component } from 'react'
import Axios from 'axios'

export default class App extends Component {

    getStudent=()=>{
        // 如果3000中没有请求的这个资源，就会走/api1配置的这个代理：http://localhost:3000/api1前缀的请求被转发到http://localhost:5000
        Axios.get('http://localhost:3000/api1/students').then(
            response=>{console.log("成功了，学生为：",response.data);},
            error=>{console.log("失败了",error);}
        )
    }

    getCar=()=>{
        // 如果3000中没有请求的这个资源，就会走/api2配置的这个代理：http://localhost:3000/api2前缀的请求被转发到http://localhost:5001
        Axios.get('http://localhost:3000/api2/cars').then(
            response=>{console.log("成功了，汽车为：",response.data);},
            error=>{console.log("失败了",error);}
        )
    }

    render() {
        return (
            <div>
                <button onClick={this.getStudent}>点我获取学生信息</button>
                <button onClick={this.getCar}>点我获取汽车信息</button>
            </div>
        )
    }
}

```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103734.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103745.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103757.png)

## 12.github搜索案例

### 1.静态组件

- 先将普通前端项目转化为react前端项目
- 再拆分组件，拆分的时候注意查看html中可以舍弃的标签在css中有没有给它设置样式
- 拆分时记得在主组件中引入子组件，并且将组件放在该渲染的位置
- 样式也要拆分，先将所有样式放在父组件的样式中，然后根据不同子组件将样式进行拆分

注意：

1. 第三方样式库，如bootStrap.css最好放在public文件夹中，在public文件夹中建一个css文件夹，将第三方样式放在里面。放在public的样式应该被index.html文件引入。其他样式就放在src文件夹中（**如果要运行其他项目，需要将index.html中引入bootStrap样式的代码注释掉**）

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103808.png)

2. 注意修改class为className（vscode中ctrl+shift+l表示全选中文件中的出现的所有某个字段）
3. 注意修改外部样式为style={{key,value}}形式

### 2.axios发送请求

问题：如果向github服务器发送的请求次数多了，github就会禁止发送请求。

解决方法：设置一个中间服务器，端口号假设为5000，3000向5000发送请求，5000再向github服务器发送请求，请求的返回数据再返回给3000。5000请求github次数多了，github就会禁止5000再向它发送请求，但是在中间服务器5000中还设有另一个接口，就是返回模拟数据给3000。

所以如果5000向github发送请求，那么5000返回的就是真实数据，如果5000不能向github发送请求了，就直接返回模拟数据给3000

1. 启动5000服务器

   ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103820.png)

2. 配置setupProxy

   ```js
   // 此包在安装react脚手架时就安装了
   const proxy = require('http-proxy-middleware')
   
   module.exports = function(app){
       /** proxy第一个参数：请求的前缀；第二个参数：转发详情
        *      1.proxy：表示将带有某个前缀的请求转发到另一个网址
        *      2.changeOrigin：控制服务器收到的请求头中Host字段的值，Host：标识着本次请求从哪里发出的
        *          为false（默认）：Host值为真正发出请求的来源，此例中即为3000
        *          为true：Host值为转发后的网址，此例中为5000或5001。最好改为true，因为避免服务器知道你实际上是3000发出的请求，就会对你有一些限制
        *      3.pathRewrite：修改请求前缀
        */
   
       // 表示只要是3000，并且带了/api1前缀的请求，即前面有http://localhost:3000/api1的请求，就会被转发到http://localhost:5000/api1。这时我们需要去掉前缀，所以用pathRewrite去掉前缀。
       // 所以只要有http://localhost:3000/api1的请求，就会被转发到http://localhost:5000
       // 同理只要有http://localhost:3000/api2的请求，就会被转发到http://localhost:5001
       app.use(
           proxy('/api1',{
               target:'http://localhost:5000',
               changeOrigin:true,
               pathRewrite:{'^/api1':''}
           })
       )
   }
   ```

3. 发送请求 

   ```js
   // 这里客户端为3000服务器为5000，而且5000服务器中没有解决跨域问题。所以需要设置代理，代理端口号为3000，所有发送给3000的请求都会被转发到5000
           // 这里有一个简写的方法：如果当前客户端为3000，代码中可以省略主机名+3000，即省略http://localhost:3000
           axios.get(`/api1/search/users?q=${keywords}`).then(
               response=>{console.log("成功了",response.data);},
               error=>{console.log("失败了",error);}
           )
   ```

### 3.展示数据

分析请求回来的数据：
![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103835.png)

由于数据是在Search组件中请求回来的，需要在List组件中展示，所以需要先把数据传给父组件App，再由父组件传给List组件。

### 4.完善案例

效果：

1. 一进页面，还没有搜索任何东西的时候，页面显示“欢迎使用，输入关键字点击搜索，即可获取github用户”

2. 点击搜索后，在没返回数据之前，显示加载中
3. 成功返回数据后，显示返回的数据
4. 请求失败后，显示“请求失败”

实现：

- 不同时刻显示不同页面，由于状态的改变必会驱动页面改变，所以需要在状态中设置这四种情况对应的属性

- 在Search组件中根据不同时刻修改state属性的属性值，并传给App组件

- App组件得到修改后的对象属性，进而setState，并将state对象传给List子组件进行条件展示

注意：**react中不能直接渲染一个对象到页面，可以展示对象的某个属性**

App.js

```js
import React, { Component } from 'react'
import Search from './component/Search/index'
import List from './component/List/index'

export default class App extends Component {

    state={first:true,  //是否第一次打开页面
        loading:false,  //是否正在加载中
        error:'',  //存储错误信息
        people:[]};  //用户数据

    // 点击查询按钮时，获取修改了值的属性对象，再更新状态中相应的属性
    updateState=(stateObj)=>{
        this.setState(stateObj);
    }

    render() {

        return (
            <div className="container">
                <Search updateState={this.updateState}/>
                <List {...this.state}/>
            </div>
        )
    }
}

```

Search.js

```js
import React, { Component } from 'react'
import axios from 'axios';

export default class Search extends Component {

    handleClick=()=>{
        // 获取用户的输入
        // 连续结构赋值并给拿到的属性取别名
        // 表示：从this中拿到inputElement对象的value属性，并给value属性取一个别名叫作keywords
        const {inputElement:{value:keywords}} = this

        // 发送网络请求
        // 这里客户端和服务器的主机名和端口不一样，但能请求成功，是因为github服务器在后端用了cors加了一个特殊的响应头解决了跨域问题
        
        // 在点击搜索按钮后，发送请求前，设置页面的first为false，loading设置为true
        this.props.updateState({first:false,loading:true});
        
        // axios.get(`https://api.github.comdd/search/users?q=${keywords}`).then(
        //     response=>{console.log("成功了",response.data);},
        //     error=>{console.log("失败了",error);}
        // )

        // 这里客户端为3000服务器为5000，而且5000服务器中没有解决跨域问题。所以需要设置代理，代理端口号为3000，所有发送给3000的请求都会被转发到5000
        // 这里有一个简写的方法：如果当前客户端为3000，代码中可以省略主机名+3000，即省略http://localhost:3000
           axios.get(`/api1/search/users?q=${keywords}`).then(
               // 请求成功时将通知App更新状态
               response=>{this.props.updateState({people:response.data.items,loading:false})},
               // 请求失败时将通知App更新状态
               error=>{this.props.updateState({error:error,loading:false})}
         )
    }

    render() {
        return (
            <section className="jumbotron">
                <h3 className="jumbotron-heading" >Search Github Users</h3>
                <div>
                    {/* 回调函数的形式定义ref，将input输入框结点赋值给实例对象中的inputElement属性 */}
                    <input ref={currentCode=>this.inputElement=currentCode} type="text" placeholder="enter the name you search"/>&nbsp;<button onClick={this.handleClick}>Search</button>
                </div>
            </section>
        )
    }
}
```

List.js

```js
import { object } from 'prop-types'
import React, { Component } from 'react'
import './index.css'

export default class List extends Component {
    render() {
        const {first,loading,error,people}=this.props;
        console.log(first,loading,error,people);
        return (
            <div className="row">
                {
                    // 因为在jsx中的大括号内只能写表达式，不能写if这种语句判断，所以选择用嵌套三元式来进行条件判断
                    // react中不能直接渲染一个对象到页面，可以展示对象的某个属性，所以显示error对象中的message属性
                    first?<h2>欢迎使用，请输入关键字进行搜索用户</h2>:
                    loading?<h2>加载中.........</h2>:
                    error?<h2 style={{color:"red"}}>错误信息:{error.message}</h2>:
                    people.map(item=>{
                        return (
                            <div className="card" key={item.id}>
                                {/* href中如果设置了target="_blank"，另开一个页面。但是需要和rel="noreferrer"一起使用，否则会弹出警告 */}
                                <a rel="noreferrer" href={item.html_url} target="_blank">
                                    {/* img标签必须设置一个alt属性：表示当图片加载失败时显示的文字，否则会警告 */}
                                    <img alt="Avatar" src={item.avatar_url} style={{width:'100px'}}/>
                                </a>
                                <p className="card-text">{item.login}</p>
                            </div>
                        )
                    })
                }
            </div>
        )
    }
}

```

## 13.消息订阅与发布

订阅步骤：

1. 指定消息名

2. 接收数据（包含消息名、数据）

   ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103914.png)

   ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103927.png)

发布步骤：

1. 指定消息名

2. 携带数据

   ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002103937.png)

在需要接收数据的组件中订阅消息，指定消息名，**如果有人发布了这个消息，那么你指定的回调就要被调用。**不是你调的，是发布消息者调的，别人在调用函数的时候需要指定消息名和携带这个消息的数据

消息订阅与发布需要借助工具库：有很多工具库，这里我们选择Pubsub库

1. 安装库：npm i pubsub-js
2. 导入库：import PubSub from 'pubsub-js'

### 1.优化github搜索案例

**可以做到任何组件之间的数据直接传递**

`之前的思想：`Search组件中的搜索举动影响着List组件中的展示，但由于兄弟组件之间无法直接传递数据，所以借助App父组件，将所有数据都放在App父组件的state中，Search的举动影响着那些状态属性，所以每改变一次，就将改变的状态属性传给父组件，父组件再去修改自己状态中的相应属性，然后传给List子组件去动态展示

`现在的思想`：利用消息发布与订阅，Search组件负责发布消息，List组件负责订阅消息，所以将所有的数据放在List组件的State中。Search的举动影响着那些状态属性，所以每改变一次，就将改变的状态属性数据发布一次，那么List组件就会动态订阅消息，再进行setState状态修改，进而驱动页面动态显示数据



注意事项：

- 消息的订阅一般放在componentDidMount，组件挂载后

- subscribe方法中第二个参数：回调函数，**函数的参数必须是两个，第一个为消息名，第二个为数据**，如果没有用到某个参数，可以用`_`占位符代替那个参数

- 一般需要指定一个变量接收消息订阅，方便在取消订阅的时候能找的到取消的是哪一个消息订阅。如this.token=PubSub.subscribe('stateObj',(_,data)=>{this.setState(data)});

  PubSub.unsubscribe(this.token);

App.js

```js
import React, { Component } from 'react'
import Search from './component/Search/index'
import List from './component/List/index'

export default class App extends Component {
    render() {
        return (
            <div className="container">
                <Search/>
                <List/>
            </div>
        )
    }
}

```

Search.js

```js
import React, { Component } from 'react'
import axios from 'axios';
import PubSub from 'pubsub-js'

export default class Search extends Component {

    handleClick=()=>{
        const {inputElement:{value:keywords}} = this
        
        // 在点击搜索按钮后，发送请求前，发布消息：{first:false,loading:true}，消息名为：stateObj
        PubSub.publish('stateObj',{first:false,loading:true});

        // axios.get(`https://api.github.comdd/search/users?q=${keywords}`).then(
        //     response=>{console("成功了",response.data);},
        //     error=>{console("失败了",error);}
        // )

        // 这里客户端为3000服务器为5000，而且5000服务器中没有解决跨域问题。所以需要设置代理，代理端口号为3000，所有发送给3000的请求都会被转发到5000
        // 这里有一个简写的方法：如果当前客户端为3000，代码中可以省略主机名+3000，即省略http://localhost:3000
        axios.get(`/api1/search/users?q=${keywords}`).then(
            // 请求成功时，发布消息：{people:response.data.items,loading:false}，消息名为：stateObj
            response=>{PubSub.publish('stateObj',{people:response.data.items,loading:false});},
            // 请求失败时，发布消息：{error:error,loading:false}，消息名为：stateObj
            error=>{PubSub.publish('stateObj',{error:error,loading:false});}
        )
    }

    render() {
        return (
            <section className="jumbotron">
                <h3 className="jumbotron-heading" >Search Github Users</h3>
                <div>
                    <input ref={currentCode=>this.inputElement=currentCode} type="text" placeholder="enter the name you search"/>&nbsp;<button onClick={this.handleClick}>Search</button>
                </div>
            </section>
        )
    }
}

```

List.js

```js
import React, { Component } from 'react';
import PubSub from 'pubsub-js';
import './index.css';

export default class List extends Component {

    state={
        first:true,  //是否第一次打开页面
        loading:false,  //是否正在加载中
        error:'',  //存储错误信息
        people:[]  //用户数据
    };  

    // 将订阅消息放在componentDidMount中，组件挂载完毕后，调用一次componentDidMount函数，那么就开启了一个消息订阅
    // subscribe中第二个参数：回调函数，回调函数的参数必须有两个，第一个是消息名，第二个是数据。如果有某个参数用不到，可以用_占位符代替，一下例子中就用占位符代替了msg
    componentDidMount(){
        // 指定变量接收消息订阅，方便取消消息订阅时能找到是哪一个消息订阅
        // 只要一发布stateObj消息，就会调用回调函数，从而更新state
        this.token=PubSub.subscribe('stateObj',(_,data)=>{this.setState(data)});
    }

    // 在组件被卸载之前取消消息订阅
    componentWillUnmount(){
        PubSub.unsubscribe(this.token);
    }

    render() {

        const {first,loading,error,people}=this.state;
        return (
            <div className="row">
                {
                    // 因为在jsx中的大括号内只能写表达式，不能写if这种语句判断，所以选择用嵌套三元式来进行条件判断
                    // react中不能直接渲染一个对象到页面，可以展示对象的某个属性，所以显示error对象中的message属性
                    first?<h2>欢迎使用，请输入关键字进行搜索用户</h2>:
                    loading?<h2>加载中.........</h2>:
                    error?<h2 style={{color:"red"}}>错误信息:{error.message}</h2>:
                    people.map(item=>{
                        return (
                            <div className="card" key={item.id}>
                                {/* href中如果设置了target="_blank"，另开一个页面。但是需要和rel="noreferrer"一起使用，否则会弹出警告 */}
                                <a rel="noreferrer" href={item.html_url} target="_blank">
                                    {/* img标签必须设置一个alt属性：表示当图片加载失败时显示的文字，否则会警告 */}
                                    <img alt="Avatar" src={item.avatar_url} style={{width:'100px'}}/>
                                </a>
                                <p className="card-text">{item.login}</p>
                            </div>
                        )
                    })
                }
            </div>
        )
    }
}
```

## 14.fetch发送请求

[学习文档](https://segmentfault.com/a/1190000003810652)

发送ajax请求有三种方法：

- 原生xhr（XMLHttpRequest）
- jQuery（封装的xhr，需要安装）
- axios（封装xhr，需要安装）
- fetch：
  - 跟xhr同级，不再使用xhr对象提交请求
  - 是js的原生函数，不需要安装
  - promise风格



知识点：

- fetch主要是关注分离思想：分步骤做某件事，如果当前步骤成功了，再往下继续做，如果失败了，就不继续做。如果是请求数据：那么先看服务器连接成不成功，再看数据返回成不成功

  

- promise实例才能.then，.then是用来指定成功的回调和失败的回调的。**.then的实例对象的状态和值取决于当前.then的返回值。**

- **如果.then的返回值为非promise值，那么.then的promise实例对象的状态为成功，值为这个非promise值**

- **如果.then的返回值为promise对象，那么.then的promise实例对象就为当前返回的这个promise实例对象**

- 只有断网或者其他不可抗因素，才会使.then的promise实例对象的状态为失败

- 返回初始化promise对象是可以终止promise链的

  ​        return new Promise(()=>{});

- promise实例可以在最后统一处理错误，但是在最后统一处理错误的话，那么.then中就只有成功的情况，就可以用await简化.then。

- await后面接的是promise实例对象，可以等到这个promise实例对象的成功的结果，但是需要在await所在函数上添加async。由于await只能等到成功的结果，所以需要用try、catch捕捉失败的情况



- 服务器请求成功中的response.json()是一个promise实例对象。如果数据获取成功了，那么.then的promise实例对象的状态为成功，值为获取的数据。如果数据获取数据失败了，那么.then的promise实例对象的状态为失败，值为失败原因。如果抛出异常了，.then的promise实例对象的状态为失败，值为抛出的异常

  ```js
  // 不规范的fetch写法
          fetch(`/api1/search/users2?q=${keywords}`).then(
              // 此处的response.json()为一个实例对象，数据获取成功了，那么.then的promise实例对象的状态为成功，值为获取的数据。如果数据获取数据失败了，那么.then的promise实例对象的状态为失败，值为失败原因。
              response=>{
                  console.log("连接服务器成功了");
                  // 访问数据
                  response.json().then(
                      response=>{console.log("访问数据成功了",response)},
                      error=>{console.log("访问数据失败了",error)}
                  )
              },
              error=>{console.log("连接服务器失败了",error);}
          )
  ```

  

 应用到github搜索案例上：

Search.js

```js
import React, { Component } from 'react'
import axios from 'axios';
import PubSub from 'pubsub-js'

export default class Search extends Component {

    handleClick=async()=>{
        const {inputElement:{value:keywords}} = this
        
        // 在点击搜索按钮后，发送请求前，发布消息：{first:false,loading:true}，消息名为：stateObj
        PubSub.publish('stateObj',{first:false,loading:true});

        // axios.get(`/api1/search/users?q=${keywords}`).then(
        //     response=>{PubSub.publish('stateObj',{people:response.data.items,loading:false});},
        //     error=>{PubSub.publish('stateObj',{error:error,loading:false});}
        // )

        // 不规范的fetch写法
        // fetch(`/api1/search/users2?q=${keywords}`).then(
        //     // 此处的response.json()为一个实例对象，数据获取成功了，那么.then的promise实例对象的状态为成功，值为获取的数据。如果数据获取数据失败了，那么.then的promise实例对象的状态为失败，值为失败原因。
        //     response=>{
        //         console.log("连接服务器成功了");
        //         // 访问数据
        //         response.json().then(
        //             response=>{console.log("访问数据成功了",response)},
        //             error=>{console.log("访问数据失败了",error)}
        //         )
        //     },
        //     error=>{console.log("连接服务器失败了",error);}
        // )

        // 规范的fetch写法
        // fetch(`/api1/search/users2?q=${keywords}`).then(
        //     // 此处的response.json()为一个实例对象，数据获取成功了，那么.then的promise实例对象的状态为成功，值为获取的数据。如果数据获取数据失败了，那么.then的promise实例对象的状态为失败，值为失败原因。
        //     response=>{
        //         console.log("连接服务器成功了");
        //         // 访问数据
        //         // 返回的为promise实例对象，所以此promise实例对象将作为.then的promise实例对象，那么：就可以链式.then
        //         return response.json();
        //     },
        //     error=>{
        //         console.log("连接服务器失败了",error);
        //         // 返回初始化promise对象是可以终止promise链的
        //         return new Promise(()=>{});
        //     }
        // ).then(
        //     response=>{console.log("访问数据成功了",response)},
        //     error=>{console.log("访问数据失败了",error)}
        // )

        // 优化：统一处理错误
        // fetch(`/api1/search/users2?q=${keywords}`).then(
        //     // 此处的response.json()为一个实例对象，数据获取成功了，那么.then的promise实例对象的状态为成功，值为获取的数据。如果数据获取数据失败了，那么.then的promise实例对象的状态为失败，值为失败原因。
        //     response=>{
        //         console.log("连接服务器成功了");
        //         // 访问数据
        //         // 返回的为promise实例对象，所以此promise实例对象将作为.then的promise实例对象，那么：就可以链式.then
        //         return response.json();
        //     },
        // ).then(
        //     response=>{console.log("访问数据成功了",response)},
        // ).catch(
        //     error=>{
        //         console.log("出现错误",error);
        //     }
        // )

        // 优化：await后面接的是promise实例对象，可以等到这个promise实例对象的成功的结果，但是需要在await所在函数上添加async。由于await只能等到成功的结果，所以需要用try、catch捕捉失败的情况
        // try{
        //     // 得到的是服务器连接成功的结果
        //     const response=await fetch(`/api1/search/users2?q=${keywords}`);
        //     // 得到的是数据获取成功的结果
        //     const data=await response.json();
        //     console.log("请求成功",data);
        // }catch(error){
        //     console.log("请求出错",error);
        // }

        // 应用到本例
        try{
            // 得到的是服务器连接成功的结果
            const response=await fetch(`/api1/search/users2?q=${keywords}`);
            // 得到的是数据获取成功的结果
            const data=await response.json();
            PubSub.publish('stateObj',{people:data.items,loading:false});
        }catch(error){
            PubSub.publish('stateObj',{error:error,loading:false});
        }
    }

    render() {
        return (
            <section className="jumbotron">
                <h3 className="jumbotron-heading" >Search Github Users</h3>
                <div>
                    <input ref={currentCode=>this.inputElement=currentCode} type="text" placeholder="enter the name you search"/>&nbsp;<button onClick={this.handleClick}>Search</button>
                </div>
            </section>
        )
    }
}
```

总结fetch：

缺点：老版本的浏览器不支持fetch。

## 15.路由

### 15.1 SPA

单页面，多组件



做一个导航栏按钮的切换效果

以前：先写多个页面，用超链接，点击超链接跳转到另一个页面

现在：使用路由，点击导航链接，引起浏览器路径变化。路径变化被路由器捕捉到，进行组件匹配，从而改变展示区内容

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104041.png)


![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104051.png)

### 15.2 对路由的理解

前端路由就是通过改变浏览器上面地址的**path**来达到显示不同组件的效果，改变地址的**path**不会去到一个新页面，而是改变当前页面内显示的组件

path就是/xxx，不带.html，如果是/xxx.html表示的是一个页面，而/xxx表示一个组件

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104104.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104114.png)

### 15.3 路由原理history

BOM浏览器对象中存在一个history对象：window.history。

history是一个栈，后push的path就存在栈顶

**原理：浏览器地址后面跟的永远是history栈顶的path，显示的也是栈顶的path对应的组件**

方法一：直接使用H5推出的history身上的API

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>前端路由的基石_history</title>
</head>
<body>

	<!-- 浏览器地址后面跟的的永远是history栈顶的path，显示的也是栈顶的path对应的组件 -->

	<!-- onclick="return false" 表示点击连接不会跳转到其他页面 -->
	<a href="http://www.atguigu.com" onclick="return push('/test1') ">push test1</a><br><br>
	<button onclick="push('/test2')">push test2</button><br><br>
	<button onclick="replace('/test3')">replace test3</button><br><br>
	<button onclick="back()">&lt;= 回退</button>
	<button onclick="forword()">前进 =&gt;</button>

	<!-- 引入此库后，就有一个History对象 -->
	<script type="text/javascript" src="https://cdn.bootcss.com/history/4.7.2/history.js"></script>
	<script type="text/javascript">
		// 方法一，直接使用H5推出的history身上的API
		// 这里的history直接操作的是BOM里面的history对象，直接修改浏览器的访问地址后缀
		let history = History.createBrowserHistory()

		function push (path) {
			// 往BOM中的history栈顶中添加此path
			history.push(path)
			// onclick="return return false" 表示点击连接不会跳转到其他页面
			return false
		}

		function replace (path) {
			// 表示替换掉栈顶的那个元素
			history.replace(path)
		}

		function back() {
			// 将history堆栈中的指针位置减1
			history.goBack()
		}

		function forword() {
			// 将history堆栈中的指针位置加1
			history.goForward()
		}

		// history监听
		// 只要路径变化了就会触发此回调函数
		history.listen((location) => {
			console.log('请求路由路径变化了', location)
		})
	</script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104128.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104139.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104148.png)



方法二：hash值（锚点）

#后面的path不会作为资源发送给服务器，也就是不干涉请求资源的路径

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>前端路由的基石_history</title>
</head>
<body>

	<!-- 浏览器地址后面跟的的永远是history栈顶的path，显示的也是栈顶的path对应的组件 -->

	<!-- onclick="return false" 表示点击连接不会跳转到其他页面 -->
	<a href="http://www.atguigu.com" onclick="return push('/test1') ">push test1</a><br><br>
	<button onclick="push('/test2')">push test2</button><br><br>
	<button onclick="replace('/test3')">replace test3</button><br><br>
	<button onclick="back()">&lt;= 回退</button>
	<button onclick="forword()">前进 =&gt;</button>

	<!-- 引入此库后，就有一个History对象 -->
	<script type="text/javascript" src="https://cdn.bootcss.com/history/4.7.2/history.js"></script>
	<script type="text/javascript">
		// 方法二，hash值（锚点）
        // 这里的history直接操作的是BOM里面的history对象，自动在浏览器访问地址后面加上#/，点击链接改变的浏览器地址部分，全在#/后面，而#后面的部分不会作为请求资源
		let history = History.createHashHistory()

		function push (path) {
			// 往BOM中的history栈中添加此path
			history.push(path)
			// onclick="return return false" 表示点击连接不会跳转到其他页面
			return false
		}

		function replace (path) {
			// 表示替换掉栈顶的那个元素
			history.replace(path)
		}

		function back() {
			// 将栈顶元素出栈
			history.goBack()
		}

		function forword() {
			// 将goBack出栈的那个栈顶元素重新进栈
			history.goForward()
		}

		// history监听
		// 只要路径变化了就会触发此回调函数
		history.listen((location) => {
			console.log('请求路由路径变化了', location)
		})
	</script>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104159.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104209.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104219.png)

### 15.4 react-router-dom理解

react-router库有三个方面：

1. web，针对web用
2. native
3. any

现在所学的是第一种，针对web用的，所以用的是react-router-dom库

（印记中文中有react-router相关中文文档）

### 15.5 路由的基本使用

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104229.png)



1. 安装react-router-dom

   npm install react-router-dom

2. 找到导航栏，找到展示区。思想：点击导航链接，引起浏览器路径变化。路径变化被路由器捕捉到，进行组件匹配，从而改变展示区内容

	![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104243.png)

3. 编写路由链接：指定点击链接后，改变浏览器path

4. 注册路由：指定每个path对应的是哪个组件，key为path，value为组件

注意：

- 记得导入组件
- 由于Link标签和Route标签都需要包含在路由器标签中，所以直接将路由器标签在组件被使用的地方包含整个组件
- 不同路由器标签表示不同路由器，所以不能用两个路由器标签分别包含Link和Route
- 路由器分为BrowserRouter和HashRouter，主要区别见15.20
- Route中的component属性首字母小写，属性值为{组件名}形式，而不是<组件名/>形式



About.js

```js
import React, { Component } from 'react'

export default class About extends Component {
    render() {
        return (
            <h3>我是About的内容</h3>
        )
    }
}
```

Home.js

```js
import React, { Component } from 'react'

export default class Home extends Component {
    render() {
        return (
            <h3>我是Home的内容</h3>
        )
    }
}
```

index.js

```js
import React from 'react'
import ReactDom from 'react-dom'
// 记得要引入组件
import {BrowserRouter} from 'react-router-dom'
import App from './App'

ReactDom.render(
    // 直接将路由器标签包含整个组件，那么组件中就不存在不同路由器标签互相不能访问的问题
    <BrowserRouter>
        <App/>
    </BrowserRouter>,
    document.getElementById("root")
    );
```

App.js

```js
import React, { Component } from 'react'
// Link、Route首字母大写，是组件
import {Link, Route} from 'react-router-dom'
import About from './component/About'
import Home from './component/Home'

export default class App extends Component {
    render() {
        return (
            <div>
                <div className="row">
                    <div className="col-xs-offset-2 col-xs-8">
                        <div className="page-header"><h2>React Router Demo</h2></div>
                    </div>
                </div>
                <div className="row">
                    <div className="col-xs-2 col-xs-offset-2">
                        <div className="list-group">
                            {/* 原生html中，靠a标签跳转到不用页面实现导航功能 */}
                            {/* <a className="list-group-item active" href="./about.html">About</a>
                            <a className="list-group-item" href="./home.html">Home</a> */}

                            {/* 在React路由中靠路由链接实现切换组件-------编写路由链接 */}
                            {/* 不是href了，而是to，并且to属性值为path，即 /组件名 这种形式的 */}
                            {/* 路由链接需要被包含在路由器里面，所以外侧需要包含一个Router标签，但是Router标签有两种：BrowserRouter、HashRouter，需要在其中选择一个。二者区别在 路由原理history 中有详解。一般路由器标签都直接放在组件使用的地方，直接包含整个组件 */}
                            {/* 在组件内部使用多个路由器标签，代表的是不同的路由器，互相不相干，所以一般直接将路由器标签放在组件被使用的地方，直接包含整个组件 */}
                            <Link className="list-group-item active" to="/about">About</Link>
                            <Link className="list-group-item" to="/home">Home</Link>
                        </div>
                    </div>
                    <div className="col-xs-6">
                        <div className="panel">
                            <div className="panel-body">
                                {/* 注册路由 */}
                                {/* 一定要注意component首字母小写 */}
                                <Route path='/about' component={About}/>
                                <Route path='/home' component={Home}/>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        )
    }
}
```

样式需要引入第三方库

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104301.png)

index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- 引入第三方样式：bootstrap -->
    <link rel="stylesheet" href="./css/bootstrap.css"/>
    <title>主页面</title>
</head>
<body>
    <div id="root"></div>
</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104322.png)



细节：其实浏览器是不认识Link这种链接的，所以浏览器会自动将Link链接转为a链接，并加一些监听让其实现不跳转

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104345.png)

### 15.6 路由组件与一般组件

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104354.png)

location.pathname、match.path、match.url都可以获取当前组件所处的路由路径

### 15.7 NavLink的使用（高亮）

对于booptstrap来说，点击链接，链接变成高亮的话，需要在className属性值后面追加一个active

**而直接使用NavLink代替Link，就可以达到点击链接后，改变链接的样式**

- 先引入NavLink组件从react-router-dom包中

- NavLink默认是点击链接后，往className属性值的后面追加一个active，但是可以用activeClassName属性指定点击链接后追加的是什么
- 需要注意的点是：bootstrap样式的权重很高，如果追加的是自己定义的样式，那么需要在每一个样式后面加上 !important

index.heml

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- 引入第三方样式：bootstrap -->
    <link rel="stylesheet" href="./css/bootstrap.css"/>
    <style>
        /* bootstrap样式的权重很高，所以加上!important */
        .testNavLink{background-color:cadetblue !important};
    </style>
    <title>主页面</title>
</head>
<body>
    <div id="root"></div>
</body>
</html>
```

App.js

```js
import React, { Component } from 'react'
// Link、Route首字母大写，是组件
import {NavLink, Route} from 'react-router-dom'
import About from './component/About'
import Home from './component/Home'

export default class App extends Component {
    render() {
        return (
            <div>
                <div className="row">
                    <div className="col-xs-offset-2 col-xs-8">
                        <div className="page-header"><h2>React Router Demo</h2></div>
                    </div>
                </div>
                <div className="row">
                    <div className="col-xs-2 col-xs-offset-2">
                        <div className="list-group">
                            {/* 原生html中，靠a标签跳转到不用页面实现导航功能 */}
                            {/* <a className="list-group-item active" href="./about.html">About</a>
                            <a className="list-group-item" href="./home.html">Home</a> */}

                            {/* 在React路由中靠路由链接实现切换组件-------编写路由链接 */}
                            {/* 不是href了，而是to，并且to属性值为path，即 /组件名 这种形式的 */}
                            {/* 路由链接需要被包含在路由器里面，所以外侧需要包含一个Router标签，但是Router标签有两种：BrowserRouter、HashRouter，需要在其中选择一个。二者区别在 路由原理history 中有详解。一般路由器标签都直接放在组件使用的地方，直接包含整个组件 */}
                            {/* 在组件内部使用多个路由器标签，代表的是不同的路由器，互相不相干，所以一般直接将路由器标签放在组件被使用的地方，直接包含整个组件 */}
                            <NavLink activeClassName="testNavLink" className="list-group-item" to="/about">About</NavLink>
                            <NavLink activeClassName="testNavLink" className="list-group-item" to="/home">Home</NavLink>
                        </div>
                    </div>
                    <div className="col-xs-6">
                        <div className="panel">
                            <div className="panel-body">
                                {/* 注册路由 */}
                                {/* 一定要注意component首字母小写 */}
                                <Route path='/about' component={About}/>
                                <Route path='/home' component={Home}/>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        )
    }
}
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104408.png)

### 15.8 二次封装NavLink

虽然NavLink上的activeClassName可以指定链接被点击后，className后面能追加的属性名，但是如果页面中有很多个链接，className都相同，并且点击链接后都变成橙色 ，那么页面中就有很多NavLink标签，并且很多重复的属性。

我们可以把NavLink进行二次封装，将相同的属性封装起来，**不同的属性值通过props传递**。不同的标签体内容，也可以通过props传递，只不过标签体内容传递到props里面，是一个value，它的key默认指定为children。通过this.props.children就可以得到传递的标签体内容



index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- 引入第三方样式：bootstrap -->
    <link rel="stylesheet" href="./css/bootstrap.css"/>
    <style>
        /* bootstrap样式的权重很高，所以加上!important */
        .testNavLink{background-color:cadetblue !important};
    </style>
    <title>主页面</title>
</head>
<body>
    <div id="root"></div>
</body>
</html>
```

MyNavLink.js

```js
import React, { Component } from 'react'
import { NavLink } from 'react-router-dom'

export default class MyNavLink extends Component {
    render() {
        return (
            <div>
                {/* 这里不需要将所有传递的属性一一以键值对的形式写在标签上，直接用...{this.props}展开运算符。
                包括标签体内容children，因为在标签上写children属性和直接在开始标签和结束标签之间写标签体内容是一样的 */}
                <NavLink activeClassName="testNavLink" className="list-group-item" {...this.props}></NavLink>
            </div>
        )
    }
}
```

App.js

```js
import React, { Component } from 'react'
// Link、Route首字母大写，是组件
import {Route} from 'react-router-dom'

import About from './pages/About'
import Home from './pages/Home'
import Header from './component/Header'
import MyNavLink from './component/MyNavLink'

export default class App extends Component {
    render() {
        return (
            <div>
                <Header/>
                <div className="row">
                    <div className="col-xs-2 col-xs-offset-2">
                        <div className="list-group">
                            {/* 原生html中，靠a标签跳转到不用页面实现导航功能 */}
                            {/* <a className="list-group-item active" href="./about.html">About</a>
                            <a className="list-group-item" href="./home.html">Home</a> */}

                            {/* 在React路由中靠路由链接实现切换组件-------编写路由链接 */}
                            {/* 不是href了，而是to，并且to属性值为path，即 /组件名 这种形式的 */}
                            {/* 路由链接需要被包含在路由器里面，所以外侧需要包含一个Router标签，但是Router标签有两种：BrowserRouter、HashRouter，需要在其中选择一个。二者区别在 路由原理history 中有详解。一般路由器标签都直接放在组件使用的地方，直接包含整个组件 */}
                            {/* 在组件内部使用多个路由器标签，代表的是不同的路由器，互相不相干，所以一般直接将路由器标签放在组件被使用的地方，直接包含整个组件 */}
                            
                            {/* 不同的属性值，通过props传递。不同的标签体，也可以通过props传递，传递后的value为children，值为传递的标签体内容，在MyNavLink中可以通过this.props.children拿到传递的标签体内容 */}
                            <MyNavLink to="/about">About</MyNavLink>
                            <MyNavLink to="/home">Home</MyNavLink>
                        </div>
                    </div>
                    <div className="col-xs-6">
                        <div className="panel">
                            <div className="panel-body">
                                {/* 注册路由 */}
                                {/* 一定要注意component首字母小写 */}
                                <Route path='/about' component={About}/>
                                <Route path='/home' component={Home}/>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        )
    }
}

```

### 15.9 Switch

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104423.png)

防止注册路由的时候，一个路径对应多个组件，这多个组件都生效的情况。当用Switch标签将注册的所有路由包裹起来时，即使一个path对应着多个组件，那么永远只会对第一个生效

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104435.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104445.png)



App.js

（Switch组件要引入）

```js
import React, { Component } from 'react'
// Link、Route首字母大写，是组件
import {Route, Switch} from 'react-router-dom'

import About from './pages/About'
import Home from './pages/Home'
import Test from './pages/Test'
import Header from './component/Header'
import MyNavLink from './component/MyNavLink'

export default class App extends Component {
    render() {
        return (
            <div>
                <Header/>
                <div className="row">
                    <div className="col-xs-2 col-xs-offset-2">
                        <div className="list-group">
                            {/* 原生html中，靠a标签跳转到不用页面实现导航功能 */}
                            {/* <a className="list-group-item active" href="./about.html">About</a>
                            <a className="list-group-item" href="./home.html">Home</a> */}

                            {/* 在React路由中靠路由链接实现切换组件-------编写路由链接 */}
                            {/* 不是href了，而是to，并且to属性值为path，即 /组件名 这种形式的 */}
                            {/* 路由链接需要被包含在路由器里面，所以外侧需要包含一个Router标签，但是Router标签有两种：BrowserRouter、HashRouter，需要在其中选择一个。二者区别在 路由原理history 中有详解。一般路由器标签都直接放在组件使用的地方，直接包含整个组件 */}
                            {/* 在组件内部使用多个路由器标签，代表的是不同的路由器，互相不相干，所以一般直接将路由器标签放在组件被使用的地方，直接包含整个组件 */}
                            
                            {/* 不同的属性值，通过props传递。不同的标签体，也可以通过props传递，传递后的value为children，值为传递的标签体内容，在MyNavLink中可以通过this.props.children拿到传递的标签体内容 */}
                            <MyNavLink to="/about">About</MyNavLink>
                            <MyNavLink to="/home">Home</MyNavLink>
                        </div>
                    </div>
                    <div className="col-xs-6">
                        <div className="panel">
                            <div className="panel-body">
                                {/* 加了Switch，一个path对应多个组件时，只对第一个组件生效，不加的话就对两个都生效 */}
                                <Switch>
                                    {/* 注册路由 */}
                                    {/* 一定要注意component首字母小写 */}
                                    <Route path='/about/a/b' component={About}/>
                                    {/* 一个path对应两个组件 */}
                                    <Route path='/home' component={Home}/>
                                    <Route path='/home' component={Test}/>
                                </Switch>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        )
    }
}
```

### 15.10 解决样式丢失问题

localhost:3000是react脚手架内置的一个服务器，是由webpack、devServer开启的一个服务器。**它的主机号加端口号代表的就是React脚手架中的public文件夹**



**如果请求的资源是正确的，就返回相应资源。如果请求的资源不存在，就返回public中的index.html**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104458.png)

**当路由path为两级以上，刷新页面后样式丢失的原因：**

当路由的path路径为两级及两级以上时，如<Route path="/test1/test2/about" component={About}/>，浏览器在第一次进页面，请求bootstrap资源时的request url是正确的：http://localhost:3000/css/bootstrap.css，所以能成功请求到bootstrap样式。但是一旦将页面刷新后，浏览器请求bootstrap资源的地址就发生了变化，将path路径最后一级路径前的所有路径都加到了 bootstrap资源请求路径中端口号的后面：http://localhost:3000/test/test2/css/bootstrap.css，bootstrap路径不在此请求路径中，所以请求bootstrap样式失败，直接返回的是public中的index.html

第一次进页面：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104509.png)

刷新页面后：

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104520.png)



**解决方法：**

1. 去掉html中引入bootstrap库路径最前面的`.`     （将相对路径改为绝对路径）

   index.html

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta http-equiv="X-UA-Compatible" content="IE=edge">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <!-- ./css/bootstrap.css表示index.html同级目录下的css文件夹下的bootstrap.css文件 -->
       <!-- /css/bootstrap.css，去掉.表示：由于浏览器的localhost:3000表示的就是public，而localhost:3000又可以省略不写，所以/css/bootstrap.css表示直接在public文件夹下去找css/bootstrap.css，不会因为刷新页面而改变请求路径了 -->
       <link rel="stylesheet" href="/css/bootstrap.css"/>
       <style>
           /* bootstrap样式的权重很高，所以加上!important */
           .testNavLink{background-color:cadetblue !important};
       </style>
       <title>主页面</title>
   </head>
   <body>
       <div id="root"></div>
   </body>
   </html>
   ```

2. 用`%PUBLIC_URL%`代替`.` （只适合在react脚手架里面这么写，在其他地方没有这东西）    （将相对路径改为绝对路径）

   index.html

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta http-equiv="X-UA-Compatible" content="IE=edge">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <!-- 引入第三方样式：bootstrap -->
       <!-- %PUBLIC_URL%就表示public -->
       <link rel="stylesheet" href="%PUBLIC_URL%/css/bootstrap.css"/>
       <style>
           /* bootstrap样式的权重很高，所以加上!important */
           .testNavLink{background-color:cadetblue !important};
       </style>
       <title>主页面</title>
   </head>
   <body>
       <div id="root"></div>
   </body>
   </html>
   ```

3. 方法三（用的少）：用HashRouter，因为HashRouter会自动在浏览器地址后面加上#，#后面的不会作为请求资源。如果path为‘/test/test2/about’，点击链接后，浏览器地址会变成：http://localhost:3000/#/test/test2/about，#后面的不会作为请求资源路径，所以/test/test2/about不会存在于请求资源路径中，不会出现请求bootstrap资源的请求路径为：http://localhost:3000/#/test/test2/about这种情况。

   index.js

   ```js
   import React from 'react'
   import ReactDom from 'react-dom'
   // 记得要引入组件
   import {HashRouter} from 'react-router-dom'
   import App from './App'
   
   ReactDom.render(
       // 直接将路由器标签包含整个组件，那么组件中就不存在不同路由器标签互相不能访问的问题
       <HashRouter>
           <App/>
       </HashRouter>,
       document.getElementById("root")
       );
   ```

### 15.11 路由的模糊匹配和严格匹配

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104533.png)



路由默认是模糊匹配的，`exact="false"`，给的路径不少于要的路径，且顺序位置要对应上

情况一：给的路径少于要的路径，就不能匹配

​	<MyNavLink to="/about">About</MyNavLink>

​	<Route path='/about/a/a' component={About}/>

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104544.png)

情况二：给的路径多，要的路径少。能匹配，因为匹配是将一个个路径拎出来，about a b与about匹配，发现路由的path为about与给的路径：about a b中的第一个匹配成功

​	<MyNavLink to="/about/a/b">About</MyNavLink>

​	<Route path='/about/' component={About}/>

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104556.png)

情况三：给的路径与要的路径字段位置不匹配。不能匹配成功，因为匹配是按位置顺序匹配的，第一个没匹配成功就不会再继续匹配

​	<MyNavLink to="/a/about/b">About</MyNavLink>

​	<Route path='/about/' component={About}/>

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104614.png)



解决：开启严格匹配，在路由上加一个`exact属性`，默认为false，设置其为true即可开启严格匹配。

​	<MyNavLink to="/about/a/b">About</MyNavLink>

​	<Route exact path='/about/' component={About}/>

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104625.png)

**但是严格匹配不能随便开启。开启时机：模糊匹配出问题的时候再开启严格匹配。因为随意开启严格匹配有时候会出现问题，比如多级路由的时候（详见15.13嵌套路由）**

### 15.12 Redirect重定向

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104635.png)



当所有路由都匹配不上点击路由链接后给的路由地址时，可以使用重定向直接指定路由到哪里去，比如实现一进页面就显示的是About组件的内容：<Redirect to='/about'/>

App.js

```js
import React, { Component } from 'react'
// Link、Route首字母大写，是组件
import {Route, Redirect} from 'react-router-dom'

import About from './pages/About'
import Home from './pages/Home'
import Header from './component/Header'
import MyNavLink from './component/MyNavLink'

export default class App extends Component {
    render() {
        return (
            <div>
                <Header/>
                <div className="row">
                    <div className="col-xs-2 col-xs-offset-2">
                        <div className="list-group">
                            {/* 原生html中，靠a标签跳转到不用页面实现导航功能 */}
                            {/* <a className="list-group-item active" href="./about.html">About</a>
                            <a className="list-group-item" href="./home.html">Home</a> */}

                            {/* 在React路由中靠路由链接实现切换组件-------编写路由链接 */}
                            {/* 不是href了，而是to，并且to属性值为path，即 /组件名 这种形式的 */}
                            {/* 路由链接需要被包含在路由器里面，所以外侧需要包含一个Router标签，但是Router标签有两种：BrowserRouter、HashRouter，需要在其中选择一个。二者区别在 路由原理history 中有详解。一般路由器标签都直接放在组件使用的地方，直接包含整个组件 */}
                            {/* 在组件内部使用多个路由器标签，代表的是不同的路由器，互相不相干，所以一般直接将路由器标签放在组件被使用的地方，直接包含整个组件 */}
                            
                            {/* 不同的属性值，通过props传递。不同的标签体，也可以通过props传递，传递后的value为children，值为传递的标签体内容，在MyNavLink中可以通过this.props.children拿到传递的标签体内容 */}
                            <MyNavLink to="/about">About</MyNavLink>
                            <MyNavLink to="/home">Home</MyNavLink>
                        </div>
                    </div>
                    <div className="col-xs-6">
                        <div className="panel">
                            <div className="panel-body">
                                    {/* 注册路由 */}
                                    {/* 一定要注意component首字母小写 */}
                                    <Route path='/about' component={About}/>
                                    <Route path='/home' component={Home}/>
                                    <Redirect to='/about'/>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        )
    }
}
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104754.png)

### 15.13 嵌套路由

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104805.png)

- 子路由和子路由链接不需要用路由器标签包裹起来

- 路由链接匹配路由的顺序，是按照注册路由的顺序来的。每一个具体路由链接与路由的匹配规则是从左向右，一旦从左向右出现匹配不成功的path字段，就匹配失败

- 路由链接一旦执行重定向指定的路由，就不会再继续匹配后面的路由

- 子路由要写上父路由的path值的原因是：父路由先注册，子路由后注册，子路由中的路由链接匹配顺序就是从父路由的路由中开始，如果子路由没有加上父路由的path，那么子路由的路由链接上也应该没有父路由的path，那么子路由的路由链接从父路由中开始匹配的，就匹配不到任何父路由，那么就会走父组件的重定向，一旦走了父组件的重定向指定的路由，那么就不会再继续匹配后面的路由了，所以最终显示的是父组件的重定向指定的路由对应的组件，不会显示任何子路由对应的组件

  如果子路由的path值前面加上了父路由的path值，那么子路由链接匹配时，会匹配成功父路由，父路由对应的组件就会显示出来。子路由链接继续匹配，匹配到子路由那么子路由对应的组件也会显示出来达到了点击子路由链接时，既不丢弃父组件也会显示子组件

- 如果在父路由中开启了严格匹配，那么子路由链接按照注册顺序匹配时，匹配不到任何父路由，所以会走重定向，一旦走了重定向指定的路由，就不会再继续匹配后面的路由了，所以也匹配不到任何子路由

App.js

```js
import React, { Component } from 'react'
// Link、Route首字母大写，是组件
import {Route, Redirect, Switch} from 'react-router-dom'

import About from './pages/About'
import Home from './pages/Home'
import Header from './component/Header'
import MyNavLink from './component/MyNavLink'

export default class App extends Component {
    render() {
        return (
            <div>
                <Header/>
                <div className="row">
                    <div className="col-xs-2 col-xs-offset-2">
                        <div className="list-group">
                            {/* 原生html中，靠a标签跳转到不用页面实现导航功能 */}
                            {/* <a className="list-group-item active" href="./about.html">About</a>
                            <a className="list-group-item" href="./home.html">Home</a> */}

                            {/* 在React路由中靠路由链接实现切换组件-------编写路由链接 */}
                            {/* 不是href了，而是to，并且to属性值为path，即 /组件名 这种形式的 */}
                            {/* 路由链接需要被包含在路由器里面，所以外侧需要包含一个Router标签，但是Router标签有两种：BrowserRouter、HashRouter，需要在其中选择一个。二者区别在 路由原理history 中有详解。一般路由器标签都直接放在组件使用的地方，直接包含整个组件 */}
                            {/* 在组件内部使用多个路由器标签，代表的是不同的路由器，互相不相干，所以一般直接将路由器标签放在组件被使用的地方，直接包含整个组件 */}
                            
                            {/* 不同的属性值，通过props传递。不同的标签体，也可以通过props传递，传递后的value为children，值为传递的标签体内容，在MyNavLink中可以通过this.props.children拿到传递的标签体内容 */}
                            <MyNavLink to="/about">About</MyNavLink>
                            <MyNavLink to="/home">Home</MyNavLink>
                        </div>
                    </div>
                    <div className="col-xs-6">
                        <div className="panel">
                            <div className="panel-body">
                                <Switch>
                                    {/* 组件级别最高，所以是最先注册路由 */}
                                    {/* 一定要注意component首字母小写 */}
                                    <Route exact path='/about' component={About}/>
                                    <Route path='/home' component={Home}/>
                                    <Redirect to='/about'/>
                                </Switch>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        )
    }
}
```

Home.js

```js
import React, { Component } from 'react'
import {Route, Redirect, Switch} from 'react-router-dom'
import MyNavLink from '../../component/MyNavLink'
import Message from './Message'
import News from './News'

export default class Home extends Component {
    render() {
        return (
            <div>
                    <h2>Home组件内容</h2>
                    <div>
                        <ul className="nav nav-tabs">
                        <li>
                            {/* 先匹配父组件中先注册的路由，to="/home/news"能和path="/home"匹配上，所以能显示Home组件的内容，再继续匹配到子组件中的路由，能和path="/home/news"匹配上，所以也能显示News组件的内容 */}
                            <MyNavLink to="/home/news">News</MyNavLink>
                        </li>
                        <li>
                            <MyNavLink to="/home/message">Message</MyNavLink>
                        </li>
                        </ul>
                        <Switch>
                            {/* 组件级别其次，第二注册的路由 */}
                            <Route path="/home/news" component={News}/>
                            <Route path="/home/message" component={Message}/>
                            <Redirect to="/home/news"/>
                        </Switch>
                    </div>
            </div>
        )
    }
}
```

News.js

```js
import React, { Component } from 'react'

export default class News extends Component {
    render() {
        return (
            <ul>
                <li>news001</li>
                <li>news002</li>
                <li>news003</li>
            </ul>
        )
    }
}
```

Message.js

```js
import React, { Component } from 'react'

export default class Message extends Component {
    render() {
        return (
            <ul>
                <li>
                    {/* 浏览器根据此请求路径找不到相关资源，会默认显示public下的index.html */}
                    <a href="/message1">message001</a>&nbsp;&nbsp;
                </li>
                <li>
                    <a href="/message2">message002</a>&nbsp;&nbsp;
                </li>
                <li>
                    <a href="/message/3">message003</a>&nbsp;&nbsp;
                </li>
            </ul>
        )
    }
}
```

### 15.14 向路由组件传递参数params数据

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104820.png)

1. 在编写路由链接时，**传递参数**，格式：路由链接的路由path/参数值1/参数值2
2. 在注册路由时，**声明接收**参数，格式：路由path/参数1/参数2
3. **在路由对应组件中接收参数：**/:参数名1/:参数名2，传递的参数以props的形式传递给路由对应的组件，参数都放在props对象中的match对象中的params对象中

Message.js

```js
import React, { Component } from 'react'
import {Link, Route} from 'react-router-dom'
import Detail from './Detail'

export default class Message extends Component {
    state={messageArr:[
        {id:1,title:"消息1"},
        {id:2,title:"消息2"},
        {id:3,title:"消息3"}
    ]}
    render() {
        const {messageArr}=this.state
        return (
            <div>
                <ul>
                    {
                        messageArr.map(item=>{
                            // 带上祖先路由的path
                            return (
                                <li key={item.id}>
                                    {/* 向路由组件传递params参数 */}
                                    {/* Link是标签，而反引号``属于js的语法，所以要在外侧包一个{} */}
                                    <Link to={`/home/message/detail/${item.id}/${item.title}`}>{item.title}</Link>
                                </li>
                            )
                        })
                    }
                </ul>
                {/* 声明接收params参数，参数以props传递到路由对应的组件中了,参数存放在props中的match对象中的params对象中，参数以key value键值对的形式存放 */}
                <Route path="/home/message/detail/:id/:title" component={Detail}/>
            </div>
        )
    }
}
```

Detail.js

```js
import React, { Component } from 'react'

const detailMsg=[
    {id:1,context:"你是猪"},
    {id:2,context:"天天就知道睡觉"},
    {id:3,context:"就知道吃饭"}
]
export default class Detail extends Component {
    render() {
        const {id,title}=this.props.match.params;
        // find得到的是满足条件的那个数组项，而filter得到的是将满足条件的数组项封装成的那个数组
        const matchMsg=detailMsg.find(item=>{
            return item.id==id
        })
        return (
            <div>
                <ul>
                    <li>ID:{id}</li>
                    <li>TITLE:{title}</li>
                    <li>CONTEXT:{matchMsg.context}</li>
                </ul>
            </div>
        )
    }
}
```

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104836.png)

### 15.15 向路由器传递参数search数据

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104846.png)

1. **传递参数**，以?开始，每个参数之间用&连接：路由链接path路径/?参数1=参数值&参数2=参数值
2. **声明接收参数**，在注册路由时不需要写任何接收参数，正常注册路由即可
3. **路由对应组件中接收参数**，接收参数从this.props.location.search中拿，参数类型为urlencoded编码的字符串

Message.js

```js
import React, { Component } from 'react'
import {Link, Route} from 'react-router-dom'
import Detail from './Detail'

export default class Message extends Component {
    state={messageArr:[
        {id:1,title:"消息1"},
        {id:2,title:"消息2"},
        {id:3,title:"消息3"}
    ]}
    render() {
        const {messageArr}=this.state
        return (
            <div>
                <ul>
                    {
                        messageArr.map(item=>{
                            // 带上祖先路由的path
                            return (
                                <li key={item.id}>
                                    {/* 向路由组件传递params参数 */}
                                    {/* Link是标签，而反引号``属于js的语法，所以要在外侧包一个{} */}
                                    {/* <Link to={`/home/message/detail/${item.id}/${item.title}`}>{item.title}</Link> */}

                                    {/* 向路由组件传递search参数,以?开始，每个参数之间用&连接 */}
                                    <Link to={`/home/message/detail/?id=${item.id}&title=${item.title}`}>{item.title}</Link>
                                </li>
                            )
                        })
                    }
                </ul>
                {/* 声明接收params参数，参数以props传递到路由对应的组件中了,参数存放在props中的match对象中的params对象中，参数以key value键值对的形式存放 */}
                {/* <Route path="/home/message/detail/:id/:title" component={Detail}/> */}

                {/* 声明接收search参数，search参数不需要在注册路由时声明接收，正常注册路由即可。且参数以urlencoded编码方式存放在props的localhost的search中:  参数1:参数值&参数2:参数值 */}
                <Route path="/home/message/detail" component={Detail}/>
            </div>
        )
    }
}
```

Detail.js

```js
import React, { Component } from 'react'
import qs from 'querystring'

const detailMsg=[
    {id:1,context:"你是猪"},
    {id:2,context:"天天就知道睡觉"},
    {id:3,context:"就知道吃饭"}
]
export default class Detail extends Component {
    render() {
        // 向路由传递params参数时，接收参数从props.match.params中拿，参数类型为键值对
        // const {id,title}=this.props.match.params;

        // 向路由传递params参数时，接收参数从props.location.searc中拿，参数类型为urlencoded编码的字符串
        const {search}=this.props.location
        // 从字符串1位开始截取字符串，去掉参数字符串首部的?号
        const detail = search.substring(1)
        // 将urlencoded编码转换为键值对对象形式
        const {id,title}=qs.parse(detail)          

        // find得到的是满足条件的那个数组项，而filter得到的是将满足条件的数组项封装成的那个数组
        const matchMsg=detailMsg.find(item=>{
            return item.id==id
        })

        return (
            <div>
                <ul>
                    <li>ID:{id}</li>
                    <li>TITLE:{title}</li>
                    <li>CONTEXT:{matchMsg.context}</li>
                </ul>
            </div>
        )
    }
}
```

### 15.16 向路由组件传递state参数

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104900.png)

1. **传递参数**，向路由组件传递state参数,to值必须为一个对象，每个属性格式为键值对,state属性值也必须为一个对象，参数格式为键值对
2. **声明接收参数**，state参数不需要在注册路由时声明接收，正常注册路由即可。且参数以key value存放在props的localhost的state中
3. **路由对应组件中接收参数**，接收参数从props.location.state中拿，参数类型为键值对

- state传参不会在浏览器地址栏显示出来，刷新页面，参数不会丢，因为BrowserRouter是一直操作history对象的，history对象中也存放了一个location对象，所以state数据会一直保存在history中，除非清空浏览器缓存，所以刷新页面不会导致state数据的丢失

Message.js

```js
import React, { Component } from 'react'
import {Link, Route} from 'react-router-dom'
import Detail from './Detail'

export default class Message extends Component {
    state={messageArr:[
        {id:1,title:"消息1"},
        {id:2,title:"消息2"},
        {id:3,title:"消息3"}
    ]}
    render() {
        const {messageArr}=this.state
        return (
            <div>
                <ul>
                    {
                        messageArr.map(item=>{
                            // 带上祖先路由的path
                            return (
                                <li key={item.id}>
                                    {/* 向路由组件传递params参数 */}
                                    {/* Link是标签，而反引号``属于js的语法，所以要在外侧包一个{} */}
                                    {/* <Link to={`/home/message/detail/${item.id}/${item.title}`}>{item.title}</Link> */}

                                    {/* 向路由组件传递search参数,以?开始，每个参数之间用&连接 */}
                                    {/* <Link to={`/home/message/detail/?id=${item.id}&title=${item.title}`}>{item.title}</Link> */}

                                    {/* 向路由组件传递state参数,to值必须为一个对象，每个属性格式为键值对,state属性值也必须为一个对象，参数格式为键值对*/}
                                    {/* 最外侧括号表示html中写的是js代码，里侧的括号表示to属性值为一个对象 */}
                                    <Link to={{pathname:'/home/message/detail',state:{id:item.id,title:item.title}}}>{item.title}</Link>
                                </li>
                            )
                        })
                    }
                </ul>
                {/* 声明接收params参数，参数以props传递到路由对应的组件中了,参数存放在props中的match对象中的params对象中，参数以key value键值对的形式存放 */}
                {/* <Route path="/home/message/detail/:id/:title" component={Detail}/> */}

                {/* 声明接收search参数，search参数不需要在注册路由时声明接收，正常注册路由即可。且参数以urlencoded编码方式存放在props的localhost的search中:  参数1:参数值&参数2:参数值 */}
                {/* <Route path="/home/message/detail" component={Detail}/> */}

                 {/* 声明接收state参数，state参数不需要在注册路由时声明接收，正常注册路由即可。且参数以key value存放在props的localhost的state中 */}
                 <Route path="/home/message/detail" component={Detail}/>
            </div>
        )
    }
}
```

Detail.js

```js
import React, { Component } from 'react'
import qs from 'querystring'

const detailMsg=[
    {id:1,context:"你是猪"},
    {id:2,context:"天天就知道睡觉"},
    {id:3,context:"就知道吃饭"}
]
export default class Detail extends Component {
    render() {
        // 向路由传递params参数时，接收参数从props.match.params中拿，参数类型为键值对
        // const {id,title}=this.props.match.params;

        // 向路由传递search参数时，接收参数从props.location.search中拿，参数类型为urlencoded编码的字符串
        // const {search}=this.props.location
        // // 从字符串1位开始截取字符串，去掉参数字符串首部的?号
        // const detail = search.substring(1)
        // // 将urlencoded编码转换为键值对对象形式
        // const {id,title}=qs.parse(detail)

        // 向路由传递state参数时，接收参数从props.location.state中拿，参数类型为键值对
        // 如果路由没传state参数，那么this.props.location.state就为默认值undefined，从undefined中取值就会报错，所以当没传state参数时，让其为空对象，而不是默认值undefined
        const {id,title}=this.props.location.state||{};

        // find得到的是满足条件的那个数组项，而filter得到的是将满足条件的数组项封装成的那个数组
        // 如果this.props.location.state没有id属性，id为undefined，detailMsg中没有id值与undefined相同，所以matchMsg为undefined，那么下面matchMsg.context就会报错，所以当没有值与state中的id相同时，让matchMsg为空对象
        const matchMsg=detailMsg.find(item=>{
            return item.id==id
        })||{}

        return (
            <div>
                <ul>
                    <li>ID:{id}</li>
                    <li>TITLE:{title}</li>
                    <li>CONTEXT:{matchMsg.context}</li>
                </ul>
            </div>
        )
    }
}
```

### 15.17push与replace

push:

- 压栈的方式，当前组件路径永远在history栈顶
- 所有的都会留下记录，都能返回和前进

replace：开启方法是在编写路由链接时设置replace属性为true

- 替换掉栈顶路由路径，不会留下历史记录

```js
<Link replace to={{pathname:'/home/message/detail',state:{id:item.id,title:item.title}}}>{item.title}</Link>
```

Message.js

```js
import React, { Component } from 'react'
import {Link, Route} from 'react-router-dom'
import Detail from './Detail'

export default class Message extends Component {
    state={messageArr:[
        {id:1,title:"消息1"},
        {id:2,title:"消息2"},
        {id:3,title:"消息3"}
    ]}
    render() {
        const {messageArr}=this.state
        return (
            <div>
                <ul>
                    {
                        messageArr.map(item=>{
                            // 带上祖先路由的path
                            return (
                                <li key={item.id}>
                                    {/* 向路由组件传递params参数 */}
                                    {/* Link是标签，而反引号``属于js的语法，所以要在外侧包一个{} */}
                                    {/* replace开启替换history栈顶的路由路径，每点击消息链接，会替换掉/home/message，再点击就会替换掉刚刚的消息链接路由路径，所以点击返回，返回的是/home/news */}
                                    <Link replace to={`/home/message/detail/${item.id}/${item.title}`}>{item.title}</Link>

                                    {/* 向路由组件传递search参数,以?开始，每个参数之间用&连接 */}
                                    {/* <Link to={`/home/message/detail/?id=${item.id}&title=${item.title}`}>{item.title}</Link> */}

                                    {/* 向路由组件传递state参数,to值必须为一个对象，每个属性格式为键值对,state属性值也必须为一个对象，参数格式为键值对*/}
                                    {/* 最外侧括号表示html中写的是js代码，里侧的括号表示to属性值为一个对象 */}
                                    {/* <Link replace to={{pathname:'/home/message/detail',state:{id:item.id,title:item.title}}}>{item.title}</Link> */}
                                </li>
                            )
                        })
                    }
                </ul>
                {/* 声明接收params参数，参数以props传递到路由对应的组件中了,参数存放在props中的match对象中的params对象中，参数以key value键值对的形式存放 */}
                <Route path="/home/message/detail/:id/:title" component={Detail}/>

                {/* 声明接收search参数，search参数不需要在注册路由时声明接收，正常注册路由即可。且参数以urlencoded编码方式存放在props的localhost的search中:  参数1:参数值&参数2:参数值 */}
                {/* <Route path="/home/message/detail" component={Detail}/> */}

                 {/* 声明接收state参数，state参数不需要在注册路由时声明接收，正常注册路由即可。且参数以key value存放在props的localhost的state中 */}
                 {/* <Route path="/home/message/detail" component={Detail}/> */}
            </div>
        )
    }
}
```

### 15.18 编程式路由导航

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104924.png)

实现不点击链接也能跳转路由，即不用Link、NavLink，比如点击某个按钮实现跳转路由：利用**路由组件中的history对象**中的push方法实现push形式的路由跳转，replace方法实现replace形式的跳转

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104937.png)

go：参数为nubmer，表示前进几步，可以为负值，负值表示后退

goBack：后退一步

goForward：前进一步

push：以push形式路由

replace：以replace形式路由



![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002104950.png)

pathname、path、url都可以获取当前组件所处的路由路径



Message.js

```js
import React, { Component } from 'react'
import {Link, Route} from 'react-router-dom'
import Detail from './Detail'

export default class Message extends Component {
    state={messageArr:[
        {id:1,title:"消息1"},
        {id:2,title:"消息2"},
        {id:3,title:"消息3"}
    ]}

    pushShow=(id,title)=>{
        // push跳转+携带params参数
        // this.props.history.push(`/home/message/detail/${id}/${title}`)

        // push跳转+携带search参数
        // this.props.history.push(`/home/message/detail/?id=${id}&title=${title}`)

        // push跳转+携带state参数
        // push可以携带两个参数：path,state
        this.props.history.push('/home/message/detail',{id,title})
    }

    replaceShow=(id,title)=>{
        // replace跳转+携带params参数
        // this.props.history.replace(`/home/message/detail/${id}/${title}`)

        // replace跳转+携带search参数
        // this.props.history.replace(`/home/message/detail/?id=${id}&title=${title}`)

        // replace跳转+携带state参数
        // replace可以携带两个参数：path,state
        this.props.history.replace('/home/message/detail',{id,title})
    }

    back=()=>{
        this.props.history.goBack()
    }

    forward=()=>{
        this.props.history.goForward()
    }

    render() {
        const {messageArr}=this.state
        return (
            <div>
                <ul>
                    {
                        messageArr.map(item=>{
                            // 带上祖先路由的path
                            return (
                                <li key={item.id}>
                                    {/* 向路由组件传递params参数 */}
                                    {/* Link是标签，而反引号``属于js的语法，所以要在外侧包一个{} */}
                                    {/* replace开启替换history栈顶的路由路径，每点击消息链接，会替换掉/home/message，再点击就会替换掉刚刚的消息链接路由路径，所以点击返回，返回的是/home/news */}
                                    <Link replace to={`/home/message/detail/${item.id}/${item.title}`}>{item.title}</Link>

                                    {/* 向路由组件传递search参数,以?开始，每个参数之间用&连接 */}
                                    {/* <Link to={`/home/message/detail/?id=${item.id}&title=${item.title}`}>{item.title}</Link> */}

                                    {/* 向路由组件传递state参数,to值必须为一个对象，每个属性格式为键值对,state属性值也必须为一个对象，参数格式为键值对*/}
                                    {/* 最外侧括号表示html中写的是js代码，里侧的括号表示to属性值为一个对象 */}
                                    {/* <Link replace to={{pathname:'/home/message/detail',state:{id:item.id,title:item.title}}}>{item.title}</Link> */}

                                    <button onClick={()=>{this.pushShow(item.id,item.title)}}>push跳转</button>
                                    <button onClick={()=>{this.replaceShow(item.id,item.title)}}>replace跳转</button>    
                            
                                </li>
                            )
                        })
                    }
                </ul>
                {/* 声明接收params参数，参数以props传递到路由对应的组件中了,参数存放在props中的match对象中的params对象中，参数以key value键值对的形式存放 */}
                {/* <Route path="/home/message/detail/:id/:title" component={Detail}/> */}

                {/* 声明接收search参数，search参数不需要在注册路由时声明接收，正常注册路由即可。且参数以urlencoded编码方式存放在props的localhost的search中:  参数1:参数值&参数2:参数值 */}
                {/* <Route path="/home/message/detail" component={Detail}/> */}

                 {/* 声明接收state参数，state参数不需要在注册路由时声明接收，正常注册路由即可。且参数以key value存放在props的localhost的state中 */}
                 <Route path="/home/message/detail" component={Detail}/>

                 <button onClick={this.back}>回退</button>
                 <button onClick={this.forward}>前进</button>
            </div>
        )
    }
}
```

Detail.js

```js
import React, { Component } from 'react'
import qs from 'querystring'

const detailMsg=[
    {id:1,context:"你是猪"},
    {id:2,context:"天天就知道睡觉"},
    {id:3,context:"就知道吃饭"}
]
export default class Detail extends Component {
    render() {
        // 向路由传递params参数时，接收参数从props.match.params中拿，参数类型为键值对
        // const {id,title}=this.props.match.params;

        // 向路由传递search参数时，接收参数从props.location.search中拿，参数类型为urlencoded编码的字符串
        // const {search}=this.props.location
        // // 从字符串1位开始截取字符串，去掉参数字符串首部的?号
        // const detail = search.substring(1)
        // // 将urlencoded编码转换为键值对对象形式
        // const {id,title}=qs.parse(detail)

        // 向路由传递state参数时，接收参数从props.location.state中拿，参数类型为键值对
        // 如果路由没传state参数，那么this.props.location.state就为默认值undefined，从undefined中取值就会报错，所以当没传state参数时，让其为空对象，而不是默认值undefined
        const {id,title}=this.props.location.state||{};

        // find得到的是满足条件的那个数组项，而filter得到的是将满足条件的数组项封装成的那个数组
        // 如果this.props.location.state没有id属性，id为undefined，detailMsg中没有id值与undefined相同，所以matchMsg为undefined，那么下面matchMsg.context就会报错，所以当没有值与state中的id相同时，让matchMsg为空对象
        const matchMsg=detailMsg.find(item=>{
            return item.id==id
        })||{}

        return (
            <div>
                <ul>
                    <li>ID:{id}</li>
                    <li>TITLE:{title}</li>
                    <li>CONTEXT:{matchMsg.context}</li>
                </ul>
            </div>
        )
    }
}
```

### 15.19 withRouter使用

是react-router-dom中的一个方法，需要引入

withRouter作用：可以让一般组件也能使用路由组件的api，比如this.props自动有history、location、match属性

现在Header组件内也加上回退、前进、go按钮

Header.js

```js
import React, { Component } from 'react'
import {withRouter} from 'react-router-dom'

class Header extends Component {
    back=()=>{
        this.props.history.goBack()
    }

    forward=()=>{
        this.props.history.goForward()
    }

    go=()=>{
        this.props.history.go(2)
    }

    render() {
        return (
                <div className="col-xs-offset-2 col-xs-8">
                    <div className="page-header"><h2>React Router Demo</h2></div>
                    <button onClick={this.back}>回退</button>
                    <button onClick={this.forward}>前进</button>
                    <button onClick={this.go}>go</button>
                </div>
        )
    }
}

// 默认暴露出withRouter(Header)的返回值，withRouter能让一般组件也能使用路由组件的api，返回值为能使用路由组件api的新的一般组件
export default withRouter(Header)
```

### 15.20 BrowserRouter和HashRouter的区别

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105007.png)

在向路由组件传递state参数时，由于这种传参方式不会在浏览器地址栏中显示出来参数，而HashRputer靠的就是地址栏后面的哈希值，所以使用HashRouter时，会造成参数的丢失。BrowserRouter使用的是浏览器BOM内置的history对象，与this.props.history对象不一样，所以刷新页面不影响路由携带的参数，因为state保存在了浏览器内置history栈中。而HashRouter没有使用浏览器内置history，它使用的是url的哈希值，由于state参数不会在地址栏显示出来，所以没有人帮它记住历史path，所以刷新页面会造成路由参数丢失

所以BrowserRouter用的多，但是HashRouter的兼容性比较好

## 16. antd

### 16.1 antd的基本使用

- ant-design：第三方组件库

- 需要安装：npm install antd

- 引入库：import antd from 'antd'，如果from后面只有一个包名，那么会默认去node_modules中找

- 需要引入antd包下的dist文件夹中的antd.css样式，样式才有效：import antdcss from 'antd/dist/antd.css'

  App.js

  ```js
  import React, { Component } from 'react'
  
  import antd, { Button } from 'antd'
  // 需要引入antd样式，样式才有效
  import antdcss from 'antd/dist/antd.css'
  
  export default class App extends Component {
      render() {
          return (
              <div>
                  <button>普通按钮</button>
                  <Button type="primary">主要按钮</Button>
              </div>
          )
      }
  }
  ```

  ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105020.png)


- 有很多子库，安装antd库的时候就已经安装了子库，但是使用的时候需要分别引入，因为子库中的内容，antd主库中没有。比如：如果需要使用antd中的图标，就需要引入@ant-design/icons库，针对不同图标对应的不同组件，从@ant-design/icons库中引入

  ```js
  import React, { Component } from 'react'
  
  import antd, { Button } from 'antd'
  // 需要引入antd样式，样式才有效
  import antdcss from 'antd/dist/antd.css'
  // 引入antd的子库：图标库，需引入图标库中图标对应的组件
  import {RedditOutlined} from '@ant-design/icons'
  
  export default class App extends Component {
      render() {
          return (
              <div>
                  <button>普通按钮</button>
                  <Button type="primary">主要按钮</Button>
                  <RedditOutlined />
              </div>
          )
      }
  }
  ```

  ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105035.png)

### 16.2 antd样式的按需引入

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105045.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105054.png)

  直接引入@ant-design/icons表示引入所有antd组件的样式，文件会很大，而且我们用到的只有那么几个组件，其他组件样式引入后根本没用。



**按需引入样式的方法：修改react配置**

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105106.png)

方法一：可以使用 create-react-app 提供的**npm run eject**命令将所有react内建的配置暴露出来。这种方法非常不推荐使用，因为一旦暴露出来，不小心修改错了，那么会导致项目都运行不起来，而且是不可恢复的

执行命令后react脚手架会出现一个config文件夹，里面存放着配置文件

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105113.png)

方法二：

1. 将要修改的内容单独放在一个文件中，这个文件一般是在根目录中创建一个config-overrides.js文件，用于修改默认配置

   ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105127.png)

2. npm add react-app-rewired customize-cra：安装react-app-rewired    customize-cra这两个包， customize-cra包用于找到craco.config.js配置文件，并执行。react-app-rewired包用于启动项目，因为用customize-cra修改配置时，会导致用原来的react-scripts start命令不能启动脚手架，所以需要安装这个包去启动项目

3. npm start是个短命令，原命令是：react-scripts start。用customize-cra修改配置后，原来的react-scripts start不能启动项目了，所以需要在package.json中修改启动命令，用刚下载的react-app-rewired去启动项目

   修改start、build、test的命令

   ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105139.png)

4. npm add babel-plugin-import：安装babel-plugin-import插件，这个插件支持样式按需引入

5. 修改config-overrides.js文件内容

   ```json
   // 修改默认配置
   const { override, fixBabelImports } = require('customize-cra');
   
   // import表示引入，antd表示引入的是antd的东西，es表示用了es规范，css表示引入的是样式
   module.exports = override(
     fixBabelImports('import', {
       libraryName: 'antd',
       libraryDirectory: 'es',
       style: 'css',
     }),
   );
   ```

   做完了以上5步后，在写代码的时候就可以不用引入import antdcss from 'antd/dist/antd.css'样式了

   

   出现问题：修改package.json文件react-app-rewired后，用npm start发现启动不了项目。

   解决方法：

   https://www.cnblogs.com/tu-0718/p/12541841.html

   https://www.cnblogs.com/qianxiaox/p/13830977.html

### 16.3 antd自定义主题

antd底层样式全是用less写的，react会自动将less编译成css，如果你要修改antd组件的样式，**需要安装less 和 less-loader**，antd组件样式底层是用less写的，要改就要先安装less，由于需要将less编译为css，所以需要安装less-loader。之前不用安装是因为之前底层已经自动编译好了。

例子：修改底层主题颜色，antd的主题颜色是蓝色

1. 安装less 和 less-loader：npm 

2. 修改config-overrides.js，将style:'css'，修改为style:true，因为现在不是将css按需引入，而是将修改后的less，再编译成css按需引入

   ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105203.png)

3. 引入addLessLoader

   ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105214.png)

4. 添加代码

   ```json
   // javascriptEnabled表示能用js修改底层less样式文件
   // 这里不能用modifyVars去修改主题样式了，因为现在less-loader更新了，不再用modifyVars去修改主题样式了，而是用lessOptions对象
   // addLessLoader:修改antd底层样式的方法
   addLessLoader({
       // 坑：如果你使用的less-loader是5.0.0版本的，就不要加lessOptions，直接写里面两个属性
       lessOptions:{
         javascriptEnabled: true,
         modifyVars: { '@primary-color': 'green' },
       }
     }),
   ```

报错1：addlessloader is not defined，可能是因为你使用的less-loader是5.0.0版本，要去掉lessOptions，直接配置里面的选项

报错2：this.getOptions is not a function，less-loader版本过高，降低版本，建议不要降到5.0.0版本，因为5.0.0版本不能加lessOption

- npm uninstall less-loader
- npm install less-loader@6.0.0



**坑：如果你使用的less-loader是5.0.0版本的，就不要加lessOptions，直接配置里面两个属性**

## 17. redux

### 17.1 redux简介

1. 英文文档: https://redux.js.org/

2. 中文文档: http://www.redux.org.cn/

3. Github: https://github.com/reactjs/redux

   

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105229.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105238.png)

### 17.2 redux工作流程

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105248.png)

**React Components**：提出要对redux里面的状态进行某些动作，比如想对redux中的状态数据进行+运算

**Action Creators**：当React Components要对redux里面的状态数据操作时，将要做的动作（如：加法运算）封装在一个对象action对象中，对象中包含两个属性：

1. type：动作类型，比如+。
2. 2.data：数据，比如1。

**dispatch**函数：将参数action对象分发给Store去做，存在于store对象中

**Store**：是一个指挥者，负责将收到的动作对象交给Reducers去做，并获取到完成后的结果

**Reducers**：

- 本质是一个函数：

  ​	接收参数：preState、action对象

  ​	返回值：newState

1. 初始化状态。由于加工状态的时候，需要知道加工前的状态--------previousState，以及要进行加工的动作-------action对象，比如：+1这个动作，需要知道之前一次的数是多少，才能进行+1运算。

   但是第一次加工的时候，状态并没有初始值，所以Reducers还能初始化状态。当previousState没有值时，那么它的初始值为undefined。

   

   当为初始化动作时，action对象中，preState为undefined，type内置为：**@@init+随机生成的字符串**，（后面加上随机生成的字符串，是为了防止初始化的type，和加工状态的type重名）。data：没有任何值。在reduce.js中指定状态初始值为多少

- 当为初始化状态时，reduce中的方法是由store自动调用的

- 当为加工状态时，reduce里面的方法是当在组件中使用store.dispatch(action)方法后，再调用的

2. 加工状态，能修改redux里面的状态，先获取上一次的状态，再对上一次的状态进行加工动作,得到结果newState，将结果返回给Store，React Components通过getState得到新状态

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105301.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105310.png)

### 17.3 求和案例

#### 1. 纯react版

状态放在组件自身上

Count.js

```js
import React, { Component } from 'react'

export default class Count extends Component {
    state={count:0}

    increment = () =>{
        const {value} = this.selectNumNode;
        const {count} = this.state;
        // value是一个字符串，所以需要-0转为数字再进行运算
        this.setState({count:count+(value-0)});
    }

    decrement = () =>{
        const {value} = this.selectNumNode;
        const {count} = this.state;
        this.setState({count:count-(value-0)});
    }

    incrementIfOdd = () =>{
        const {value} = this.selectNumNode;
        const {count} = this.state;
        if(count%2 !== 0){
            this.setState({count:count+(value-0)});
        }
    }

    incrementAsync = () =>{
        const {value} = this.selectNumNode;
        const {count} = this.state;
        // 每次点击按钮后，过2秒再执行这个函数
        setTimeout(()=>{
            this.setState({count:count+(value-0)});
        },2000);
    }
    render() {
        const {count} = this.state;
        return (
            <div>
                <h2>当前求和为：{count}</h2>
                {/* 获取选中的结点 */}
                <select ref={c=>{this.selectNumNode=c}}>
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                </select>&nbsp;
                <button onClick={this.increment}>+</button>&nbsp;
                <button onClick={this.decrement}>-</button>&nbsp;
                <button onClick={this.incrementIfOdd}>当前求和为奇数时再加</button>&nbsp;
                <button onClick={this.incrementAsync}>异步加</button>
            </div>
        )
    }
}
```

#### 2. 精简版的redux

即不用Action Creators创建action对象

做一个计算器

（1）去除Count组件自身的状态（如果只有Count组件自己用的或者被很少组件需要的状态，就直接放在组件内部）

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105718.png)

1. 安装redux：npm i redux

2. 创建一个redux文件夹，存放唯一的store.js文件和reducer.js，当一个组件需要用到redux中的状态时，就会为这个组件单独建一个reducer，所以最好命名为：一看就知道是哪个组件的名字。比如Count组件的reducer，可以命名为count_reducer

   ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105328.png)

3. store.js

   ```js
   /**
    * 该文件专门用于暴露一个store对象，整个应用只有一个store对象
    */
   
   // 引入创建store对象的方法
   import { createStore } from "redux";
   // 引入为Count组件服务的reducer对象
   import countReducer from './count_reducer'
   
   // 创建store的同时，指定为store做事的reducer
   // 并默认暴露
   export default createStore(countReducer)
   ```

4. count_reducer.js

   注意：
   
   - redux中的state初始化是由store自动调用的
   
   - redux只负责管理state，加工和初始化state，不负责更新页面渲染，不能像setState一样，一改变state就自动重新渲染页面。
   
     解决：用store.ubscribe去监听reducer中的状态，一旦有状态改变，那么就自己去调用render重新渲染组件。自己调用render的方法有：1.this.setState({})，改变state时，不改变任何东西，传递一个空对象，那么react也会重新调用render  2.用forceUpdate不修改状态也能强制更新组件：this.forceUpdate();
   
   - 结合17.2来看
   
   ```js
   /**
    * 该组件是创建一个为Count组件服务的reducer
    */
   
   // 初始化方法一：
   const initState=0;
   // 
   // 如果type为初始化，那么就初始化state为initState，下一次由组件触发函数发生时，获取到的参数preState就为初始化state值，所以可以直接给preState赋initState值
   // 参数中赋值表示如果preState为undefined，那么就会被赋值为initState，如果preState有值，那么就不会执行preState=initState
   export default function countReducer(preState=initState,action){
       const {type,data}=action;
       console.log(preState);
       // 使用switch直接按条件返回结果，由于当初始化时给preState设置了初始化state值，所以初始化时直接返回preState
       switch (type) {
           case 'increment':
               return preState+data;
           case 'decrement':
               return preState-data;
           default:
               return preState;
       }
   }
   
   // // 初始化方法二：
   // // 如果type为初始化，那么就初始化state为initState，下一次由组件触发函数发生时，获取到的参数preState就为初始化state值，所以可以直接给preState赋initState值
   // // 参数中赋值表示如果preState为undefined，那么就会被赋值为initState，如果preState有值，那么就不会执行preState=initState
   // export default function countReducer(preState,action){
   //     if(preState===undefined)
   //     // 设置你想初始化的值
   //     preState=0;
   //     const {type,data}=action;
   //     // 使用switch直接按条件返回结果，由于当初始化时给preState设置了初始化state值，所以初始化时直接返回preState
   //     switch (type) {
   //         case 'increment':
   //             return preState+data;
   //         case 'decrement':
   //             return preState+data;
   //         default:
   //             return preState;
   //     }
   // }
   
   // // 初始化方法三：
   // // 
   // // 如果type为初始化，那么就初始化state为initState，下一次由组件触发函数发生时，获取到的参数preState就为初始化state值，所以可以直接给preState赋initState值
   // // 参数中赋值表示如果preState为undefined，那么就会被赋值为initState，如果preState有值，那么就不会执行preState=initState
   // export default function countReducer(preState,action){
   //     const {type,data}=action;
   //     // 使用switch直接按条件返回结果，由于当初始化时给preState设置了初始化state值，所以初始化时直接返回preState
   //     switch (type) {
   //         case 'increment':
   //             return preState+data;
   //         case 'decrement':
   //             return preState+data;
   //         default:
   //             return 0;
   //     }
   //     // 不是increment也不是decrement，那就是初始化，所以直接返回要初始化的值，这里写的是0
   // }
   ```
   
5. Count.js

   store三大方法：

   - getState()，获取状态
   - dispatch({type:xxx,data:xxx})，参数为action对象
   - subScribe(()=>{})，一旦reducer中的状态有变化，就会触发里面的回调函数
   
   ```js
   import React, { Component } from 'react'
   import store from '../../redux/store';
   
   export default class Count extends Component {
   
       componentDidMount=()=>{
           // 在组件挂载完毕时，监听reducer中的状态变化，一旦有状态改变就会调用里面的回调
           store.subscribe(()=>{
               // 在不修改该组件自身state的情况下，手动更新组件
               this.setState({});
               // this.forceUpdate();
           })
       }
   
       increment = () =>{
           const {value} = this.selectNumNode;
           // value是一个字符串，所以需要-0转为数字再进行运算
           store.dispatch({type:"increment",data:(value-0)});
       }
   
       decrement = () =>{
           const {value} = this.selectNumNode;
           store.dispatch({type:"decrement",data:(value-0)});
       }
   
       incrementIfOdd = () =>{
           const {value} = this.selectNumNode;
           if(store.getState()%2 !== 0){
               store.dispatch({type:"increment",data:(value-0)});
           }
       }
   
       incrementAsync = () =>{
           const {value} = this.selectNumNode;
           // 每次点击按钮后，过2秒再执行这个函数
           setTimeout(()=>{
               store.dispatch({type:"increment",data:(value-0)});
           },2000);
       }
       render() {
           return (
               <div>
                   <h2>当前求和为：{store.getState()}</h2>
                   {/* 获取选中的结点 */}
                   <select ref={c=>{this.selectNumNode=c}}>
                       <option value="1">1</option>
                       <option value="2">2</option>
                       <option value="3">3</option>
                   </select>&nbsp;
                   <button onClick={this.increment}>+</button>&nbsp;
                   <button onClick={this.decrement}>-</button>&nbsp;
                   <button onClick={this.incrementIfOdd}>当前求和为奇数时再加</button>&nbsp;
                   <button onClick={this.incrementAsync}>异步加</button>
               </div>
           )
       }
   }
   ```

#### 3. 完整版的redux

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105800.png)

store.js

```js
/**
 * 该模块专门用于暴露一个store对象，整个应用只有一个store对象
 */

// 引入创建store对象的方法
import { createStore } from "redux";
// 引入为Count组件服务的reducer对象
import countReducer from './count_reducer'

// 创建store的同时，指定为store做事的reducer
// 并默认暴露
export default createStore(countReducer)
```

count_reducer.js

```js
/**
 * 该模块是创建一个为Count组件服务的reducer
 */

 import {INCREMENT, DECREMENT} from './constant'
// 初始化方法一：
const initState=0;
// 
// 如果type为初始化，那么就初始化state为initState，下一次由组件触发函数发生时，获取到的参数preState就为初始化state值，所以可以直接给preState赋initState值
// 参数中赋值表示如果preState为undefined，那么就会被赋值为initState，如果preState有值，那么就不会执行preState=initState
export default function countReducer(preState=initState,action){
    const {type,data}=action;
    console.log(preState);
    // 使用switch直接按条件返回结果，由于当初始化时给preState设置了初始化state值，所以初始化时直接返回preState
    switch (type) {
        case INCREMENT:
            return preState+data;
        case DECREMENT:
            return preState-data;
        default:
            return preState;
    }
}

// // 初始化方法二：
// // 如果type为初始化，那么就初始化state为initState，下一次由组件触发函数发生时，获取到的参数preState就为初始化state值，所以可以直接给preState赋initState值
// // 参数中赋值表示如果preState为undefined，那么就会被赋值为initState，如果preState有值，那么就不会执行preState=initState
// export default function countReducer(preState,action){
//     if(preState===undefined)
//     // 设置你想初始化的值
//     preState=0;
//     const {type,data}=action;
//     // 使用switch直接按条件返回结果，由于当初始化时给preState设置了初始化state值，所以初始化时直接返回preState
//     switch (type) {
//         case 'increment':
//             return preState+data;
//         case 'decrement':
//             return preState+data;
//         default:
//             return preState;
//     }
// }

// // 初始化方法三：
// // 
// // 如果type为初始化，那么就初始化state为initState，下一次由组件触发函数发生时，获取到的参数preState就为初始化state值，所以可以直接给preState赋initState值
// // 参数中赋值表示如果preState为undefined，那么就会被赋值为initState，如果preState有值，那么就不会执行preState=initState
// export default function countReducer(preState,action){
//     const {type,data}=action;
//     // 使用switch直接按条件返回结果，由于当初始化时给preState设置了初始化state值，所以初始化时直接返回preState
//     switch (type) {
//         case 'increment':
//             return preState+data;
//         case 'decrement':
//             return preState+data;
//         default:
//             return 0;
//     }
//     // 不是increment也不是decrement，那就是初始化，所以直接返回要初始化的值，这里写的是0
// }
```

count_action.js

```js
/**
 * 
 * 该模块专门为Count组件创建action对象
 * 
 */

 import {INCREMENT, DECREMENT} from './constant'

// 箭头函数体中，如果只有一行代码，并且是返回之，那么可以省略{}和return，但是如果返回的是一个对象那么()=>{xxx:xxx}会报错，因为会把这个大括号解析为是一个代码块，而不是对象外面的大括号。
// 方法一：加上{}和return
// const incrementAction = (data) => {return {type:'increment',data:data}};
// 方法二：对象外面加一个小括号

// 分别暴露
export const incrementAction = (data) => ({type:INCREMENT,data:data});
export const decrementAction = (data) => ({type:DECREMENT,data:data});
```

constant.js

```js
/**
 * 该模块是用于定义action对象中type类型的常量值，目的只有一个：便于管理的同时防止程序员单词写错
 */

export const INCREMENT = 'increment';
export const DECREMENT = 'decrement';
```

Count.js

```js
import React, { Component } from 'react'
import store from '../../redux/store';
import {incrementAction, decrementAction} from '../../redux/count_action'

export default class Count extends Component {

    componentDidMount=()=>{
        // 在组件挂载完毕时，监听reducer中的状态变化，一旦有状态改变就会调用里面的回调
        store.subscribe(()=>{
            // 在不修改该组件自身state的情况下，手动更新组件
            this.setState({});
            // this.forceUpdate();
        })
    }

    increment = () =>{
        const {value} = this.selectNumNode;
        // value是一个字符串，所以需要-0转为数字再进行运算
        store.dispatch(incrementAction(value-0));
    }

    decrement = () =>{
        const {value} = this.selectNumNode;
        store.dispatch(decrementAction(value-0));
    }

    incrementIfOdd = () =>{
        const {value} = this.selectNumNode;
        if(store.getState()%2 !== 0){
            store.dispatch(incrementAction(value-0));
        }
    }

    incrementAsync = () =>{
        const {value} = this.selectNumNode;
        // 每次点击按钮后，过2秒再执行这个函数
        setTimeout(()=>{
            store.dispatch(incrementAction(value-0));
        },2000);
    }
    render() {
        return (
            <div>
                <h2>当前求和为：{store.getState()}</h2>
                {/* 获取选中的结点 */}
                <select ref={c=>{this.selectNumNode=c}}>
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                </select>&nbsp;
                <button onClick={this.increment}>+</button>&nbsp;
                <button onClick={this.decrement}>-</button>&nbsp;
                <button onClick={this.incrementIfOdd}>当前求和为奇数时再加</button>&nbsp;
                <button onClick={this.incrementAsync}>异步加</button>
            </div>
        )
    }
}
```

#### 4.异步action

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105827.png)

- 如果action为一个对象，则为同步action
- 如果action为一个函数，则为异步action
- 如果想要修改reducer中的状态，但是具体数据要靠异步任务返回时，比如：我在餐厅点菜，我想30分钟后再吃饭，1.我可以自己看菜单看30分钟，然后再叫服务员上菜，2.还可以告诉服务员，我30分钟后吃饭，你给我控制时间。
- 对应到redux中就是：比如：我想要5秒后进行计算器加法运算，1. 我可以在组件内自己设一个延时器，5秒后再dispatch一个action对象，由store交给reducer去改变状态。2. 我还可以dispatch的时候，将一个函数形式的action作为参数，store接收到函数式的action时，会自动调用这个函数（需要借助redux-thunk和applyMiddleware才能自动调用，不然不识别函数式的action），可以在这个函数式action中设置一个5秒的延时器，延时器中分发一个同步的action对象给store，再stroe交给reducer去加工状态
- 所以异步action不是必须要写的，完全可以等待异步任务结束了再去分发同步action

store.js

```js
/**
 * 该模块专门用于暴露一个store对象，整个应用只有一个store对象
 */

// 1.引入创建store对象的方法
// 2.引入第三方组件，允许store调用异步action函数
import { createStore, applyMiddleware} from "redux";
// 引入redux-thunk允许store调用异步action函数
import thunk from 'redux-thunk'
// 引入为Count组件服务的reducer对象
import countReducer from './count_reducer'

// 创建store的同时，指定为store做事的reducer
// 并默认暴露
// 需要将applyMiddleware函数作为创建store的第二个参数传递，一定要是第二个参数，并且将thunk作为参数传给applyMiddleware函数
export default createStore(countReducer,applyMiddleware(thunk))
```

count_reducer.js

```js
/**
 * 该模块是创建一个为Count组件服务的reducer
 */

 import {INCREMENT, DECREMENT} from './constant'
// 初始化方法一：
const initState=0;
// 
// 如果type为初始化，那么就初始化state为initState，下一次由组件触发函数发生时，获取到的参数preState就为初始化state值，所以可以直接给preState赋initState值
// 参数中赋值表示如果preState为undefined，那么就会被赋值为initState，如果preState有值，那么就不会执行preState=initState
export default function countReducer(preState=initState,action){
    const {type,data}=action;
    console.log(preState);
    // 使用switch直接按条件返回结果，由于当初始化时给preState设置了初始化state值，所以初始化时直接返回preState
    switch (type) {
        case INCREMENT:
            return preState+data;
        case DECREMENT:
            return preState-data;
        default:
            return preState;
    }
}

// // 初始化方法二：
// // 如果type为初始化，那么就初始化state为initState，下一次由组件触发函数发生时，获取到的参数preState就为初始化state值，所以可以直接给preState赋initState值
// // 参数中赋值表示如果preState为undefined，那么就会被赋值为initState，如果preState有值，那么就不会执行preState=initState
// export default function countReducer(preState,action){
//     if(preState===undefined)
//     // 设置你想初始化的值
//     preState=0;
//     const {type,data}=action;
//     // 使用switch直接按条件返回结果，由于当初始化时给preState设置了初始化state值，所以初始化时直接返回preState
//     switch (type) {
//         case 'increment':
//             return preState+data;
//         case 'decrement':
//             return preState+data;
//         default:
//             return preState;
//     }
// }

// // 初始化方法三：
// // 
// // 如果type为初始化，那么就初始化state为initState，下一次由组件触发函数发生时，获取到的参数preState就为初始化state值，所以可以直接给preState赋initState值
// // 参数中赋值表示如果preState为undefined，那么就会被赋值为initState，如果preState有值，那么就不会执行preState=initState
// export default function countReducer(preState,action){
//     const {type,data}=action;
//     // 使用switch直接按条件返回结果，由于当初始化时给preState设置了初始化state值，所以初始化时直接返回preState
//     switch (type) {
//         case 'increment':
//             return preState+data;
//         case 'decrement':
//             return preState+data;
//         default:
//             return 0;
//     }
//     // 不是increment也不是decrement，那就是初始化，所以直接返回要初始化的值，这里写的是0
// }
```

count_action.js

```js
/**
 * 
 * 该模块专门为Count组件创建action对象
 * 
 */

 import {INCREMENT, DECREMENT} from './constant'

// 箭头函数体中，如果只有一行代码，并且是返回之，那么可以省略{}和return，但是如果返回的是一个对象那么()=>{xxx:xxx}会报错，因为会把这个大括号解析为是一个代码块，而不是对象外面的大括号。
// 方法一：加上{}和return
// const incrementAction = (data) => {return {type:'increment',data:data}};
// 方法二：对象外面加一个小括号

// 分别暴露
// 该函数返回一个同步action：对象
export const incrementAction = (data) => ({type:INCREMENT,data:data});
export const decrementAction = (data) => ({type:DECREMENT,data:data});

// 该函数返回一个异步action：函数
export const incrementAsyncAction = (data,time) => {
    // 由于只要配置好了redux-thunk，store就会帮你调用这个函数，并且一般在异布action函数中都会分发同步action给store，所以异步action函数可以接受到一个dispatch参数，所以异步action函数中的分发同步action对象时不需要使用store.来调用dispatch函数
    return (dispatch) => {
        setTimeout(() => {
            dispatch(incrementAction(data));
        },time);
    }
}
```

constant.js

```js
/**
 * 该模块是用于定义action对象中type类型的常量值，目的只有一个：便于管理的同时防止程序员单词写错
 */

export const INCREMENT = 'increment';
export const DECREMENT = 'decrement';
```

Count.js

```js
import React, { Component } from 'react'
import store from '../../redux/store';
import {incrementAction, decrementAction, incrementAsyncAction} from '../../redux/count_action'

export default class Count extends Component {

    componentDidMount=()=>{
        // 在组件挂载完毕时，监听reducer中的状态变化，一旦有状态改变就会调用里面的回调
        store.subscribe(()=>{
            // 在不修改该组件自身state的情况下，手动更新组件
            this.setState({});
            // this.forceUpdate();
        })
    }

    increment = () =>{
        const {value} = this.selectNumNode;
        // value是一个字符串，所以需要-0转为数字再进行运算
        store.dispatch(incrementAction(value-0));
    }

    decrement = () =>{
        const {value} = this.selectNumNode;
        store.dispatch(decrementAction(value-0));
    }

    incrementIfOdd = () =>{
        const {value} = this.selectNumNode;
        if(store.getState()%2 !== 0){
            store.dispatch(incrementAction(value-0));
        }
    }

    // incrementAsync = () =>{
    //     const {value} = this.selectNumNode;
    //     // 每次点击按钮后，过2秒再执行这个函数（组件自身设置延时器实现）
    //     setTimeout(()=>{
    //         store.dispatch(incrementAction(value-0));
    //     },2000);
    // }

    incrementAsync = () =>{
        const {value} = this.selectNumNode;
        // 每次点击按钮后，过2秒再执行这个函数（异步action函数内设置延时器实现）
        store.dispatch(incrementAsyncAction(value-0,2000));
    }

    render() {
        return (
            <div>
                <h2>当前求和为：{store.getState()}</h2>
                {/* 获取选中的结点 */}
                <select ref={c=>{this.selectNumNode=c}}>
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                </select>&nbsp;
                <button onClick={this.increment}>+</button>&nbsp;
                <button onClick={this.decrement}>-</button>&nbsp;
                <button onClick={this.incrementIfOdd}>当前求和为奇数时再加</button>&nbsp;
                <button onClick={this.incrementAsync}>异步加</button>
            </div>
        )
    }
}
```

### 17.4 react-redux理解

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105857.png)

UI组件不与redux打交道，只负责界面，不能使用任何redux的api

容器组件只做两件事：1.传递redux中的状态给UI组件 2.传递操作redux状态的方法给UI组件，让UI组件不直接与redux交互

#### 1. 连接容器组件与UI组件

- 新建一个容器组件，一般放在containers文件夹中

- 删掉原UI组件中的有关与redux交互的东西
- npm install react-redux
- 连接UI组件和容器组件
- 渲染到页面的是容器组件，由于容器组件中包含UI组件，所以渲染的同时会将UI组件也渲染到页面，所以App.js中应该渲染的是容器组件
- 容器组件自身引入redux是引入的store，但是它自身不能引入store，所以需要在容器组件的父组件中（例子中为App.js）以props形式将store传给容器组件
- 记得暴露容器组件

#### 2.react-redux的基本使用

- 使用容器组件与redux进行交互时，redux中的状态改变时，会自动重新渲染页面，不需要监听redux状态改变，手动去重新渲染页面

- connect第一次调用，即第一个括号中的参数作用是向UI组件传东西，参数名最好固定，参数必须为函数，且第一个参数函数表示传递状态给UI组件，第二个参数函数表示传递操作状态的方法给UI组件，函数的返回值都要为一个对象，对象中的属性名作为UI组件的props中的key，对象中的属性值作为UI组件的props中的value

- 由于第一个函数参数固定是传递状态的，所以默认可以接收到state状态，不需要通过store.getState()方法去获取状态

- 由于第二个函数参数固定是传递操作状态的方法的，那么一定会使用store.dispatch(action)方法，所以默认可以接收到dispatch方法，不需要通过store.dispatch去获取

  App.js

  ```js
  import React, { Component } from 'react'
  import Count from './containers/Count'
  import store from './redux/store'
  
  export default class App extends Component {
      render() {
          return (
              <div>
                  {/* 由于容器组件自身不能引入store，所以将store以props形式传给容器组件 */}
                  <Count store={store}/>
              </div>
          )
      }
  }
  ```

  容器组件Count.js

  ```js
  // 引入UI组件
  import CountUI from '../components/Count'
  
  // 引入连接UI组件和容器组件、以及连接容器组件与redux的方法:connect
  import {connect} from 'react-redux'
  
  import {incrementAction, decrementAction, incrementAsyncAction} from '../redux/count_action'
  
  // mapStateToProps函数返回的对象中的key就作为传递给UI组件props的key，value就作为传递给UI组件的value————状态
  // 由于第一个函数参数固定是传递状态的，所以默认可以接收到state状态，不需要通过store.getState()方法去获取状态
  const mapStateToProps=(state)=>{
      return {count:state};
  }
  
  // mapDispatchToProps函数返回的对象中的key就作为传递给UI组件props的key，value就作为传递给UI组件的value————操作状态的方法
  // 由于第二个函数参数固定是传递操作状态的方法的，那么一定会使用store.dispatch(action)方法，所以默认可以接收到dispatch方法，不需要通过store.dispatch去获取
  const mapDispatchToProps=(dispatch)=>{
      return {jia:data => dispatch(incrementAction(data)),jian:data => dispatch(decrementAction(data)), asyncAdd:(data,time) => dispatch(incrementAsyncAction(data,time))};
  }
  
  // 创建一个容器组件并连接相应的UI组件
  // connect第一次调用，即第一个括号中的参数作用是向UI组件传东西，参数名最好固定
  // 参数必须为函数，且第一个参数函数表示传递状态给UI组件，第二个参数函数表示传递操作状态的方法给UI组件，函数的返回值都要为一个对象，对象中的属性名作为UI组件的props中的key，对象中的属性值作为UI组件的props中的value
  export default connect(mapStateToProps, mapDispatchToProps)(CountUI);
  ```

  UI组件Count.js

  ```js
  import React, { Component } from 'react'
  
  export default class Count extends Component {
  
      increment = () =>{
          const {value} = this.selectNumNode;
          this.props.jia(value-0);
      }
  
      decrement = () =>{
          const {value} = this.selectNumNode;
          this.props.jian(value-0);
      }
  
      incrementIfOdd = () =>{
          const {value} = this.selectNumNode;
          if(this.props.count % 2 !== 0){
              this.props.jia(value-0);
          }
      }
  
      incrementAsync = () =>{
          const {value} = this.selectNumNode;
          this.props.asyncAdd(value-0,3000);
      }
  
      render() {
          return (
              <div>
                  <h2>当前求和为：{this.props.count}</h2>
                  {/* 获取选中的结点 */}
                  <select ref={c=>{this.selectNumNode=c}}>
                      <option value="1">1</option>
                      <option value="2">2</option>
                      <option value="3">3</option>
                  </select>&nbsp;
                  <button onClick={this.increment}>+</button>&nbsp;
                  <button onClick={this.decrement}>-</button>&nbsp;
                  <button onClick={this.incrementIfOdd}>当前求和为奇数时再加</button>&nbsp;
                  <button onClick={this.incrementAsync}>异步加</button>
              </div>
          )
      }
  }
  ```

#### 3.总结

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002105922.png)

#### 4.mapDispatchToProps精简版

mapDispatchToPropsc参数可以是一个对象，当为对象时，redux会自动调用dispatch，不需要我们手动去调用dispatch

容器组件：Count

```js
// 引入UI组件
import CountUI from '../components/Count'

// 引入连接UI组件和容器组件、以及连接容器组件与redux的方法:connect
import {connect} from 'react-redux'

import {incrementAction, decrementAction, incrementAsyncAction} from '../redux/count_action'

// 创建一个容器组件并连接相应的UI组件
// 1.可以直接将函数体写在参数中
// 2.mapDispatchToProps可以传一个函数，也可以传一个对象，当只传一个对象时，即action对象，redux会自动帮你调用dispatch，不需要自己去调用
export default connect(
    state => ({count:state}),

    //mapDispatchToProps的一般写法：传函数
    // dispatch => ({
    //     jia:data => dispatch(incrementAction(data)),
    //     jian:data => dispatch(decrementAction(data)),
    //     asyncAdd:(data,time) => dispatch(incrementAsyncAction(data,time))
    // })

    //mapDispatchToProps的简写版：传对象
    //跟jia:(data) => ({type:INCREMENT,data:data})作用一样，所以在UI组件中调用jia函数时传递参数传给了incrementAction函数，所以返回结果为一个action对象，当返回为action对象时，redux会自动帮你调用dispatch分发
    {
        jia:incrementAction,
        jian:decrementAction,
        asyncAdd:incrementAsyncAction
    }
)(CountUI);
```

#### 5.Provider组件的使用

- 使用了react_redux之后，不需要再手动去检测redux中状态改变，connect使得容器组件具有自动检测redux状态改变，并且重新渲染组件的效果
- 如果有多个容器组件，那么在App.js中就要以props形式传多个store={store}，react-redux中的Provider标签能解决这个问题

App.js

```js
import React, { Component } from 'react'
import Count from './containers/Count'

export default class App extends Component {
    render() {
        return (
            <div>
                <Count/>
            </div>
        )
    }
}
```

index.js

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import store from './redux/store'
import {Provider} from 'react-redux'

ReactDOM.render(
    // Provider保证App组件中的所有用到store的地方（App中的所有容器组件），都会给它传store
    <Provider store={store}>
        <App/>
    </Provider>,
    document.getElementById('root')
)
```

#### 6.整合UI组件和容器组件

可以将UI组件放在容器组件内部，整合成一个组件，并且都放在containers文件夹下

- 删掉容器组件中，引入UI组件语句
- 将UI组件代码复制到容器组件中，import语句放在一起
- 不暴露UI组件，因为只在容器组件内部使用
- 在connect方法的二次调用中，即第二个括号中使用自定义的UI组件

容器Count组件

```js
// 引入连接UI组件和容器组件、以及连接容器组件与redux的方法:connect
import {connect} from 'react-redux'
// 引入action
import {incrementAction, decrementAction, incrementAsyncAction} from '../redux/count_action'

import React, { Component } from 'react'

// 自定义UI组件，不需要暴露了，因为只在容器组件中使用
class Count extends Component {

    increment = () =>{
        const {value} = this.selectNumNode;
        this.props.jia(value-0);
    }

    decrement = () =>{
        const {value} = this.selectNumNode;
        this.props.jian(value-0);
    }

    incrementIfOdd = () =>{
        const {value} = this.selectNumNode;
        if(this.props.count % 2 !== 0){
            this.props.jia(value-0);
        }
    }

    incrementAsync = () =>{
        const {value} = this.selectNumNode;
        this.props.asyncAdd(value-0,3000);
    }

    render() {
        return (
            <div>
                <h2>当前求和为：{this.props.count}</h2>
                {/* 获取选中的结点 */}
                <select ref={c=>{this.selectNumNode=c}}>
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                </select>&nbsp;
                <button onClick={this.increment}>+</button>&nbsp;
                <button onClick={this.decrement}>-</button>&nbsp;
                <button onClick={this.incrementIfOdd}>当前求和为奇数时再加</button>&nbsp;
                <button onClick={this.incrementAsync}>异步加</button>
            </div>
        )
    }
}

// 创建一个容器组件并连接相应的UI组件
// 1.可以直接将函数体写在参数中
// 2.mapDispatchToProps可以传一个函数，也可以传一个对象，当只传一个对象时，即action对象，redux会自动帮你调用dispatch，不需要自己去调用
export default connect(
    state => ({count:state}),

    //mapDispatchToProps的一般写法：传函数
    // dispatch => ({
    //     jia:data => dispatch(incrementAction(data)),
    //     jian:data => dispatch(decrementAction(data)),
    //     asyncAdd:(data,time) => dispatch(incrementAsyncAction(data,time))
    // })

    //mapDispatchToProps的简写版：传对象
    //跟jia:(data) => ({type:INCREMENT,data:data})作用一样，所以在UI组件中调用jia函数时传递参数传给了incrementAction函数，所以返回结果为一个action对象，当返回为action对象时，redux会自动帮你调用dispatch分发
    {
        jia:incrementAction,
        jian:decrementAction,
        asyncAdd:incrementAsyncAction
    }
)(Count);
```

#### 7.优化react-redux总结

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002110135.png)

### 17.5 数据共享

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002110149.png)




redux要存多个组件存放在redux中的状态，那么redux存放的总状态应该为一个对象，每个组件对应一个key，组件中需要共享的状态即为value

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002110203.png)

如果redux总存储状态为一个对象，那么容器组件中的connect第一个参数中的返回的state即为一个对象，所以不能将这个总状态对象渲染到UI组件中，需要取对象中你需要的状态显示在UI组件上

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002110215.png)

store.js

```js
/**
 * 该模块专门用于暴露一个store对象，整个应用只有一个store对象
 */

// 1.引入创建store对象的方法
// 2.引入第三方组件，允许store调用异步action函数
import { createStore, applyMiddleware, combineReducers} from "redux";
// 引入redux-thunk允许store调用异步action函数
import thunk from 'redux-thunk'
// 引入为Count组件服务的reducer对象
import countReducer from './reducers/count'
// 引入为Person组件服务的reducer对象
import personReducer from './reducers/person'

// redux中存放的总状态为一个对象，combineReducers就是将对象参数转化为redux中的总状态
// he:countReducer表示：he代表Count组件在redux中存放的状态，rens代表Person组件在redux中存放的状态
const allReducers = combineReducers({
    he:countReducer,
    rens:personReducer
})

// 创建store的同时，指定为store做事的reducer
// 并默认暴露
// 需要将applyMiddleware函数作为创建store的第二个参数传递，一定要是第二个参数，并且将thunk作为参数传给applyMiddleware函数
export default createStore(allReducers,applyMiddleware(thunk))
```

reducers/count.js

```js
/**
 * 该模块是创建一个为Count组件服务的reducer
 */

 import {INCREMENT, DECREMENT} from '../constant'
// 初始化方法一：
const initState=0;
// 
// 如果type为初始化，那么就初始化state为initState，下一次由组件触发函数发生时，获取到的参数preState就为初始化state值，所以可以直接给preState赋initState值
// 参数中赋值表示如果preState为undefined，那么就会被赋值为initState，如果preState有值，那么就不会执行preState=initState
export default function countReducer(preState=initState,action){
    const {type,data}=action;
    console.log(preState);
    // 使用switch直接按条件返回结果，由于当初始化时给preState设置了初始化state值，所以初始化时直接返回preState
    switch (type) {
        case INCREMENT:
            return preState+data;
        case DECREMENT:
            return preState-data;
        default:
            return preState;
    }
}

// // 初始化方法二：
// // 如果type为初始化，那么就初始化state为initState，下一次由组件触发函数发生时，获取到的参数preState就为初始化state值，所以可以直接给preState赋initState值
// // 参数中赋值表示如果preState为undefined，那么就会被赋值为initState，如果preState有值，那么就不会执行preState=initState
// export default function countReducer(preState,action){
//     if(preState===undefined)
//     // 设置你想初始化的值
//     preState=0;
//     const {type,data}=action;
//     // 使用switch直接按条件返回结果，由于当初始化时给preState设置了初始化state值，所以初始化时直接返回preState
//     switch (type) {
//         case 'increment':
//             return preState+data;
//         case 'decrement':
//             return preState+data;
//         default:
//             return preState;
//     }
// }

// // 初始化方法三：
// // 
// // 如果type为初始化，那么就初始化state为initState，下一次由组件触发函数发生时，获取到的参数preState就为初始化state值，所以可以直接给preState赋initState值
// // 参数中赋值表示如果preState为undefined，那么就会被赋值为initState，如果preState有值，那么就不会执行preState=initState
// export default function countReducer(preState,action){
//     const {type,data}=action;
//     // 使用switch直接按条件返回结果，由于当初始化时给preState设置了初始化state值，所以初始化时直接返回preState
//     switch (type) {
//         case 'increment':
//             return preState+data;
//         case 'decrement':
//             return preState+data;
//         default:
//             return 0;
//     }
//     // 不是increment也不是decrement，那就是初始化，所以直接返回要初始化的值，这里写的是0
// }
```

reducers/person.js

```js
// 引入type变量
import {ADD_PERSON} from '../constant'
import {nanoid} from 'nanoid'

const initPerson = [{id:nanoid(),name:'张三',age:19}];
export default function personReducer(preState=initPerson,action){
    const {type,data} = action;
    switch (type) {
        case ADD_PERSON:
            return [data,...preState]
        default:
            return preState
    }
}
```

constant.js

```js
/**
 * 该模块是用于定义action对象中type类型的常量值，目的只有一个：便于管理的同时防止程序员单词写错
 */
// Count组件进行加法运算的type
export const INCREMENT = 'increment';
// Count组件进行减法运算的type
export const DECREMENT = 'decrement';

// Person组件中添加人的type
export const ADD_PERSON = 'add_person'
```

actions/count.js

```js
/**
 * 
 * 该模块专门为Count组件创建action对象
 * 
 */

 import {INCREMENT, DECREMENT} from '../constant'

// 箭头函数体中，如果只有一行代码，并且是返回之，那么可以省略{}和return，但是如果返回的是一个对象那么()=>{xxx:xxx}会报错，因为会把这个大括号解析为是一个代码块，而不是对象外面的大括号。
// 方法一：加上{}和return
// const incrementAction = (data) => {return {type:'increment',data:data}};
// 方法二：对象外面加一个小括号

// 分别暴露
// 该函数返回一个同步action：对象
export const incrementAction = (data) => ({type:INCREMENT,data:data});
export const decrementAction = (data) => ({type:DECREMENT,data:data});

// 该函数返回一个异步action：函数
export const incrementAsyncAction = (data,time) => {
    // 由于只要配置好了redux-thunk，store就会帮你调用这个函数，并且一般在异步action函数中都会分发同步action给store，所以异步action函数可以接受到一个dispatch参数，所以异步action函数中的分发同步action对象时不需要使用store.来调用dispatch函数
    return (dispatch) => {
        setTimeout(() => {
            dispatch(incrementAction(data));
        },time);
    }
}
```

actions/person.js

```js
// 引入type
import {ADD_PERSON} from '../constant'

// 新增人
export const createPersonAction = per => ({type:ADD_PERSON,data:per})
```

contains/Count/index.js

```js
// 引入连接UI组件和容器组件、以及连接容器组件与redux的方法:connect
import {connect} from 'react-redux'
// 引入action
import {incrementAction, decrementAction, incrementAsyncAction} from '../../redux/actions/count'

import React, { Component } from 'react'

// UI组件不需要暴露了，因为只在容器组件中使用
class Count extends Component {

    increment = () =>{
        const {value} = this.selectNumNode;
        this.props.jia(value-0);
    }

    decrement = () =>{
        const {value} = this.selectNumNode;
        this.props.jian(value-0);
    }

    incrementIfOdd = () =>{
        const {value} = this.selectNumNode;
        if(this.props.count % 2 !== 0){
            this.props.jia(value-0);
        }
    }

    incrementAsync = () =>{
        const {value} = this.selectNumNode;
        this.props.asyncAdd(value-0,3000);
    }

    render() {
        return (
            <div>
                <h2>此部分为Count组件，下面Person组件的人数为{this.props.personCount}</h2>
                <h4>当前求和为：{this.props.count}</h4>
                {/* 获取选中的结点 */}
                <select ref={c=>{this.selectNumNode=c}}>
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                </select>&nbsp;
                <button onClick={this.increment}>+</button>&nbsp;
                <button onClick={this.decrement}>-</button>&nbsp;
                <button onClick={this.incrementIfOdd}>当前求和为奇数时再加</button>&nbsp;
                <button onClick={this.incrementAsync}>异步加</button>
            </div>
        )
    }
}

// 创建一个容器组件并连接相应的UI组件
// 1.可以直接将函数体写在参数中
// 2.mapDispatchToProps可以传一个函数，也可以传一个对象，当只传一个对象时，即action对象，redux会自动帮你调用dispatch，不需要自己去调用
export default connect(
    state => ({count:state.he, personCount:state.rens.length}),
    {
        jia:incrementAction,
        jian:decrementAction,
        asyncAdd:incrementAsyncAction
    }
)(Count);
```

contains/Person/index.js

```js
import React, { Component } from 'react'
import {connect} from 'react-redux'
import {createPersonAction} from '../../redux/actions/persoin'
import {nanoid} from 'nanoid'
class Person extends Component {

    addPerson = () =>{
        if(this.nameNode.value.trim()!=='' && this.ageNode.value.trim()!==''){
            this.props.addPerson({id:nanoid(), name:this.nameNode.value, age:this.ageNode.value})
        }
        this.nameNode.value='';
        this.ageNode.value='';
    }

    render() {
        return (
            <div>
                <h2>此部分为Person组件，上面Count组件的求和为{this.props.he}</h2>
                <input ref={c=>{this.nameNode=c}} type="text" placeholder="请输入姓名"/>
                <input ref={c=>{this.ageNode=c}} type="text" placeholder="请输入年龄"/>
                <button onClick={this.addPerson}>添加</button>
                <ul>
                    {
                        this.props.persons.map(item=>{
                            return <li key={item.id}>{item.name} {item.age}</li>
                        })
                    }
                </ul>
            </div>
        )
    }
}

export default connect(
    state => ({persons:state.rens, he:state.he}),
    {
        addPerson:createPersonAction
    }
)(Person)
```

App.js

```js
import React, { Component } from 'react'
import Count from './containers/Count'
import Person from './containers/Person'

export default class App extends Component {
    render() {
        return (
            <div>
                <Count/>
                <hr/>
                <Person/>
            </div>
        )
    }
}
```

index.js

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import store from './redux/store'
import {Provider} from 'react-redux'

ReactDOM.render(
    // Provider保证App组件中的所有用到store的地方（App中的所有容器组件），都会给它传store
    <Provider store={store}>
        <App/>
    </Provider>,
    document.getElementById('root')
)
```

### 17.6 纯函数

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002110247.png)

contains/Person/index.js

```js
// 引入type变量
import {ADD_PERSON} from '../constant'
import {nanoid} from 'nanoid'

const initPerson = [{id:nanoid(),name:'张三',age:19}];
export default function personReducer(preState=initPerson,action){
    const {type,data} = action;
    // 1.这里往数组头部添加一个值，不适用数组自带的方法：unshift()是因为：
    // preState.unshift(data) 往数组头部添加一个数，但是preState这个数组的地址并没有改变，redux在检测状态更新时，在底层进行了一个浅比较，发现数组的地址没变，就认为这个状态并没有被更新，所以不会驱动页面重新渲染
    // 所以一般react 和 redux很少使用数组自带的那些方法

    //2. redux的reducer函数必须是一个纯函数，纯函数不能对参数进行修改，unshift是会修改原数组的值，所以preState.unshift(xxx)是会修改preState的值的
    switch (type) {
        case ADD_PERSON:
            return [data,...preState]
        default:
            return preState
    }
}
```

### 17.7 redux开发者工具

1. 导入redux开发者工具

   ![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002110302.png)

2. 安装 npm install redux-devtools-extension 

3. 在store中引用

   ```js
   /**
    * 该模块专门用于暴露一个store对象，整个应用只有一个store对象
    */
   
   // 1.引入创建store对象的方法
   // 2.引入第三方组件，允许store调用异步action函数
   import { createStore, applyMiddleware, combineReducers} from "redux";
   // 引入redux-thunk允许store调用异步action函数
   import thunk from 'redux-thunk'
   // 引入为Count组件服务的reducer对象
   import countReducer from './reducers/count'
   // 引入为Person组件服务的reducer对象
   import personReducer from './reducers/person'
   // 使用redux开发者工具
   import {composeWithDevTools} from 'redux-devtools-extension'
   
   const allReducers = combineReducers({
       he:countReducer,
       rens:personReducer
   })
   
   // 如果createStore没有第二个参数，那么直接将composeWithDevTools()作为第二个参数
   // 如果createStore有第二个参数，那么将composeWithDevTools()作为第二个参数，并将原来的第二个参数作为composeWithDevTools的参数
   export default createStore(allReducers,composeWithDevTools(applyMiddleware(thunk)))
   ```

做完上述三步就可以使用redux开发者工具了

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002110315.png)

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002110326.png)

### 17.8 最终版

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002110338.png)

- 可以汇总reducer的事情放在单独的一个文件夹中，因为如果有很多个容器组件，那么就需要在store中import很多次，并且在combineReducers中定义很多个key、value。一般是将汇总reducer放在reducers文件夹中的index.js文件中
- 对象简写方法：{count:count,person:person}可以简写为只要属性值{count,person}，这表示count为对象的属性值，属性名也为count所以.count==count的

index.js

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import store from './redux/store'
import {Provider} from 'react-redux'

ReactDOM.render(
    // Provider保证App组件中的所有用到store的地方（App中的所有容器组件），都会给它传store
    <Provider store={store}>
        <App/>
    </Provider>,
    document.getElementById('root')
)
```

App.js

```js
import React, { Component } from 'react'
import Count from './containers/Count' //引入Count容器组件
import Person from './containers/Person' //引入Person容器组件

export default class App extends Component {
    render() {
        return (
            <div>
                <Count/>
                <hr/>
                <Person/>
            </div>
        )
    }
}
```

store.js

```js
/**
 * 该模块专门用于暴露一个store对象，整个应用只有一个store对象
 */

// 1.引入创建store对象的方法
// 2.引入第三方组件，允许store调用异步action函数
import { createStore, applyMiddleware} from "redux";
// 引入redux-thunk允许store调用异步action函数
import thunk from 'redux-thunk'
// 使用redux开发者工具
import {composeWithDevTools} from 'redux-devtools-extension'
// 引入汇总后的reducer
import reducer from './reducers'


// 如果createStore没有第二个参数，那么直接将composeWithDevTools()作为第二个参数
// 如果createStore有第二个参数，那么将composeWithDevTools()作为第二个参数，并将原来的第二个参数作为composeWithDevTools的参数
export default createStore(reducer,composeWithDevTools(applyMiddleware(thunk)))
```

constant.js

```js
/**
 * 该模块是用于定义action对象中type类型的常量值，目的只有一个：便于管理的同时防止程序员单词写错
 */
// Count组件进行加法运算的type
export const INCREMENT = 'increment';
// Count组件进行减法运算的type
export const DECREMENT = 'decrement';

// Person组件中添加人的type
export const ADD_PERSON = 'add_person'
```

reducers/count.js

```js
/**
 * 该模块是创建一个为Count组件服务的reducer
 */

 import {INCREMENT, DECREMENT} from '../constant'
// 初始化方法一：
const initState=0;
// 
// 如果type为初始化，那么就初始化state为initState，下一次由组件触发函数发生时，获取到的参数preState就为初始化state值，所以可以直接给preState赋initState值
// 参数中赋值表示如果preState为undefined，那么就会被赋值为initState，如果preState有值，那么就不会执行preState=initState
export default function countReducer(preState=initState,action){
    const {type,data}=action;
    // 使用switch直接按条件返回结果，由于当初始化时给preState设置了初始化state值，所以初始化时直接返回preState
    switch (type) {
        case INCREMENT:
            return preState+data;
        case DECREMENT:
            return preState-data;
        default:
            return preState;
    }
}

// // 初始化方法二：
// // 如果type为初始化，那么就初始化state为initState，下一次由组件触发函数发生时，获取到的参数preState就为初始化state值，所以可以直接给preState赋initState值
// // 参数中赋值表示如果preState为undefined，那么就会被赋值为initState，如果preState有值，那么就不会执行preState=initState
// export default function countReducer(preState,action){
//     if(preState===undefined)
//     // 设置你想初始化的值
//     preState=0;
//     const {type,data}=action;
//     // 使用switch直接按条件返回结果，由于当初始化时给preState设置了初始化state值，所以初始化时直接返回preState
//     switch (type) {
//         case 'increment':
//             return preState+data;
//         case 'decrement':
//             return preState+data;
//         default:
//             return preState;
//     }
// }

// // 初始化方法三：
// // 
// // 如果type为初始化，那么就初始化state为initState，下一次由组件触发函数发生时，获取到的参数preState就为初始化state值，所以可以直接给preState赋initState值
// // 参数中赋值表示如果preState为undefined，那么就会被赋值为initState，如果preState有值，那么就不会执行preState=initState
// export default function countReducer(preState,action){
//     const {type,data}=action;
//     // 使用switch直接按条件返回结果，由于当初始化时给preState设置了初始化state值，所以初始化时直接返回preState
//     switch (type) {
//         case 'increment':
//             return preState+data;
//         case 'decrement':
//             return preState+data;
//         default:
//             return 0;
//     }
//     // 不是increment也不是decrement，那就是初始化，所以直接返回要初始化的值，这里写的是0
// }
```

reducers/person.js

```js
// 引入type变量
import {ADD_PERSON} from '../constant'
import {nanoid} from 'nanoid'

const initPerson = [{id:nanoid(),name:'张三',age:19}];
export default function personReducer(preState=initPerson,action){
    const {type,data} = action;
    switch (type) {
        case ADD_PERSON:
            return [data,...preState]
        default:
            return preState
    }
}
```

reducers/index.js

```js
import {combineReducers} from "redux";
// 引入为Count组件服务的reducer对象
import count from '../reducers/count'
// 引入为Person组件服务的reducer对象
import persons from '../reducers/person'

// 汇总所有的reducer变成一个总reducer
// {count:count,person:person}可以简写为只要属性值{count,person}，这表示count为对象的属性值，属性名也为count所以.count==count的
export default combineReducers({
    count, persons
})
```

actions/count.js

```js
/**
 * 
 * 该模块专门为Count组件创建action对象
 * 
 */

 import {INCREMENT, DECREMENT} from '../constant'

// 箭头函数体中，如果只有一行代码，并且是返回之，那么可以省略{}和return，但是如果返回的是一个对象那么()=>{xxx:xxx}会报错，因为会把这个大括号解析为是一个代码块，而不是对象外面的大括号。
// 方法一：加上{}和return
// const incrementAction = (data) => {return {type:'increment',data:data}};
// 方法二：对象外面加一个小括号

// 分别暴露
// 该函数返回一个同步action：对象
export const increment = (data) => ({type:INCREMENT,data});
export const decrement = (data) => ({type:DECREMENT,data});

// 该函数返回一个异步action：函数
export const incrementAsync = (data,time) => {
    // 由于只要配置好了redux-thunk，store就会帮你调用这个函数，并且一般在异步action函数中都会分发同步action给store，所以异步action函数可以接受到一个dispatch参数，所以异步action函数中的分发同步action对象时不需要使用store.来调用dispatch函数
    return (dispatch) => {
        setTimeout(() => {
            dispatch(increment(data));
        },time);
    }
}
```

actions/person.js

```js
// 引入type
import {ADD_PERSON} from '../constant'

// 新增人
export const addPerson = per => ({type:ADD_PERSON,data:per})
```

containers/Count/index.js

```js
// 引入连接UI组件和容器组件、以及连接容器组件与redux的方法:connect
import {connect} from 'react-redux'
// 引入action
import {increment, decrement, incrementAsync} from '../../redux/actions/count'

import React, { Component } from 'react'

// UI组件不需要暴露了，因为只在容器组件中使用
class Count extends Component {

    increment = () =>{
        const {value} = this.selectNumNode;
        this.props.increment(value-0);
    }

    decrement = () =>{
        const {value} = this.selectNumNode;
        this.props.decrement(value-0);
    }

    incrementIfOdd = () =>{
        const {value} = this.selectNumNode;
        if(this.props.count % 2 !== 0){
            this.props.increment(value-0);
        }
    }

    incrementAsync = () =>{
        const {value} = this.selectNumNode;
        this.props.incrementAsync(value-0,3000);
    }

    render() {
        return (
            <div>
                <h2>此部分为Count组件，下面Person组件的人数为{this.props.persons}</h2>
                <h4>当前求和为：{this.props.count}</h4>
                {/* 获取选中的结点 */}
                <select ref={c=>{this.selectNumNode=c}}>
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                </select>&nbsp;
                <button onClick={this.increment}>+</button>&nbsp;
                <button onClick={this.decrement}>-</button>&nbsp;
                <button onClick={this.incrementIfOdd}>当前求和为奇数时再加</button>&nbsp;
                <button onClick={this.incrementAsync}>异步加</button>
            </div>
        )
    }
}

// 创建一个容器组件并连接相应的UI组件
// 1.可以直接将函数体写在参数中
// 2.mapDispatchToProps可以传一个函数，也可以传一个对象，当只传一个对象时，即action对象，redux会自动帮你调用dispatch，不需要自己去调用
export default connect(
    state => ({count:state.count, persons:state.persons.length}),
    {
        increment,
        decrement,
        incrementAsync
    }
)(Count);
```

containers/Person/index.js

```js
import React, { Component } from 'react'
import {connect} from 'react-redux'
import {addPerson} from '../../redux/actions/persoin'
import {nanoid} from 'nanoid'
class Person extends Component {

    addPerson = () =>{
        if(this.nameNode.value.trim()!=='' && this.ageNode.value.trim()!==''){
            this.props.addPerson({id:nanoid(), name:this.nameNode.value, age:this.ageNode.value})
        }
        this.nameNode.value='';
        this.ageNode.value='';
    }

    render() {
        return (
            <div>
                <h2>此部分为Person组件，上面Count组件的求和为{this.props.count}</h2>
                <input ref={c=>{this.nameNode=c}} type="text" placeholder="请输入姓名"/>
                <input ref={c=>{this.ageNode=c}} type="text" placeholder="请输入年龄"/>
                <button onClick={this.addPerson}>添加</button>
                <ul>
                    {
                        this.props.persons.map(item=>{
                            return <li key={item.id}>{item.name} {item.age}</li>
                        })
                    }
                </ul>
            </div>
        )
    }
}

export default connect(
    state => ({persons:state.persons, count:state.count}),
    {
        addPerson
    }
)(Person)

```

### 17.9 项目打包运行

1. 项目一般是打包成静态资源给后端服务器去部署的，但是由于现在没有服务器，可以借助一个包serve，指定某个文件夹，然后开启一个小型服务器
2. 全局安装serve包,npm install serve -g
3. 将项目打包：npm run build
4. 将build作为服务器根目录，开启一个服务器：serve build

如果serve build报错，原因是：无法加载文件 D:\nvm\v12.16.0\node_global\serve.ps1，因为在此系统上禁止运行脚本。

解决方法：

- 使用管理员权限打开工具软件（如VScode）
- 命令行执行：get-ExecutionPolicy，得到结果为Restricted，表示禁止状态
- 命令行执行：set-ExecutionPolicy RemoteSigned
- 再次执行命令：get-ExecutionPolicy，结果显示为RemoteSigned
- 执行serve build,运行成功。

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211002110524.png)

成功后直接访问5000

运行serve build 报错；**serve : 无法将“serve”项识别为 cmdlet、函数、脚本文件或可运行程序的名称。请检查名称的拼写，如果包括路径，请确保路径正确，然后再试一次**

解决方法：

- 使用`yarn`语法进行服务器安装`yarn add serve`，安装完毕后执行`yarn serve build`即可成功

![](https://cdn.jsdelivr.net/gh/cloverfelix/image/image/20211010210840.png)


