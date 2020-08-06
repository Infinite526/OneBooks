# 常见问题

1. 对tree-shaking的了解？
   - 虽然生产模式下默认开启，但是由于经过了babel编译，全部模块都被封装成IIFE，所以其实shaking的程度并不如我们想象的那么多
   - es6是静态加载的，所以它可以在代码不运行的条件下就能分析出不需要使用的代码，而commonjs是动态加载的，对shaking支持不够
   - 使用过的export被标记为/ * harmony export ([type]) * /，没有被使用过的export被标记为/ * unused harmony export [FuncName] * /
   - IIFE存在副作用无法被tree-shaking掉
   - sideEffect决定了副作用的处理方式，可以提高有效代码的纯粹度。为false的时候，表示可以安全的删除未使用的export；为true或者一个文件数组的时候，表示这个文件可以有副作用，不能将它删除。
   
   参考：
   
   [Tree-Shaking性能优化实践 - 原理篇](https://juejin.im/post/6844903544756109319)
   
   [Tree Shaking原理 -【webpack进阶系列】](https://segmentfault.com/a/1190000022194321)
   
   推荐 - [你的Tree-shaking并没什么卵用](https://segmentfault.com/a/1190000012794598)
   
2. commonJs和es6 module的区别？
   - commonjs是加载时运行，而es module是编译时运行
   - commonjs输出的是值的浅拷贝，es module是值的引用
   - webpack的webpack_require对他们的处理方式不同
   
3. 前端大型文件上传优化？
   - 文件切片
   
   - 用web-worker单独计算文件的hash值
   
   - 进度条处理

   - 对已经传过的文件跳过秒传，对失败的文件做重传处理
   
   - 上传由于和其他接口同一域名，所以要做并发数处理
   
     https://juejin.im/post/6844904046436843527
   
4. hmr实现原理

5. JSBridge 原理

6. CDN 的特点，用 CDN 资源为什么快

7. 如何自己实现一个组件按需加载

8. Sentry 监控错误的原理是什么

9. WebAssembly 是什么，一般用来做什么

10. 扫码登录的实现逻辑