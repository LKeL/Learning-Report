1、::shadow 和 /deep/ 连结符能穿透 Shadow DOM 的边界而允许你从不同的 shadow trees 里给 elements 上样式,但/deep/可以完全忽略 shadow 的边界，可以穿透任意数量的 shadow trees

2、::content的作用范围是当前选择的element，比如

```
<polymer-element name="x-foo" noscript>中定义
	polyfill-next-selector { content: 'footer > p'; }
	::content footer > p {
		color: green;
}
```
在 polyfill-next-selector 的内部，添加一个content 属性。它的值必须是一个与原生的规则相同的选择器。 Polymer 会使用这个值去顶替原生的选择器	

```
<x-foo>
	<footer>
		<p>I'm green</p>
	</footer>
</x-foo>
```
footer中的文字呈绿色(让我想想怎么表述才到位)

3、有，:host 和 :host(<选择器>) 允许你在其内容的定义里设置目标和样式化一个 custom element，:host 指向 custom element 自身且有着最底层的特征。它允许用户从外部重写你的样式