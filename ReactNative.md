## React native js 和 native 是如何通信的
Bridge通信机制 Q:React Native Bridge的通信原理？A:Bridge采用 异步消息队列: 1.JavaScript调用原生方法时，将调用信息序列化 为JSON 2.通过Bridge传递到原生层 3.原生层解析JSON，执行对应方法 4.结果通过Bridge返回给JavaScript层 性能瓶颈: ·序列化/反序列化开销 。异步通信延迟
Q:新架构(Fabric+TurboModules)如何改进通信机 制？A:改进点: •JSI(JavaScript Interface)：同步调用原生方法 ·类型安全：编译时类型检查 ·懒加载：按需加载原生模块 ·减少Bridge依赖：直接访问原生对象

## 渲染机制 
Q:React Native 的渲染流程？
A:渲染步骤： 1. JavaScript层创建Element树 2.转换为ShadowTree(原生组件树） 3.布局计算(Yoga引擎) 4.原生UI渲染

## 你如何debug你的 react native 项目


## React Native 的线程架构？
A:三个主要线程： · Main Thread(UI Thread)：处理原生UI渲染 ·JavaScript Thread：执行业务逻辑 · Shadow Thread：布局计算 通信通过Bridge进行，存在线程间切换开销。
