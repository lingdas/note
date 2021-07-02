---
title: 前端笔记es6语法学习
author: lovelves
avatar: 'https://cdn.jsdelivr.net/gh/lingdas/note/img/avactor.jpeg'
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 技术
comments: true
tags: ["JavaScript","ES6",前端笔记]
keywords: js
photos: 'https://cdn.jsdelivr.net/gh/lingdas/note/img/10.jpg'
date: 2021-06-16 20:52:32
authorLink:
description:
---

# es6 学习
###  1，var const let 的区别
+ 变量提升
```javascript
console.log(i);
var i=100;
```
以上代码 在正规的语法中 理应报错 因为变量i未声明过 然而并没有报错  打印出undefined
这是由于var关键字的作用  var声明的变量 无论在程序的哪一行 都会把声明置顶 程序这样翻译
```javascript
var i;
console.log(i);
i=100
```
然而以上代码中 var 关键字 改为let 就会杜绝这种情况的出现 在哪行声明 就在哪行 并不会置顶

var、let声明的就是变量，变量一旦初始化之后，还可以重新赋值

**Const 声明的是常量  const声明后必须立即初始化  const 声明的常量可以修改 但不能重新赋值**

**重复声明 var 允许重复声明，let、const 不允许**

**var 没有块级作用域 let/const 有块级作用域**

用var定义的变量 在windows对象的属性和方法（全局作用域） const let 则没有



###  2，模板字符串
+ 模板字符串与一般字符串一样 唯一区别 模板字符串可以直接注入变量
```javascript
      const person = {
        username: 'Alex',
        age: 18,
        sex: 'male'
      };

      const info =
        '我的名字是：' +
        person.username +
        ', 性别：' +
        person.sex +
        ', 今年' +
        person.age +
        '岁了';
      console.log(info);
```
普通字符串只能 通过加号和变量拼接 但是模板字符串可以很方便的注入变量 
```javascript
      const info = `我的名字是：${person.username}, 性别：${person.sex}, 今年${person.age}岁了`;
      console.log(info);
```
+ 模板字符串的注意事项
  输出多行字符串
     一般字符串
```javascript
      const info = '第1行\n第2行';
      console.log(info);
```
模板字符串
```javascript
      const info = `第1行\n第2行`;
            const info = `第1行
      第2行`;
            console.log(info);
```
 模板字符串中，所有的空格、换行或缩进都会被保留在输出之中
 模板字符串的注入(方法 表达式 对象)
 ```javascript
    ${}
      const username = 'alex';
      const person = { age: 18, sex: 'male' };
      const getSex = function (sex) {
        return sex === 'male' ? '男' : '女';
      };

      const info = `${username}, ${person.age + 2}, ${getSex(person.sex)}`;
      console.log(info);
 ```
 只要最终可以得出一个值的就可以通过 ${} 注入到模板字符串中

 ###  3，箭头函数
 + .认识箭头函数
```javascript
      const add = (x, y) => {
        return x + y;
      };
      console.log(add(1, 1));
    
```
箭头函数的结构
 const/let 函数名 = 参数 => 函数体
如何将一般函数改写成箭头函数
 声明形式
  ```javascript
      function add() {}
  ```
 声明形式->函数表达式形式
  ```javascript
      const add = function () {};
  ```
函数表达式形式->箭头函数
 ```javascript
      const add = () => {};
 ```
1.单个参数(单个参数可以省略圆括号)
 ```javascript
      const add = x => {
        return x + 1;
      };
      console.log(add(1));
 ```
2. 无参数或多个参数不能省略圆括号
 ```javascript
       const add = () => {
        return 1 + 1;
      };
      const add = (x, y) => {
        return x + y;
      };
      console.log(add(1, 1));
 ```
  3.单行函数体
 单行函数体可以同时省略 {} 和 return
 ```javascript
       const add = (x, y) => {
        return x + y;
      };
      const add = (x, y) => x + y;
      console.log(add(1, 1));
 ```
 4.多行函数体不能再化简了
 ```javascript
       const add = (x, y) => {
        const sum = x + y;
        return sum;
      };
 ```
5.单行对象(如果箭头函数返回单行对象，可以在 {} 外面加上 ()，让浏览器不再认为那是函数体的花括号)
```javascript
      const add = (x, y) => {
        return {
          value: x + y
        };
      };
      const add = (x, y) => ({
        value: x + y
      });
```
### 4 非箭头函数中的 this 指向
 1.全局作用域中的 this 指向
 ```javascript
 console.log(this); // window
 ```
 2.一般函数（非箭头函数）中的 this 指向
 ```javascript
     'use strict';
      function add() {
        console.log(this);
      }
      add(); 
 ```
 3.严格模式就指向 undefined  add(); // undefined->window（非严格模式下）
 ```javascript
       const calc = {
        add: add
      };
    calc.add(); // 指向 calc
    const adder = calc.add;
    adder(); // undefined->window（非严格模式下）
 ```
 ```javascript
  document.onclick = function () {
        console.log(this);
      };
      document.onclick();

      function Person(username) {
        this.username = username;
        console.log(this);
      }

      const p = new Person('Alex');
 ```
  只有在函数调用的时候 this 指向才确定，不调用的时候，不知道指向谁
  this 指向和函数在哪儿调用没关系，只和谁在调用有关
  没有具体调用对象的话，this 指向 undefined，在非严格模式下，转向 window

箭头函数没有this指向 

## 解构赋值

1.模式（结构）匹配

{}={}

[]=[]

属性名相同的完成赋值

```javascript
const { age, username } = { username: 'Alex', age: 18 };
```

取别名

```javascript
const { age: age, username: uname } = { username: 'Alex', age: 18 };
```

2. 默认值的生效条件

   对象的属性值严格等于 undefined 时，对应的默认值才会生效

   ```javascript
         const { username = 'ZhangSan', age = 0 } = { username: 'alex' };
         console.log(username, age);
   ```

3. 默认值表达式 如果默认值是表达式，默认值表达式是惰性求值的

4. 如果将一个已经声明的变量用于对象的解构赋值，整个赋值需在圆括号中进行

   ```javascript

         let x = 2;
         ({ x } = { x: 1 });
         [x] = [1];
         console.log(x);
   ```

应用

1.函数参数的解构赋值

```javascript
      const logPersonInfo = ({ age = 0, username = 'ZhangSan' }) =>
        console.log(username, age);
```



### 剩余参数

剩余参数永远是个数组 即使没有值也是空数组

```javascript
        const a = (...args)=>{
            console.log(args);
        }
        a(1,3,5,6)
```

注意 

1. 箭头函数的参数部分即使只有一个剩余函数，也不能省略（）

2. 使用剩余函数代替函数的arguments

   ```javascript
          const a = function(){
               console.log(arguments);
           }
           a(1,2);
   ```

   由于arguments 在箭头函数中不可用 用剩余函数代替

   ​

3. 剩余函数必须是参数的最后位置 否则会报错



剩余参数和结构赋值的结合使用

```javascript
        let [f,b,...c]=[1,2,3,4,5];
        console.log(c); //[3,4,5]
        
        let {h,y,...p}={x:2,y:3,h:8,o:9}
        console.log(p);//{x: 2, o: 9}
```



剩余参数的应用

```javascript
        const a = (...args)=>{
            let sum=0;
            for(let i =0;i<args.length;i++){
                sum=args[i]+sum;
            }
            return sum;
        }
        let sum=a(1,7,8);
        console.log(sum);
```

## 认识展开运算符

就是把数组形式展开 如 【1，2，3】—>.  (1,2,3)

例子

```javascript
console.log(Math.min(...[3,1,2]))
```

跟剩余函数的根本区别

剩余函数 是 3，1，3 ——> [3,1,3]

展开运算符是 [3,1,3] ———> 3,1,3

例子

```javascript
//剩余参数
const add = (...args)=>{
    //展开运算符
    console.log(...args); //1,2,3
    console.log([...[1,2,3],4,5,6])//1,2,3,4,5,6
}
add([1,2,3]);
```



数组运算符的应用

 复制数组

例子

```javascript
const a = [1,2,3];
const b = a;
a[0]=3;
console.log(b)
```

上面代码 只是把a 数组的引用赋值给b. 说白了 就是给a 取了一个别名 b

正确做法 用数组运算符

```javascript
const c = [...a];
a[0]=2;
console.log(c);
```

合并数组

```javascript
const a = [1,2];
const b = [3,4,5];
const c = [5,6];
const d = [...a,...b,...c];// 把abc合并一个数组
```

将字符串转化为数组 

```javascript
console.log([...'lovelves'])
```

将函数的arguments、Nodelist 转化为数组

```javascript
const b = function(){
    console.log([...arguments])
}
```

展开对象

对象必须在{}中展开 展开后就是把属性罗列出来 用逗号分隔放在{}中 构成新的对象

```javascript
const apple ={
    color:'红色',
    shape:'球形'
}
console.log({...apple}===apple)//false
```

合并对象

```javascript
const apple ={
    color:'红色',
    shape:'球型'
};
const pen ={
    color:'黑色',
    shape:'圆形'
}
{...apple,...pen}//合并成一个对象 新对象拥有全部的属性 相同属性 后者覆盖
```

注意事项

空对象的展开（如果展开一个空对象 则没有任何效果）

非对象展开（如果展开的不是对象 则会自动转换成对象 并把属性罗列出来）

```javascript
console.log(...1);
console.log(...undefined);
console.log(...null);
//上面得出结果为空 {}
```

如果展开的运算符后面是字符串 它会自动转成一个类数组对象

```javascript
console.log({...'lovelves'})//{0:l,1:o .....}
```

不会展开对象中的对象属性

```javascript
const pen ={
    color:'黑色',
    shape:'圆形'
    hide: {
    color:'black',
    shape: true
}
}
{...pen}// {color:'黑色'，shape：'圆形'，hide{...}}
```

对象展开运算符的应用

1.复制对象

2.用户参数和默认参数

```javascript
const logUser = userParam=>{
    const defaultParam={
        username:'zhangsan',
        age:0,
        sex:'male'
    };
    const param ={...defaultParam,...userParam}
    console.log(param.username)
};
logUser();
```

## Set

Set 是一系列无序、没有重复值的数据集合

```javascript
const s = new Set();
s.add(1);
s.add(2);
s.add(1);//这里既时加入了 也没作用 Set中不允许有重复成员
console.log(s);
```

Set 没有下标去标示每一个值 所以Set是无序的，也不能像数组那样用过下标访问成员变量

方法

1.add(在set中添加元素)

```javascript
const s = new Set();
s.add(1).add(3);
```

2.has(判断是否存在)

```javascript
s.has(1);
```

3.delete（删除某个元素 即使不存在 也不会报错）

```javascript
s.delete(1);
```

4.clear (清除set中的所有元素)

```javascript
s.clear();
```

5.forEach(按照成员添加进集合的顺序遍历)

```javascript
s.forEach(function(value,key,set){
    console.log(value,key,set===s);//在set中 value和key一样
    console.log(this);//this指向document 因为函数中的第二个参数写了doucument
},document);
console.log(s);
```

属性

1.size（显示成员的个数）

```javascript
s.size
```

构造函数

通过构造函数初始化成员传入数组、字符串、arguments、 Nodelist、 set 等

```javascript
const s = new Set([1,2,1]);
console.log(s);
const b = new Set(s);//复制了一份s
console.log(b===s); //false 
```

注意事项

如何判断重复的方式

set中对重复值的判断基本标准遵循严格相等=== 但是对于NaN 在set中 NaN===NaN

而实际NaN不等于NaN

```javascript
const s = new Set([NaN,1,NaN])
console.log(s) //只有2个数据 1 NaN
console.log(NaN===NaN)//false
```

什么时候使用set

1.数组或字符串去重时

2.不需要下标访问 只需要遍历时

3.为了使用set中的方法时

## Map

**map 和对象都是键值对的集合**

```javascript
const m = neew Map();
m.set('name','lovelves');//键是name 值是lovelves
console.log(m);
```

**map和对象的区别**

 在对象中 键都是字符串

```javascript
const obj = {
    name;'alex'//name 其实是个字符串
}
```

而在map中 键的类型可以是任意类型

```javascript
const m = new Map();
m.set({},'object');
```

**map的方法和属性（跟set使用一样 就不举例了）**

1.set(添加成员 如果键中已存在 后添加会覆盖已有的)

2.get（获取成员 如果获取不存在的会返回undefined）

3.has（判断是否存在）

4.delete（删除成员）

5.clear（清除所有成员）

6.foreach（遍历）

**属性**

**size**（获取多少成员）

**构造函数**

可以传入数组 这里的数组必须是二维数组 set 、map 中 使用方法跟set一样

**注意事项**

判断键名是否相同的方式（跟set中一样）

**什么时候使用map**

如果需要key->value的结构 或者需要字符串以外的值做键 使用map

使用map中的方法 如遍历

**map开发中的应用**

把dom元素放入到map中应用

```html
 <p>1</p>
    <p>2</p>
    <p>3</p>
    <p>4</p>
    <script>
    const [p1,p2,p3,p4]=document.querySelectorAll('p');  
    const b = new Map(
        [[p1,{color:"red",backgroundColor:"red"}],
        [p2,{color:"blue",backgroundColor:"blue"}],
        [p3,{color:"black",backgroundColor:"black"}],
        [p4,{color:"yellow",backgroundColor:"yellow"}]
        ]
    );
    console.log(b);

    b.forEach(function(val,ele,map){
        for (const v in val) {
            ele.style[v]=val[v];
        }
   

    },document)
    </script>
```

## Iterator

Iterator 遍历器 迭代器

 **什么是iterator**

Symbol.iterator(可遍历对象的生成方法)—>it（可遍历对象）—>it.next()—>it.next()…(直到done为true)

```javascript
        const it = [1,2][Symbol.iterator]();
        console.log(it.next());//{value:1,done:false}
        console.log(it.next());//{value:2,done:false}
        console.log(it.next());//{value:underfined,done:true}
```

 **For …of 的用法**

```javascript
const arr = [1,2,3];
for(const item of arr){
    console.log(item);
}
//循环遍历那些done为false对应的值
```

**与break continue 一起使用**

```javascript
const arr = [1,2,3];
for(const item of arr){
    if(item===2){
        break;
        //continue;
    }
    console.log(item);
}
```

**获取数组的索引和值**

```javascript
const arr = [1,2,3];
for(const [index,value] of arr.entries()){
    console.log(index,value);
}
```

**获取数组中索引**

```javascript
const arr = [1,2,3];
for(const [index,value] of arr.keys()){
    console.log(index,value);
}
```

**什么是可遍历的**

只要有Symbol.iterator方法 并且这个方法可以生成可遍历对象 就是可遍历的

只要可遍历 就可以使用 for of 方法遍历

**原生可遍历的有哪些**

1.数组

2.字符串

3.set

4.map

5.arguments

6.nodelist

**非原生可遍历的有哪些**

一般对象拥有可遍历 就是自己写

```javascript
       const obj = {
            age:18,
            name:'lovelves',
            address:'beijing'
        }
        obj[Symbol.iterator]=()=>{
            let index=0;
            return {
                next: function(){
                    index++;
                    if (index===1){
                        return {
                            value: obj.age,
                            done: false
                        }
                    }else if(index===2){
                        return{
                            value: obj.name,
                            done: false
                        }
                    }else if(index===3){
                        return{
                            value: obj.address,
                            done: false
                        }
                    }else{
                        return{
                            value: undefined,
                            done: true
                        }
                    }
                }
            }
        }
        for (const i of obj) {
            console.log(i);
        }
```

**使用了lterator的场合**

1.数组展开运算符

2.数组的解构赋值

3。set和map中的构造函数



## 字符串的新方法

**includes方法**

判断是否包含某个字符串

第一个参数是判断包含的字符串

第二个参数是从哪个位置开始判断

应用 拼接url地址

```javascript
let url = "https://wwww.lovelves.com/course/list";
const addUrlParam = (url,name,value)=>{
    url+= url.includes("?")?'&':'?';
    url+=`${name}=${value}`;
    return url;
}
url = addUrlParam(url,'c','fe');
console.log(url);
```

**补全字符串长度**

padStart() padEnd()

第一个参数 补全后的总个数

第二个参数 要补全的内容

注意事项

第一个参数 少于原字符串的长度 则不会生效 返回原来的字符串

```javascript
console.log('xxx'.padStart(2,'ab')); //不会生效 返回原来字符串 xxx
```

如果第二个参数不填的话自动补全空格

应用

数字前面不够位数补0

```javascript
console.log('1'.padStart(2,'0'));
```

 **清除字符串的首尾空格**

trimStart() 清除前面空格 trimEnd() 清除后面空格

trim()清除前后空格

```javascript
let b = '    a      ';
console.log(b.trimStart());
```



## 数组的新增方法

includes()

第一个参数 要判断的元素 

第二个参数 从哪个位置开始 默认是从0

```javascript
console.log([1,2,3].includes(2));//true
```

应用

去重

```javascript
        const a = o=>{
            const n =[];
            for (const i of o) {
                if(!n.includes(i)){
                    n.push(i);
                }
            }
            return n;
        }
        let arr=a([1,2,2,2,3,4,5,5,6,3,2]);
        console.log(arr);
```

**将其他数据类型转化为数组**

**Array.from();**

第一参数 要转换为数组的的数据

只要可遍历的的都可以转换成数组

数组 字符串 set map nodelist arguments

```javascript
console.log(Array.from("str"));
```

拥有length属性的对象可以转换成数组

```javascript
const obj={
    '0':"a",
    '1':'b',
    length:2
};
console.log(Array.from(obj));

```

第二个参数 是个回调函数 对转换成数组的数据二次处理

```javascript
console.log(Array.from('12',value=>value*2));
```

第三个参数是 决定this的指向



**find() 找到满足条件的一个立即返回**

```javascript
console.log([1,2,3,4,5,6].find((value,index,arr)=>{retrun value>3;}));//4
```

**findIndex() 找到满足条件的立即返回下标**

```javascript
console.log([1,2,3,4,5,6].find((value,index,arr)=>{retrun value>3;}));//3
```

应用

```javascript
const students=[
    {
        name:'aaa',
        sex:'男',
        age:16
    },
    {
        name:'bbb',
        sex:'女',
        age:16
    },
    {
        name:'aaaddd',
        sex:'男',
        age:16
    }
]
console.log(students.find(value=>value.sex==='女')); //返回sex为女的数据
```

## 对象的新增方法

**Object.assign()**

第一个参数 目标对象

第二个参数 源对象

用于合并对象

```javascript
const apple ={
    color:'red',
    shape:'c',
    taste:'dd'
}
const pen ={
    color:'blue',
    shape:'c',
    taste:'dd'
}
console.log(Object.assign(apple,pen));//把apple和pen合并为一个对象并且赋值给apple
```

Object.keys() //返回键

Object.values() // 返回值

Object.entries() //返回键和值

和数组的keys() values() entries() 的区别 

数组返回的是iterator  对象是返回数组

```javascript
const apple ={
    color:'red',
    shape:'c',
    taste:'dd'
}
console.log(object.values(apple)); //[red,c,dd]
```



 





























































