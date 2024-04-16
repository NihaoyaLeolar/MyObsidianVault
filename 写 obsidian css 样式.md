---
tags:
  - 前端开发
---

# 以更改代码块样式为例
----
调用开发者工具：`cmd+opt+I` ，然后选择元素，找到对应的style代码（重点是**找对类和属性字段**），去css文件里粘贴并改写。
![[CleanShot 2023-07-10 at 17.17.57.gif]]
🤖 注意: 编辑模式和预览模式，是两块不同的代码，要分别找到！
以下，在text.css里分别编写两种模式代码块的css：
```css
/* 代码块的css ``` */

/* 编辑模式 */
pre {
	border: 1px solid [[ccc]];
	border-radius: 5px;
	transition: transform 0.3s ease, box-shadow 0.3s ease, border 0.3s ease;
}

/* 阅读模式 */
pre:hover {
	transform: scale(1.05);
	box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
	border-color: [[be8bd3]];
}
```
在设置里记得打开这段代码的效果，然后效果就实时生效了
![[CleanShot 2023-07-10 at 17.23.09@2x.png]]
一写学习笔记先放在这：[[css]]