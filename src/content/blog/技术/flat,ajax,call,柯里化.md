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

```js
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
```