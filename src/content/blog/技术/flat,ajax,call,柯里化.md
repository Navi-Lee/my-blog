---
title: 'flat ajax call 柯里化'
description: ''
pubDate: 'September 6 2026'
tags: ['JS','手写题']
Class: ['前端算法手写八股','技术相关']
---
- flat
  - 数组扁平化
  就是给里面的数组套数组一层层展开，函数参数传入数组和需要展开的层数
  ```js
  function arrayFlat(arr,depth=1){  
    let res=[]
    for(let i=0;i<arr.length;i++){
      if(Array.isArray(arr[i])&&depth>0){
        res=res.concat(arrayFlat(arr[i],depth-1))
        //concat 不会修改原数组，它会返回一个新数组
      }else{
        res.push(arr[i])
      }
    }
    return res
  }
  ```
  - 对象扁平化
    多层对象变成一层：比如
  ```js
    const source = {
  a: {
    b: {
      c: 1,
      d: 2
    },
    e: 3
  },
  f: {
    g: 2
  }
    }
  //变成这样。
  {
  "a.b.c": 1,
  "a.b.d": 2,
  "a.e": 3,
  "f.g": 2
  }
  ```

  核心思想就一句话：往对象里面递归走，同时把走过的 key 拼起来

  ````js
  function objectFlat(obj) {
  // 最终结果是对象，不是数组
  const res = {}

  function flat(item, prefix = '') {

    // 遍历当前对象的每一个 key 和 value
    Object.entries(item).forEach(([key, value]) => {

      // 如果 prefix 有值，就拼成 "a.b"
      // 如果 prefix 为空，说明是第一层，直接使用 key
      const newKey = prefix
        ? `${prefix}.${key}`
        : key

      // value 还是对象，就继续递归
      if (value && typeof value === 'object') {
        flat(value, newKey)
      } else {
        // 已经是普通值，就保存到结果对象中
        res[newKey] = value
      }
    })
  }

  // 从 obj 最外层开始递归
  flat(obj)

  return res
  }

  ````

- ajax
浏览器不刷新整个页面，也可以偷偷向服务器发请求、拿数据。
Asynchronous JavaScript And XML

```js
// 创建一个网络请求对象
const xhr = new XMLHttpRequest();

// 配置请求
// "GET"：向服务器获取数据
// url：请求地址
// true：异步请求，不阻塞 JS进行
xhr.open("GET", url, true);

// 当请求状态发生变化时执行
xhr.onreadystatechange = function () {

  // readyState === 4 表示请求已经彻底完成
  // 如果还没完成，就直接退出
  if (this.readyState !== 4) return;

  // status === 200 表示请求成功
  if (this.status === 200) {

    // response 是服务器返回的数据
    console.log(this.response);
    //这里也可以用dom修改页面等等

  } else {

    // 请求失败，抛出错误
    // statusText 是错误状态文字
    throw new Error(xhr.statusText);
  }
};

// 真正发送请求
xhr.send();
```

- call
```js
Function.prototype.myCall=function(thisArg,...args){
  const fn=this //这里的this即为下方调用的say
  //因为对象.函数()”的调用方式，会让函数里的 this 指向这个对象
  //(此处的say虽然是函数，但也是对象的一种)
  thisArg = (thisArg!==undefined&&thisArg!==null)?Object(thisArg):window
  // 创建一个唯一的属性名
  // 避免和 thisArg 里面原有的属性重名
  const tag=Symbol("call")
  thisArg[tag]=fn
  //通过 thisArg 调用这个函数,相当于对象.函数(),this会指向thisArg
  const res=thisArg[tag](...args) 
  delete thisArg[tag]
  return res
}

//调用
const obj = {
  name: 'Navi'
}

function say(age) {
  console.log(this.name, age)
}

say.myCall(obj, 18)

//myCall 里的 this → say
//say 里的 this → thisArg
```
