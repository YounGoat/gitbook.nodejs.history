#	glob & minimatch

NPM [minimatch](https://www.npmjs.com/package/minimatch) 提供通配符匹配功能，而 [glob](https://www.npmjs.com/package/glob) 是基于 minimatch 实现的文件检索模块，两者都是 Node.js 中很常用的模块。

根据官方说明，minimatch 支持三大类通配符语法：
*	Brace Expansion
*	Extended glob matching
*	"Globstar" ** matching

##	What is glob?

西方的程序员很喜欢在计算机术语中使用明喻、隐喻甚至暗示。有些源于创始者的灵机一动，比如 cookie；有些诞自特定的文化背景，比如 breadcrumbs；还有一些，只能归因于语言本身的差异，比如 glob，以英语为母语的程序员很容易意会，则持其他语言、特别是非拉丁语系的程序员们往往只能强记。

*glob* 本意是“一团”，[韦氏词典](http://www.merriam-webster.com/dictionary/glob)是这样解释的：
>	a large, round drop of something soft or wet

[Computer Hope](http://www.computerhope.com/jargon/g/glob.htm) 的解释比较直白：
>	Glob is a term used to describe the expansion or the match of values returned when using wildcards, regular expressions, or other pattern matches.

##	快速入门

```javascript
var minimatch = require('minimatch');

minimatch('foo.js', '*.js');
// RETURN: ture

minimatch('foo/bar/quz.js', '*');
// RETURN: false

minimatch('foo/bar/quz.js', '**');
// RETURN: true

minimatch('foo/bar/quz.js', '**.js');
// RETURN: false

minimatch('foo/bar/quz.js', '**/*.js');
// RETURN: true
```

##	在线资源

*	WIKIPEDIA, glob (programming)  
	[https://en.wikipedia.org/wiki/Glob_(programming)](https://en.wikipedia.org/wiki/Glob_(programming)
