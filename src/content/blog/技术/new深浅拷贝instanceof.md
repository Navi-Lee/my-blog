---
title: 'new深浅拷贝instanceof'
description: 'JavaScript 手写题笔记：实现 new、浅拷贝与深拷贝，以及 instanceof 的原型链判断原理。'
pubDate: 'September 4 2026'
tags: ['JS','手写题']
Class: ['前端算法手写八股','技术相关']
---
- 普通new:
```js
function Person(name, age) {
  this.name = name
  this.age = age
}

const p = new Person("Navi", 18)
```
所以我们要模拟`const p = myNew(Person, "Navi", 18)`

代码：
```js
function myNew(fn,...args){
    if(typeof fn!=="function"){
        throw new TypeError("fn must be a function")
    }

    //两步走，对function和Prototype分别击破
    const obj=Object.create(fn.prototype)
    //创建对象并指定原型。obj.__proto__===fn.prototype
    //__proto__是上一级对象 prototype就是挂方法的
    //例如调用obj.say,obj会先看自己的obj.say有没有，没有的话会去obj.__proto__找，也就是调用fn.prototype.say
    //普通函数：有 prototype也有 __proto__
    //普通对象：没有 prototype,有 __proto__
    let ret=fn.call(obj,...args)//执行构造函数，指定this是obj

    //如果构造函数主动返回对象，就返回它,否则返回 obj
    return ret instanceof object?ret:obj

}

调用例如：
function Person(name, age) {
  this.name = name
  this.age = age
}
Person.prototype.say = function() {
  console.log(`我是${this.name}，今年${this.age}岁`)
}
const p = myNew(Person, "Navi", 18)
```

老 JS：构造函数 + prototype ≈ 类
现代 JS：class 是更好看的语法，但底层还是原型机制。
```js
class Person {
  constructor(name) {
    this.name = name
  }//constructor就是构造函数

    //对象方法
  say() {
    console.log(this.name)
  }
}
```

- 深浅拷贝
浅拷贝：外层是新的，内层对象还是共用的。
深拷贝：外层和内层对象全都是新的。
```js
function shallowCopy(obj){
  const newObj={}
  for(const key in obj){
    if(obj.hasOwnProperty(key)){
    newObj[key]=obj[key]
    //这里key是变量，只能这样写。不能写obj.key，他的意思是访问名字是"key"的属性。
    }
  }
  return newObj
}
function deepCopy(obj){//输入的不一定是个对象
  if(obj instanceof Date)return new Date(obj)
  if(obj instanceof Error)return new Error(obj.message)
  if(obj instanceof RegExp)return new RegExp(obj)
  if(obj instanceof Function)return(
    function(...args){
      return obj.call(this,...args)
    }
  )
  //因为typeof(null)=='object'，所以需要拦住是null的情况直接返回
  if(!obj||typeof(obj)!=='object'){
    return obj
  }
  const newObj=Array.isArray(obj)?[]:{}
  for(let key in obj){
    if(obj.hasOwnProperty(key)){
      //hasOwnProperty,确保这个key是他自己的属性，而不是他从原型继承的
      //因为for in会遍历所有key，包括继承来的。（遍历对象及其原型链上的可枚举属性）
      if(typeof obj[key]==='object'){
        newObj[key]=deepCopy(obj[key])
        //递归
      }else{
        newObj[key]=obj[key]
      }
    }
  }
  return newObj
}
```
- instanceof
用法：
```js
function Person() {}
const p = new Person()
p instanceof Person // true
```
代码：
```js
function myInstanceOf(left,right){
  const prototype = right.prototype
  let proto = Object.getPrototypeOf(left)//也就是获得left.__proto__（left所继承的原型）
  while(true){
  if(proto===null)return false
  if(proto===prototype)return true //两者相等，left就是right的实例
  proto=Object.getPrototypeOf(proto) //这个是object内置对象上面的方法

  }
  
}
```

