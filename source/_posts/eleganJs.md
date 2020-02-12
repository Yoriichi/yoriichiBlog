---
title: JavaScript优雅写法及骚操作
date: 2020-02-12 09:47:39
excerpt: 优雅的书写代码
cover: /images/eleganJs.jpg
---
# JavaScript优雅写法及骚操作

> 昨天看权威指南的时候看到一个个人觉得很优雅的写法，便想着记录一下

### 1、 判断为空

- 直白写法

```
 if(a == undefined) a = [];
    
 if(params.success){
    params.success(res);
 }  
```

- 优雅写法

```
  a = a || [];
    
  params.success&&params.success(res);
  //注意事项
  1、if内不能出现var、=等赋值定义语句，才可以使用优雅写法
  2、if内可以有多个方法调用，但必须方法内有true返回值（此用法意义不大）   
```

> 问题：我们编写js代码时经常遇到复杂逻辑判断的情况，通常大家可以用if/else或者switch来实现多个条件判断，但这样会有个问题，随着逻辑复杂度的增加，代码中的if/else/switch会变得越来越臃肿，越来越看不懂.

### 2、多条件判断

- 小白写法

```
    var Statistics = function(){
      console.log('执行')
    }
    switch (currentTab) 
    {
       case 0:
           Statistics();
           break;
      case 1:
         Statistics();
           break;
       case 2:
           Statistics();
         break;
        case 3:
         Statistics();
         break;
    }
```

- 优雅写法

```
    //将判断条件作为对象的属性名，将处理逻辑作为对象的属性值
    var Statistics = function(){
      console.log('执行')
    }
    const comparativeTotles = new Map([
        [0,Statistics],
        [1,Statistics],
        [2,Statistics],
        [3,Statistics]
     ])
    let map = function(val){
          return comparativeTotles.get(val)
    } 
    let getMap  = map(1); //如果查找不到返回undefined
    if(!getMap){
          console.log('查找不到')
    }else{
        concaozuole.log('执行操作')
          getMap()
    }
```

- if else

```
/**
 * 按钮点击事件
 * @param {number} status 活动状态：1开票中 2开票失败 3 开票成功 4 商品售罄 5 有库存未开团
 * @param {string} identity 身份标识：guest客态 master主态
 */
const onButtonClick = (status, identity) => {
  if (identity == 'guest') {
    if (status == 1) {
      //函数处理
    } else if (status == 2) {
      //函数处理
    } else if (status == 3) {
      //函数处理
    } else if (status == 4) {
      //函数处理
    } else if (status == 5) {
      //函数处理
    } else {
      //函数处理
    }
  } else if (identity == 'master') {
    if (status == 1) {
      //函数处理
    } else if (status == 2) {
      //函数处理
    } else if (status == 3) {
      //函数处理
    } else if (status == 4) {
      //函数处理
    } else if (status == 5) {
      //函数处理
    } else {
      //函数处理
    }
  }
}
```

- 改完后

```
//利用数组循环的特性，符合条件的逻辑都会被执行，那就可以同时执行公共逻辑和单独逻辑。
const functionA = ()=>{/*do sth*/}       // 单独业务逻辑
const functionB = ()=>{/*do sth*/}       // 单独业务逻辑
const functionC = ()=>{/*send log*/}   // 公共业务逻辑
const actions = new Map([
    ['guest_1', () => { functionA }],
    ['guest_2', () => {  functionB }],
    ['guest_3', () => { functionC }],
    ['guest_4', () => { functionA }],
    ['default', () => { functionC  }],
    //...
])
 
/**
 * 按钮点击事件
 * @param {string} identity 身份标识：guest客态 master主态
  * @param {number} status 活动状态：1开票中 2开票失败 3 开票成功 4 商品售罄 5 有库存未开团
 */
const onButtonClick = (identity, status) => {
  let action = actions.get(`${identity}_${status}`) || actions.get('default')
  action.call(this)
}
```

### 三、'骚'操作

#### 1. 生成随机ID

```
 // 生成长度为10的随机字母数字字符串
Math.random().toString(36).substring(2);
```

#### 2. 每秒更新当前时间

```
setInterval(()=>document.body.innerHTML=new Date().toLocaleString().slice(10,18))
```

#### 3. 生成随机 16 进制 颜色 码 如 # ffffff

```
'#' + Math.floor(Math.random() * 0xffffff).toString(16).padEnd(6, '0');
```

#### 4. 返回键盘

```
// 用字符串返回一个键盘图形
(_=>[..."`1234567890-=~~QWERTYUIOP[]\~ASDFGHJKL;'~~ZXCVBNM,./~"].map(x=>(o+=`/${b='_'.repeat(w=x<y?2:' 667699'[x=["BS","TAB","CAPS","ENTER"][p++]||'SHIFT',p])}\|`,m+=y+(x+'    ').slice(0,w)+y+y,n+=y+b+y+y,l+=' __'+b)[73]&&(k.push(l,m,n,o),l='',m=n=o=y),m=n=o=y='|',p=l=k=[])&&k.join`
`)()
```

#### 5. 优雅的取整

```
var a = ~~2.33   ----> 2
var b = 2.33 | 0   ----> 2
var c = 2.33 >> 0   ----> 2
```

#### 6.优雅的金钱格式化

```
1、使用正则实现
var test1 = '1234567890'
var format = test1.replace(/\B(?=(\d{3})+(?!\d))/g, ',')
console.log(format) // 1,234,567,890
2、使用骚操作
function formatCash(str) {
       return str.split('').reverse().reduce((prev, next, index) => {
            return ((index % 3) ? next : (next + ',')) + prev
       })
}
console.log(format) // 1,234,567,890
```

#### 7. 五种方法实现值交换

```
1. var temp = a; a = b; b = temp; (传统，但需要借助临时变量)
 
2. a ^= b; b ^= a; a ^= b; (需要两个整数)
 
3. b = [a, a = b][0] (借助数组)
 
4. [a, b] = [b, a]; (ES6，解构赋值)
 
5. a = a + b; b = a - b; a = a - b; (小学奥赛题)
```

#### 8. 实现深拷贝

```
var b = JSON.parse(JSON.string(a))
```

#### 9. 去掉小数部分

```
//下面几种方式都行
parseInt(num)
 
~~num
 
num >> 0
 
num | 0
```

#### 10. 递归求阶乘

```
function factorial(n) {
 
  return (n > 1) ? n * f(n - 1) : n
 
```

#### 11. 打印试试

```
console.log(([][[]] + [])[+!![]] + ([] + {})[!+[] + !![]])
 
console.log((!(~+[]) + {})[--[~+''][+[]] * [~+[]] + ~~!+[]] + ({} + [])[[~!+[]] * ~+[]])
```

#### 12. console美化

```
console.info("%c哈哈", "color: #3190e8; font-size: 30px; font-family: sans-serif");
```