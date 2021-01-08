## useState

在我们import的usestate其返回的具体如下

是使用的dispatcher中useState，那dispathcer又是什么呢，在English中它译为调度员

```javascript
export function useState<S>(
  initialState: (() => S) | S,
): [S, Dispatch<BasicStateAction<S>>] {
  const dispatcher = resolveDispatcher();
  return dispatcher.useState(initialState);
}
```

发现大部分复制到dispatcher中的逻辑都在ReactFiberHooks中，

在ReactFiberHooks中又会发现dispatcher又可以分成多种，HooksDispatcherOnUpdate、HooksDispatcherOnMount、ContextOnlyDispatcher、HooksDispatcherOnRerender……

```javascript
const HooksDispatcherOnUpdate: Dispatcher = {
  readContext,
  useCallback: updateCallback,
  useContext: readContext,
  useEffect: updateEffect,
  useImperativeHandle: updateImperativeHandle,
  useLayoutEffect: updateLayoutEffect,
  useMemo: updateMemo,
  useReducer: updateReducer,
  useRef: updateRef,
  useState: updateState,
  useDebugValue: updateDebugValue,
  useDeferredValue: updateDeferredValue,
  useTransition: updateTransition,
  useMutableSource: updateMutableSource,
  useOpaqueIdentifier: updateOpaqueIdentifier,
  unstable_isNewReconciler: enableNewReconciler,
};
```

在mount阶段的useState可以发现是通过调用mountWorkInProgressHook()去创建了hooks对象然后用链表进行连接，并且保存在了当前fiber节点中的memoizedState字段中；可以在一个组件对的fiber节点保存了一个链表， 链表上的每一个节点就是一个hooks的对象。



## useEffect

useEffect则封装的useEffectImpl; 同样也会在调用mountWorkInProgressHook去创建或者是在已有链表末尾追加一个hooks对象，然后会将回调函数作为create字段，依赖字段做deps保存，并且会调用pushEffect将当前effect添加到componentUpdateQueue链表的尾端，componentUpdateQueue会在组件commit阶段中调用commitHookEffectListMount。

```javascript

function pushEffect(tag, create, destroy, deps) {
  const effect: Effect = {
    tag,
    create,
    destroy,
    deps,
    // Circular
    next: (null: any),
  };
  let componentUpdateQueue: null | FunctionComponentUpdateQueue = (currentlyRenderingFiber.updateQueue: any);
  if (componentUpdateQueue === null) {
    componentUpdateQueue = createFunctionComponentUpdateQueue();
    currentlyRenderingFiber.updateQueue = (componentUpdateQueue: any);
    componentUpdateQueue.lastEffect = effect.next = effect;
  } else {
    const lastEffect = componentUpdateQueue.lastEffect;
    if (lastEffect === null) {
      componentUpdateQueue.lastEffect = effect.next = effect;
    } else {
      const firstEffect = lastEffect.next;
      lastEffect.next = effect;
      effect.next = firstEffect;
      componentUpdateQueue.lastEffect = effect;
    }
  }
  return effect;
}
```



