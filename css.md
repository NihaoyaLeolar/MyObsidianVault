---
banner: "![[cs.avif]]"
---
[[css]] 
# 一些要知道的CSS知识
----
### 样式不生效的原因
- 选择器不正确：检查 HTML 代码，确认是否存在类名为xxx的元素
- 样式存在错误：最后，检查样式代码本身是否存在语法错误或其他错误，如拼写错误、缺少分号等。一个小错误可能导致整个样式规则不起作用。
- **样式被覆盖**：如果有其他样式规则在这些样式之后被定义，并且具有更具体的选择器或使用了 `!important`，那么可能会覆盖你的样式。你可以检查其他 CSS 规则，看是否有更具体或使用了 `!important` 的样式定义。（反过来，我也可以用`!important`来做）
- 被外面的框框限制住了，一直调大里面的无效
### css盒模型
CSS 盒模型将元素分为:
- 内容框（content box）
- 内边距框（padding box）
- 边框框（border box）
- 外边距框（margin box）
这四个层级
### 选择器的组合
```css
/* 会更改所有行 */
.markdown-source-view.mod-cm6 .cm-line {
	position: relative;
	padding: 0.25em;
}
/* 会更改h2那一行 */
.HyperMD-header-2.cm-line {
	/* 必须要important，不然就是0.25 */
	padding-top: 12em !important;
	padding-bottom: 0.4em !important;  
}
```
上述 CSS 代码中，`.markdown-source-view.mod-cm6 .cm-line` 和 `.HyperMD-header-2.cm-line` 是两个不同的选择器，它们在应用样式时有不同的作用范围。
1. `.markdown-source-view.mod-cm6 .cm-line`：
    - 这是一个连续使用多个类选择器的选择器。它会选择具有类名 `.cm-line` 的元素，但只有当它们同时具有 `.markdown-source-view` 和 `.mod-cm6` 这两个父元素的类名时才会生效。
    - 例如，`<div class="markdown-source-view mod-cm6">` 内部的具有 `.cm-line` 类的元素将受到这个选择器的样式影响。
2. `.HyperMD-header-2.cm-line`：
    - 这是一个类选择器的组合。它会选择同时具有 `.HyperMD-header-2` 和 `.cm-line` 这两个类名的元素，并为其应用样式。
    - 例如，`<div class="HyperMD-header-2 cm-line">` 元素将受到这个选择器的样式影响。

这两个选择器的主要区别在于它们的选择范围和样式应用的对象。
请注意，当类选择器连续使用时，它们表示的是多个类名同时应用在一个元素上，而不是父元素和子元素的关系。只有当一个元素同时具有所有这些类名时，样式规则才会应用。



# 一些快捷使用记录