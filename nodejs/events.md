
### 事件驱动
大多数Node.js核心API都是采用异步架构，异步靠事件驱动实现。
事件驱动首先注册事件和监听器，然后触发事件，执行监听器
- 所有能触发事件的对象都是EventEmitter类的实例

### 注册监听器
- 一个事件可以注册多个监听器
- 多个事件同步被触发
- 监听器的返回值会被忽略并丢弃
``` javascript
myEmitter.on('event', () => {
	// business
});
```

### 触发事件
``` javascript
myEmitter.emit('event');
```
