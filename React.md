## 1. useEffect 在什么时候被调用

简单来说，useEffect 在 React 完成对 DOM 的渲染和更新之后被异步调用。它不会阻塞浏览器的绘制

## 2. useEffect 的清理函数在什么时候被调用

清理函数的调用时机是：
组件卸载（Unmount）时：当组件从屏幕上移除时，清理函数会被调用，以防止内存泄漏（例如清除定时器、取消订阅、移除事件监听器）。
下一次 effect 执行前：如果 effect 因为依赖项变化需要再次执行，那么在执行新的 effect 之前，上一次 effect 返回的清理函数会先被调用。这确保了每次 effect 都是在一个“干净”的环境下运行。

## 3. 什么时候需要 useEffect 

你需要 useEffect 的核心信号是：当你的组件需要与 React 生态系统之外的世界进行交互或同步时。
组件的主要工作是接收 props 和 state，然后返回 JSX 用于渲染 UI。这是一个纯粹的计算过程。而所有不属于这个计算过程的“额外操作”，都应该被视为副作用（Side Effects），放进 useEffect 中。

以下是几个最典型的场景：
1. 与外部系统同步：这是最核心的用途。
获取数据 (Data Fetching)：当组件需要从服务器 API 获取数据来展示时。组件先渲染一个加载状态，useEffect 在渲染后执行数据请求，请求成功后更新 state，触发组件用新数据重新渲染。
设置订阅 (Setting up a subscription)：连接到一个 WebSocket 服务、监听浏览器的在线/离线状态、或者订阅一个外部 store。你需要在组件挂载时开始订阅，在卸载时取消订阅（这就要用到 useEffect 的清理函数）。
与第三方库交互：如果你在使用一个不受 React 控制的第三方 DOM 库（比如一个图表库或动画库），你需要在 useEffect 中初始化它并与之通信。

2. 操作不受 React 直接控制的 DOM
虽然 React 鼓励通过 JSX 来声明式地管理 DOM，但有时你仍需要手动操作，比如动态修改页面标题 (document.title)、手动管理焦点等。这些都属于副作用。

3. 设置定时器或延时操作
使用 setTimeout 或 setInterval 都是典型的副作用。你需要在 useEffect 中设置它们，并且必须在清理函数中用 clearTimeout 或 clearInterval 来清除它们，防止组件卸载后定时器仍在运行导致内存泄漏和 bug。

>一个简单的判断法则：
>问问自己：“我正在写的这段代码，是不是为了响应用户的某个直接操作（比如点击、输入）？”
>如果是，它应该放在事件处理函数里。
>如果不是，而是为了响应组件的渲染、props 或 state 的变化，并且需要与“外部世界”交互，那它就应该放在 useEffect 里。

## 4. 是不是所有的 setState 都需要放在 useEffect 里？

不是，恰恰相反，绝大多数的 setState 都不应该、也不需要放在 useEffect 里

## 5. setState 是异步的还是同步的
在React控制的事件中，更新会被批处理 ·在setTimeout、Promise等异步操作中，React 18 前是同步更新 ·通过isBatchingUpdates标识控制批处理


## 6. context API 用处，可能会导致的问题，怎么解决
Context性能问题 Q:Context更新导致的性能问题及解决方案？A:问 题：Context值改变会重渲染所有消费组件 解决方案: // 1.拆分Context const StateContext = createContext(); const DispatchContext = createContext(); // 2.使用useMemo优化 const value = useMemo((）=>({

## 7. 什么是 react fiber? 它如何解决主线程 阻塞问题？A:Fiber架构特点: ·时间切片：将渲染工作分割成小块，可中断 ·优先级调度：不同更新有不同优先级 ·双缓存：current树和workInProgress树 工作流程: 1.Scheduler调度：根据优先级决定何时执行任务 2.Reconciler协调：构建Fiber树，可中断 3.Renderer渲染：提交DOM更新，不可中断 中断机制：通过requestIdleCallback或 MessageChannel实现时间切片


## 8. react query 

## 9. monorepo vs micro frontend 

## 
