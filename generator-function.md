#	Generator Function

Generator Function 是从 4.x 版本开始引入的，当时必须开启 ```--harmony``` 开关才能使用。异步执行、非阻塞式的设计，是 JavaScript 作为脚本语言，却能在 Node.js 运行时环境下保持高性能的关键，但在某些不苛求性能的场景下，也成为程序设计的一大障碍。借助 Generator Function 独特的 yield 构造，开发人员可以编写同步风格的 Node.js 程序，而不必在回调函数中层层嵌套。

##	ERROR: Generator is already running

这是 v6.9.1 版本在和谐模式下运行时出现的一个现象。来看源代码

```javascript
// foo.js
'use strict';

function foo() {
	console.log('foo invoked');
}

var G = function*() {
	// KEY LINE
	return foo();
}

var gen = G();
console.log(gen.next());

try {
	gen.next();
} catch(ex) {
	console.log('[ERROR] ' + ex.message);
}
```

普通模式下执行上述程序的输出如下：
```bash
node foo.js
# OUTPUT
# foo invoked
# { value: undefined, done: true }
# { value: undefined, done: true }
```

但是打开 ```--harmony``` 开关后，问题出现了：

```bash
node -v
# OUTPUT:
# v6.9.1

node --harmony foo.js
# OUTPUT:
# foo invoked
# undefined
# [ERROR] Generator is already running
```

可以看到，差异有二：
*	```next()``` 方法没有返回值；
*	溢出执行 ```next()``` 方法会导出错误。

对于借助生成函数编写同步风格程序的设计来说，这种差异具有相当的破坏性。所幸，这个问题在 v6.0.0、v7.0.0 中都不会出现。
