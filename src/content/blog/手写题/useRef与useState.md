---
title: "useState Ref Memo CallBack"
description: "梳理 React useState、useRef、useMemo 和 useCallback 的区别、使用方式与依赖数组注意事项。"
pubDate: "August 21 2026"
latestChangeDate: "August 21 2026"
tags: ["JS"]
Class: ["技术相关"]
---

useState：值变了，页面会重新渲染。
useRef：值变了，页面不会重新渲染。(useRef means use reference)

| 方式     | 能不能跨渲染记住 | 改了会不会重新渲染 |
| -------- | ---------------- | ------------------ |
| 普通变量 | 不能稳定记住     | 不会               |
| useState | 能               | 会                 |
| useRef   | 能               | 不会               |

useState 是“给页面看的数据”，useRef 是“给代码自己记着用的数据”，普通变量是“当前这次执行临时用一下的数据”。
要不要触发界面更新，是 useState 和 useRef 最核心的区别。

useMemo 简单说就是：
缓存一个“计算结果”，依赖没变就直接复用，依赖变了才重新计算。
（返回一个结果）

语法是：

```javascript
const result = useMemo(() => {
  return 计算结果;}, [依赖]);

  const total = useMemo(() => {
  return price * count;
}, [price, count]);//只有 price 或 count 变了，才重新算
不用调用total()即可得到结果
```

而useCallBack则是缓存函数本身，返回函数本身

```javascript
const handleClick = useCallback(() => {
  console.log(price * count);
}, [price, count]);
//price 或 count 变了，才重新创建这个函数。
只有调用handleClick()才得到结果
```

依赖数组 `[]`：表示只在组件挂载/初始化时执行或创建一次，后续重新渲染不会重复执行。  
但如果 Hook 内部用到了会变化的外部 state/props，就不能随便写空数组，否则可能拿到旧值。
