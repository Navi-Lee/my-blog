---
title: 'JavaScript手写题'
description: ''
pubDate: 'August 10 2026'
latestChangeDate: 'August 10 2026'
tags: ['JS','手写题']
Class: ['前端算法手写八股']
---
1.数组转树

```javascript
/**
 * 把平铺的数组结构转成树形结构
 */
const arr = [
//pid是说他的父节点的id
  { id: "01", name: "张大大", pid: "", job: "项目经理" },
  { id: "02", name: "小亮", pid: "01", job: "产品leader" },
  { id: "03", name: "小美", pid: "01", job: "UIleader" },
  { id: "04", name: "老马", pid: "01", job: "技术leader" },
  { id: "05", name: "老王", pid: "01", job: "测试leader" },
  { id: "06", name: "老李", pid: "01", job: "运维leader" },
  { id: "07", name: "小丽", pid: "02", job: "产品经理" },
  { id: "08", name: "大光", pid: "02", job: "产品经理" },
  { id: "09", name: "小高", pid: "03", job: "UI设计师" },
  { id: "10", name: "小刘", pid: "04", job: "前端工程师" },
  { id: "11", name: "小华", pid: "04", job: "后端工程师" },
  { id: "12", name: "小李", pid: "04", job: "后端工程师" },
  { id: "13", name: "小赵", pid: "05", job: "测试工程师" },
  { id: "14", name: "小强", pid: "05", job: "测试工程师" },
  { id: "15", name: "小涛", pid: "06", job: "运维工程师" },
];
```
```javascript
//递归法
function toTree(list, parId) {
  let len = list.length;
  function loop(parId) {//返回输入的节点下面所有子节点
    let res = [];
    for (let i = 0; i < len; i++) {
      let item = list[i]; 
      if (item.pid === parId) {
        //.children表示父子关系，让树形成
        item.children = loop(item.id);//找子节点的子节点
        res.push(item);
      }
    }
    return res;
  }
  return loop(parId);
}

let result = toTree(arr, "");
console.log(result);
```
```javascript
// hash法
function toTreeHash(list) {
  const res = [];
  const map = {};
  // of遍历值（也就是下方的value对象），in遍历key
  // 1. 先遍历一遍数组，把每个元素的 id 映射为节点对象
  for (const item of list) {
    map[item.id] = { ...item, children: [] };
  }
// 此时大概是这样：map={
//  "01":{
//     id:"01",
//     name:"张大大",
//     children:[]
//  },

//  "02":{
//     id:"02",
//     name:"小亮",
//     children:[]
//  }
}
  // 2. 再遍历数组，将节点放入其父节点的 children 中
  for (const item of list) {
    const node = map[item.id];
    if (item.pid) {
      if (!map[item.pid]) continue; // 防御性处理，避免写了pid，但是找不到这个pid对应的节点而报错
      map[item.pid].children.push(node);
    } else {
      res.push(node); // pid为空字符串(false),就是根节点
    }
  }

  return res;


const tree = toTreeHash(arr);
console.log(tree);

// 优势说明
// 时间复杂度：O(n)，因为我们只遍历两次 arr；

// 利用了哈希表 map 进行父子节点的快速定位与组装；

// 比递归版本更适用于大数据量场景；

// 不依赖元素顺序，结构更健壮。
```