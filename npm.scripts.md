#	NPM Scripts

模块元数据文件 package.json 中有一个 scripts 属性，我们可以在其中定义三类不同的脚本：

*	与特定 npm 命令关联的脚本；
*	在模块生命周期特定环节执行的脚本；
*	任意自定义脚本。

这三类脚本都通 npm 命令直接或间接执行。值得一提的是，npm 在执行上述脚本时，路径 ```./node_modules/.bin``` 会被添加到 PATH 环境变量中，这就是为什么有些命令在全局环境下无法使用，却可以在 npm 脚本中执行的原因（通常它们会伴随着依赖模块安装在前述路径下）。

##	命令关联脚本

我们可以在 package.json 文件中定义若干与 npm 子命令关联的脚本。当在目录模块下执行这些子命令时，相应的脚本就会被执行。这些子命令可以认为是命令接口，包括（注释引自 npm 命令的官方说明）：

```bash
npm test
# This runs a package's "test" script, if one was provided.

npm start
# This  runs  an arbitrary command specified in the package's "start" property of its "scripts" object.
# If no "start" property is specified on the "scripts" object, it will run node server.js.

npm restart
# This  runs a package's "stop", "restart", and "start" scripts, and associated pre- and post- scripts,
# in the order given below:
# 1. prerestart
# 2. prestop
# 3. stop
# 4. poststop
# 5. restart
# 6. prestart
# 7. start
# 8. poststart
# 9. postrestart

npm stop
#  This runs a package's "stop" script, if one was provided
```

##	生命周期脚本

```bash
# See manual for more details.
npm help scripts
```

除了被动触发外，生命周期脚本也可以藉由 ```run-script``` 命令显式调用，本质上和自定义脚本并无不同：
```bash
npm run-script <stage>
```

##	自定义脚本

自定义脚本通过 ```run-script``` 命令执行：
```bash
npm run-script <script-name>
```

需要注意的是，在执行自定义脚本时，npm 同样会尝试匹配并执行相应的 pre 和 post 脚本（如果存在的话）。例如：
```bash
npm run-script foo
# 依次执行 prefoo, foo， postfoo 脚本。
```
