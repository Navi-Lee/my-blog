---
title: "数组api发布订阅模式url解析对象比较"
description: ""
pubDate: "September 9 2026"
tags: ["JS", "手写题"]
Class: ["前端算法手写八股", "技术相关"]
---

- 数组api

  ```js
  Array.prototype.map = function (fn) {
    const res = [];
    for (let i = 0; i < this.length; i++) {
      res.push(fn(this[i], i, this));
    }
    return res;
  };

  Array.prototype.filter = function (fn) {
    const res = [];
    for (let i = 0; i < this.length; i++) {
      if (fn(this[i], i, this)) {
        res.push(this[i]);
      }
    }
    return res;
  };

  Array.prototype.reduce = function (fn, initialNum) {
    let res = 0;
    let startIndex = 0;
    if (arguments.length !== 1) {
      //arguments是函数自带，统计调用时传入了几个参数
      res = initialNum;
    } else {
      res = this[0];
      startIndex = 1;
    }
    for (let i = startIndex; i < this.length; i++) {
      res = fn(res, this[i], i, this);
    }
    return res;
  };
  ```

- 发布订阅模式

  on：订阅事件(Event)，把回调函数存起来。
  emit：发布事件，执行这个事件的所有回调函数。
  off：取消订阅，删除指定回调函数。

  ```js
  class eventEmitter {
    constructor() {
      this.events = {};
    }
    on(eventName, fn) {
      if (!this.events[eventName]) {
        this.events[eventName] = [];
      }
      this.events[eventName].push(fn);
    }
    off(eventName, fn) {
      let handler = this.events[eventName];
      if (!handler) return;
      let Index = handler.indexOf(fn); //寻找fn的index
      if (Index !== -1) {
        handler.splice(Index, 1); //splice删除
      }
    }
    emit(eventName, ...args) {
      //...args是收集额外参数
      let handler = this.events[eventName];
      if (!handler) return;
      //避免修改数组，复制一个用slice
      handler.slice().forEach((fn) => {
        fn(...args);
        //传入额外参数
      });
    }
  }
  ```

- url解析

  解析url后面一串传数据的内容

  ```js
  const url = "https://a.com?name=Navi&age=18#top";
  //以此url为例
  function parseURL(url) {
    const result = {};
    const queryString = url.split("?")[1]?.split("#")[0];
    //url.split("?")意思：
    //遇到 ? 就把字符串切开放入数组,然后[1]取数组的第二个值
    //得到："name=Navi&age=18#top"
    //以此类推，然后变成："name=Navi&age=18"

    if (!queryString) {
      return result;
    }
    for (const item of queryString.split("&")) //of遍历值,in遍历key
    {
      if (!item) continue;
      const [key, ...rest] = item.split("=");
      //...rest收集为一个数组
      if (!key) continue;

      const value =
        rest.length === 0 ? true : decodeURIComponent(rest.join("="));
      //.join("=")，把去掉的=再拼回来
      //对 URL 编码进行解码,例如把“%8E%E9%81%93”一类解码
      if (Object.prototype.hasOwnProperty.call(result, key)) {
        // 如果 result 里已经有这个 key
        //此处是让 hasOwnProperty这个方法临时把result当成它的this
        //有key的话就会返回true
        if (Array.isArray(result[key])) {
          // 如果这个 key 对应的值已经是数组,说明它之前已经重复过,那就继续把新值 push 进去
          result[key].push(value);
        } else {
          //
          result[key] = [result[key], value];
        }
      } else {
        result[key] = value;
      }
    }
    return result;
  }
  ```

- 对象比较
  ```js
  function isEqual(obj1, obj2) {
    if (typeof obj1 !== "object" || typeof obj2 !== "object") {
      return obj1 === obj2;
    }
    if (obj1 === obj2) {
      return true;
    }
    if (obj1 === null || obj2 === null) {
      return false;
    }

    const key1 = Object.keys(obj1);
    const key2 = Object.keys(obj2);
    if (key1.length !== key2.length) {
      return false;
    }
    for (let key of key1) {
      if (!Object.prototype.hasOwnProperty.call(obj2, key)) {
        return false;
      }
      if (!isEqual(obj1[key], obj2[key])) {
        return false;
      }
    }
    return true;
  }
  ```
