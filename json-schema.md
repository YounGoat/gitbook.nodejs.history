#	JSON Schema

>	JSON Schema is a vocabulary that allows you to annotate and validate JSON documents.  
>	See http://json-schema.org

##	JSON 格式规范

JSON 通常是作为轻量级信息载体来使用，故而我们对于 JSON 数据在应用层面的良构性并没有给予太多的关注，只要符合 JSON 的基本语法要求，就认为数据是“合法的”。与 YAML 相比，JSON 本身抗干扰能力较强，最容易出现的格式问题往往与我们习惯了 JavaScript 的书写定势有关，包括：

*	__属性名省略定界符__  
	JavaScript 对象是允许属性名裸露的，但是 JSON 不可以。

*	__属性名或字符串字面量使用单引号作定界符__  
	JavaScript 对象允许使用单引号作为字符串定界符，但是 JSON 不可以。

*	__插入注释__  
	JSON 中不支持任何注释。

举例说明：
```javascript
/**
 * 本段作为 JavaScript 代码（对象）是合法的，但不符合 JSON 格式规范。
 *（块注释本身亦不合法）
 */
{
	// 属性名未加定界符，非法！（本行注释本身亦不合法）
	name: "Jack",

	// 使用单引号作为定界符，非法！（本行注释本身亦不合法）
	"gender": 'male'
}
```

##	应用数据的良构性

如果你用过 X(ML) S(chema) D(efinition)，那么你一定能够明白 JSON Schema 是用来干什么。简言之，JSON Schema 用来描述（或者说约束）你的 JSON 数据应该是什么样子的。

##	在线资源

*	[Understanding JSON Schema](https://spacetelescope.github.io/understanding-json-schema/)
