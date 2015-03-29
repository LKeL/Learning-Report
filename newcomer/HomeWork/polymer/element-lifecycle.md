1、 见element的生命周期,里面有所有的生命周期的说明

2、

```
Polymer({   
	foo: 'foo',
	bar: 'bar', //这是属性么？不用publish？为什么不直接attribute？
	foobar: [],
	settings: {	
	text: 'hello foobar'
	//应该在created里面创建,否则创建多个相同的element的时候可能导致共享
	},
	created: function () {  
	},
	ready: function () {
	console.log('foobar ready!');
	},
});
```