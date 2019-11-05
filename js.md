## js简史

#### js的组成部分

+ 核心：描述该语言该的语法和基本对象
+ 文档对象模型（Dom）：描述与网页内容进行交互的对象和接口
+ 浏览器对象模型（Bom）：描述与浏览器交互的对象和接口
####  事件循环

> ##### **科普**
>
> 1. v8引擎里面没有ajax、console、settimeout……，他里面只有两个东西
>
>       + 堆
>
> + 调用栈
>
> 2. chrome 浏览器里面包含V8引擎、webAPI、事件队列
>
> 3. 单线程就是只有一个调用栈，同一时间只能执行一个事件
> 4. 程序执行顺序:
>
> + 入口进栈
> + 申明所有东西
> + 没有申明的变量，然后开始从上到下执行，遇到调用函数就进栈
> + 重复2和3的步骤
>
> 3. 同步:前面的东西没有执行完，就不会执行下面的操作
> 4. 异步:前面的东西正在执行，但是我还是要先继续往后面执行操作




#### 数组

**数组去重：**

1. Array.from(new Set([...arr1,...arr2]))

**判断数组中是否包含另一个数组中的元素**
1. find
```js
var arr=[1,2,3,2,1];
var target=[3,2,1,4,6];
target.find(value=>{
    // if(arr.indexOf(value)>-1){
    //     console.log(value)//3,2,1
    // }
   return arr.indexOf(value)>-1//返回找到的第一个元素值
})
```
2. findIndex
```js
var arr=[1,2,3,2,1];
var target=[3,2,1,4,6];
target.findIndex(value=>{
    return arr.indexOf(value)>-1
})//返回找到的第一个元素的数组下标
```

**找到两个数组共同的部分**
```js
let [res,arr,target]=[[],[1,2,3],[2,3.4]];
target.find(value=>{
    if(arr.indexOf(value)>-1&&res.indexOf(value)<0){
        res.push(value)
    }
})
```
```js
getIntersection = (arr1, arr2) => {
  let len = Math.min(arr1.length, arr2.length)
  let i = -1
  let res = []
  while (++i < len) {
    const item = arr2[i]
    if (arr1.indexOf(item) > -1) res.push(item)
  }
  return res
}
```

#### 时间
```js
//1.都是用于计算时间戳
new Date('04 Dec 1995 00:12:00 GMT').getTime()===Date.parse('04 Dec 1995 00:12:00 GMT')===Date.parse('1995-12-04')
//2.通过时间戳计算日期
new Date(818035200000)//Mon Dec 04 1995 08:00:00 GMT+0800 (中国标准时间)
//3.判断历史日期
Date.parse(new Date())>Date.parse('2995-12-04')//false：这是未来的日期；true表示这是历史日期（包含今日）
//4.计算相对时间的函数
getRelativeTime = timeStamp => {
  // 判断当前传入的时间戳是秒格式还是毫秒
  const IS_MILLISECOND = isMillisecond(timeStamp)
  // 如果是毫秒格式则转为秒格式
  if (IS_MILLISECOND) Math.floor(timeStamp /= 1000)
  // 传入的时间戳可以是数值或字符串类型，这里统一转为数值类型
  timeStamp = Number(timeStamp)
  // 获取当前时间时间戳
  const currentTime = Math.floor(Date.parse(new Date()) / 1000)
  // 判断传入时间戳是否早于当前时间戳
  const IS_EARLY = isEarly(timeStamp, currentTime)
  // 获取两个时间戳差值
  let diff = currentTime - timeStamp
  // 如果IS_EARLY为false则差值取反
  if (!IS_EARLY) diff = -diff
  let resStr = ''
  const dirStr = IS_EARLY ? '前' : '后'
  // 少于等于59秒
  if (diff <= 59) resStr = diff + '秒' + dirStr
  // 多于59秒，少于等于59分钟59秒
  else if (diff > 59 && diff <= 3599) resStr = Math.floor(diff / 60) + '分钟' + dirStr
  // 多于59分钟59秒，少于等于23小时59分钟59秒
  else if (diff > 3599 && diff <= 86399) resStr = Math.floor(diff / 3600) + '小时' + dirStr
  // 多于23小时59分钟59秒，少于等于29天59分钟59秒
  else if (diff > 86399 && diff <= 2623859) resStr = Math.floor(diff / 86400) + '天' + dirStr
  // 多于29天59分钟59秒，少于364天23小时59分钟59秒，且传入的时间戳早于当前
  else if (diff > 2623859 && diff <= 31567859 && IS_EARLY) resStr = getDate(timeStamp)
  else resStr = getDate(timeStamp, 'year')
  return resStr
}

```

#### 函数处理
```js
/**
 * @description 绑定事件 on(element, event, handler)
 */
export const on = (function () {
  if (document.addEventListener) {
    return function (element, event, handler) {
      if (element && event && handler) {
        element.addEventListener(event, handler, false)
      }
    }
  } else {
    return function (element, event, handler) {
      if (element && event && handler) {
        element.attachEvent('on' + event, handler)
      }
    }
  }
})()
```
`navigator.language获取浏览器系统语言`
