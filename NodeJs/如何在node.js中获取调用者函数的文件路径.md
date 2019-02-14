以下是三个文件的示例代码：
```
// foo.js
var myFunc = require("./myFunc");
function foo(){
   myFunc("message");
}

// bar.js
var myFunc = require("./myFunc");
function bar(){
   myFunc("message");
}

// myFunc.js
module.exports = myFunc;
function myFunc(arg1){
   console.log(arg1);
   // Here I need the file path of the caller function
   // For example, "/path/to/foo.js" and "/path/to/bar.js"
}
```

我需要动态获取调用者函数的文件路径，而不需要任何额外的参数传递给myFunc。

你需要摆脱v8的内部工作。见：[the wiki entry about the JavaScript Stack Trace API](http://code.google.com/p/v8/wiki/JavaScriptStackTraceApi)。

我在a proposed commit的一些代码基础上进行了一些测试，似乎有效。你最终得到一个绝对的路径。
```
// omfg.js

module.exports = omfg

function omfg() {
  var caller = getCaller()
  console.log(caller.filename)
}

// private

function getCaller() {
  var stack = getStack()

  // Remove superfluous function calls on stack
  stack.shift() // getCaller --> getStack
  stack.shift() // omfg --> getCaller

  // Return caller's caller
  return stack[1].receiver
}

function getStack() {
  // Save original Error.prepareStackTrace
  var origPrepareStackTrace = Error.prepareStackTrace

  // Override with function that just returns `stack`
  Error.prepareStackTrace = function (_, stack) {
    return stack
  }

  // Create a new `Error`, which automatically gets `stack`
  var err = new Error()

  // Evaluate `err.stack`, which calls our new `Error.prepareStackTrace`
  var stack = err.stack

  // Restore original `Error.prepareStackTrace`
  Error.prepareStackTrace = origPrepareStackTrace

  // Remove superfluous function call on stack
  stack.shift() // getStack --> Error

  return stack
}
```
并且包括omfg模块的测试：
```
#!/usr/bin/env node
// test.js

var omfg = require("./omfg")

omfg()
```
而您将在控制台上得到test.js的绝对路径。

说明

这不是一个“node.js”问题，因为它是一个“v8”问题。

见：[Stack Trace Collection for Custom Exceptions](http://code.google.com/p/v8/wiki/JavaScriptStackTraceApi#Stack_trace_collection_for_custom_exceptions)

[Error.captureStackTrace(error, constructorOpt)](https://github.com/nodejs/node-v0.x-archive/blob/c3ca783525c3de368572a12e627e56090e49f594/deps/v8/src/messages.js#L1112) 将错误参数添加到堆栈属性，该属性的默认值为String(通过 [FormatStackTrace](https://github.com/joyent/node/blob/c3ca783525c3de368572a12e627e56090e49f594/deps/v8/src/messages.js#L1052) )。如果 `Error.prepareStackTrace(error，structuredStackTrace)`是一个`Function`，则调用它而不是`FormatStackTrace`。

所以，我们可以用我们自己的函数来覆盖`Error.prepareStackTrace`，它将返回任何我们想要的结果 – 在这种情况下，只有`structStackTrace`参数。

然后，`structStackTrace [1].receiver` 是一个表示调用者的对象。