1、[[]]和{{}}的差别，[[]]是一次性绑定，会在polymer第一次设置其值后停止绑定,但就无法设置属性监视器

2、我尝试用{{buttonClick|buttonJudge}}来尝试事件消息绑定但是并没有反应，在buttonJudge中的console.log也没有执行，所以目前觉得不能这么用

3、publish自定义属性及其默认值，如在属性中定义attribute="name"，我们就可以在下面的publish中publish:{name:"hehe"},也可以直接写出publish而不写attribute，通常情况下，我们的element有多个属性且把它们写成一行显得很笨拙。想定义属性的默认值，且追求在一处完成所有重复的工作。需要把属性的变化反射回相应的特性。我们才使用publish，api中建议写上attribute
reflect为属性变换反射，比如

```
publish:{	
	name: {
		reflect: true
	}
}
```

之后我们设定`<my-element name="somename"> </my-element>`,我们如果改变name的值，那在dom中会更新我们改变的属性值

4、this.$.xxx 可以获取一个xxx element,那么this.$.foobar时获取自定义element上id为foobar的element

5、两种监视方式。如numberChanged，也可以自定义属性监视者。使用observe代码块来统一监视多个属性

**进阶问题**

1、ref可以把一个template重用在多个地方，或引用一个别处生成的template,第二问是可以直接用bind ref导入一个树形控件？这题不理解

2、消息？有声明式的事件映射，on—even方法，也有自定义事件，polymer提供了一个fire()的方法，比如
 
```
qonClick: function() {
	this.fire('嗷', {msg: '又伤自尊了！'}); 
	// fire(inType, inDetail, inToNode)
}
```
3、可以，比如如，

```
input.bind('value',new PathObserver(obj,'path.to.value'));
```
完整路径为，obj.path.to.value.，由于这个方法位于，HTMLElement上，故所有的HTML元素都具有这个方法，并且可以自定义bind方法,也可以用`defineProperty`来修改属性

4、当载入页面的时候，valueChanged被触发，但我们点击button后valueChanged没有被触发，我们可以在

```
tmpl.clickAction = function ( event ) {
	tmpl.foo.data.x = 50;
}//将tmpl.foo = { data: { x: 50, y: 30 }, name: "foobar" }，
```
但这种方法显然很傻，那我们就要添加一个监视器

```
observe:{
	'value.data.x': 'valueDataX',
},
valueDataX:function() {
	console.log("X changed");
},
```
这样但点击button时候就会显示X changed,但是我觉得这种方式仍然很笨拙，我再想想
5、同样修改observe如题4，对vlaue.data.x和value.data.y监视