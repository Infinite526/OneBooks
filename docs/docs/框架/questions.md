# 常见问题

1. **写 React / Vue 项目时为什么要在列表组件中写 key，其作用是什么？**

>  [参考](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/1)

- 不用 key：
  **就地复用节点。**在比较新旧两个节点是否是同一个节点的过程中会判断成新旧两个节点是同一个节点，因为 a.key 和 b.key 都是 undefined。所以不会重新创建节点和删除节点，只会在节点的属性层面上进行比较和更新。所以可能在某种程度上（创建和删除节点方面）会有渲染性能上的提升；

  **无法维持组件的状态。**由于就地复用节点的关系，可能在维持组件状态方面会导致不可预知的错误，比如无法维持改组件的动画效果、开关等状态；

  **也有可能会带来性能下降。**因为是直接就地复用节点，如果修改的组件，需要复用的很多节点，顺序又和原来的完全不同的话，那么创建和删除的节点数量就会比带 key 的时候增加很多，性能就会有所下降；

- 用 key：
  **维持组件的状态，保证组件的复用。**因为有 key 唯一标识了组件，不会在每次比较新旧两个节点是否是同一个节点的时候直接判断为同一个节点，而是会继续在接下来的节点中找到 key 相同的节点去比较，能找到相同的 key 的话就复用节点，不能找到的话就增加或者删除节点。

  **查找性能上的提升。**有 key 的时候，会生成 hash，这样在查找的时候就是 hash 查找了，基本上就是 O(1) 的复杂度。

  **节点复用带来的性能提升。**因为有 key 唯一标识了组件，所以会尽可能多的对组件进行复用（尽管组件顺序不同），那么创建和删除节点数量就会变少，这方面的消耗就会下降，带来性能的提升。

- **总结**：性能提升不能只考虑一方面，不是 diff 快了性能就快，不是增删节点少了性能就快，不考虑量级的去评价性能，都只是泛泛而谈。

2. **vue 的diff算法和react的diff算法有何区别？**
   
- vue 和 react 的 diff 算法有相同和有不同，相同是都是用同层比较，不同是 vue使用双指针比较，react 是用 key 集合级比较
   
   - vue 的 diff算法
   
     [解析vue2.0的diff算法](https://github.com/aooy/blog/issues/2)
   
     ```javascript
     function patch (oldVnode, vnode) {
     	if (sameVnode(oldVnode, vnode)) {
     		patchVnode(oldVnode, vnode)
     	} else {
     		const oEl = oldVnode.el
     		let parentEle = api.parentNode(oEl)
     		createEle(vnode)
     		if (parentEle !== null) {
     			api.insertBefore(parentEle, vnode.el, api.nextSibling(oEl))
     			api.removeChild(parentEle, oldVnode.el)
     			oldVnode = null
     		}
     	}
     	return vnode
     }
     // 比较的是key和sel
     function sameVnode(oldVnode, vnode){
     	return vnode.key === oldVnode.key && vnode.sel === oldVnode.sel
     }
     
     function patchVnode (oldVnode, vnode) {
         const el = vnode.el = oldVnode.el
         let i, oldCh = oldVnode.children, ch = vnode.children
         if (oldVnode === vnode) return
         // 文本节点 
         if (oldVnode.text !== null && vnode.text !== null && oldVnode.text !== vnode.text) {
             api.setTextContent(el, vnode.text)
         }else {
             updateEle(el, vnode, oldVnode)
         	if (oldCh && ch && oldCh !== ch) {// 两个节点都有子节点，且他们不同
     	    	updateChildren(el, oldCh, ch)
     	    }else if (ch){//只有新的节点有子节点
     	    	createEle(vnode)
     	    }else if (oldCh){//只有旧的节点有子节点
     	    	api.removeChildren(el)
     	    }
         }
     }
     ```
   
     
   
3. **react fiber的原理**

   `Fiber Reconciler` 是为了解决 React v15 的DOM元素多，频繁刷新场景下的主线程阻塞问题，直观显示，则是“掉帧”问题。v15 是一次同步处理整个组件树，通过递归的方式进行渲染，使用 JavaScript 引擎自身的函数调用栈，它会一直执行到栈空位置，一旦工作量大就会阻塞整个主线程。

   然而我们的更新工作可能并不需要一次性同步完成，其中是可以按照优先级调整工作，把整个过程分片处理的，这就是 Fiber 想做的事。

   `Fiber Reconciler` 以**链表**的形式遍历组件数，可以灵活的暂停、继续、放弃当前任务。通过 Scheduler 调度器来进行任务分配，每次只做一个小任务，通过 `requestIdleCallback` 回到主线程看看有没有更高优先级的任务需要处理，如果有就暂停当前任务，去做优先级更高的，否则就继续执行。

4. **为什么react需要fiber而vue不需要？**

5. **React 16 中 Diff 算法的变化**

6. **React 冷启动很慢，为什么，哪些地方可以优化**

