#	Generator Function

Generator Function 是从 4.x 版本开始引入的，当时必须开启 ```--harmony``` 开关才能使用。异步执行、非阻塞式的设计，是 JavaScript 作为脚本语言，却能在 Node.js 运行时环境下保持高性能的关键，但在某些不苛求性能的场景下，也成为程序设计的一大障碍。借助 Generator Function 独特的 yield 构造，开发人员可以编写同步风格的 Node.js 程序，而不必在回调函数中层层嵌套。

##	ERROR: Generator is already running

###	问题解读
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

对于借助生成函数编写同步风格程序的设计来说，这种差异具有相当的破坏性。

###	 版本比对

我们对比了一下 Node.js 版本：

| Version | --harmony | 测试结果 |
| :------------- | :------------- | :------------- |
| * | closed | normal |
| 6.4.0 | open | normal |
| ^6.5.0 | open | EXCEPTION |
| 7.0.0 | open | normal |

也就是说，这个问题只有在 6.5.0（含）以上，7.0.0（不含）以下版本的和谐模式中，才会出现。

###	深入剖析

对比一下两个版本下 V8 引擎的 harmony 特性，不知是否可以发现端倪？
```bash
# Here -- tells command grep the next parameters are not options.
node --v8-options | grep -- --harmony

# OUTPUT under node v6.5.0
--harmony (enable all completed harmony features)
--harmony_shipping (enable all shipped harmony features)
--harmony_array_prototype_values (enable "harmony Array.prototype.values" (in progress))
--harmony_object_observe (enable "harmony Object.observe" (in progress))
--harmony_function_sent (enable "harmony function.sent" (in progress))
--harmony_sharedarraybuffer (enable "harmony sharedarraybuffer" (in progress))
--harmony_simd (enable "harmony simd" (in progress))
--harmony_do_expressions (enable "harmony do-expressions" (in progress))
--harmony_regexp_property (enable "harmony unicode regexp property classes" (in progress))
--harmony_string_padding (enable "harmony String-padding methods" (in progress))
--harmony_regexp_lookbehind (enable "harmony regexp lookbehind")
--harmony_tailcalls (enable "harmony tail calls")
--harmony_object_values_entries (enable "harmony Object.values / Object.entries")
--harmony_object_own_property_descriptors (enable "harmony Object.getOwnPropertyDescriptors()")
--harmony_exponentiation_operator (enable "harmony exponentiation operator `**`")
--harmony_function_name (enable "harmony Function name inference")
--harmony_instanceof (enable "harmony instanceof support")
--harmony_iterator_close (enable "harmony iterator finalization")
--harmony_unicode_regexps (enable "harmony unicode regexps")
--harmony_regexp_exec (enable "harmony RegExp exec override behavior")
--harmony_sloppy (enable "harmony features in sloppy mode")
--harmony_sloppy_let (enable "harmony let in sloppy mode")
--harmony_sloppy_function (enable "harmony sloppy function block scoping")
--harmony_regexp_subclass (enable "harmony regexp subclassing")
--harmony_restrictive_declarations (enable "harmony limitations on sloppy mode function declarations")
--harmony_species (enable "harmony Symbol.species")
--harmony_instanceof_opt (optimize ES6 instanceof support)

# OUTPUT under node v6.4.0
--harmony (enable all completed harmony features)
--harmony_shipping (enable all shipped harmony features)
--harmony_object_observe (enable "harmony Object.observe" (in progress))
--harmony_modules (enable "harmony modules" (in progress))
--harmony_function_sent (enable "harmony function.sent" (in progress))
--harmony_sharedarraybuffer (enable "harmony sharedarraybuffer" (in progress))
--harmony_simd (enable "harmony simd" (in progress))
--harmony_do_expressions (enable "harmony do-expressions" (in progress))
--harmony_iterator_close (enable "harmony iterator finalization" (in progress))
--harmony_tailcalls (enable "harmony tail calls" (in progress))
--harmony_object_values_entries (enable "harmony Object.values / Object.entries" (in progress))
--harmony_object_own_property_descriptors (enable "harmony Object.getOwnPropertyDescriptors()" (in progress))
--harmony_regexp_property (enable "harmony unicode regexp property classes" (in progress))
--harmony_function_name (enable "harmony Function name inference")
--harmony_regexp_lookbehind (enable "harmony regexp lookbehind")
--harmony_species (enable "harmony Symbol.species")
--harmony_instanceof (enable "harmony instanceof support")
--harmony_default_parameters (enable "harmony default parameters")
--harmony_destructuring_assignment (enable "harmony destructuring assignment")
--harmony_destructuring_bind (enable "harmony destructuring bind")
--harmony_tostring (enable "harmony toString")
--harmony_regexps (enable "harmony regular expression extensions")
--harmony_unicode_regexps (enable "harmony unicode regexps")
--harmony_sloppy (enable "harmony features in sloppy mode")
--harmony_sloppy_let (enable "harmony let in sloppy mode")
--harmony_sloppy_function (enable "harmony sloppy function block scoping")
--harmony_proxies (enable "harmony proxies")
--harmony_reflect (enable "harmony Reflect API")
--harmony_regexp_subclass (enable "harmony regexp subclassing")
```
