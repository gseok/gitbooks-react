React setState와 forceUpdate 정리

`react`의 `component`관련 함수는 `component life cycle`에서 살펴보았다. 여기서는 `component`관련 함수중 life cycle에 간접적으로 영향을 주는 `setState()`와 `forceUpdate()`을 정리한다.

setState\(\)

```js
setState(nextState, callback)
```

보통 `state`을 변경\(event \| resat api 등을 통해서...\)하여, 연관된 `UI`을 `update` 할 때 사용된다. 기본적으로 현재 state을 setState에서 update하려는 `nextState`을 이용해서 `merge`하는 동작을 한다.

기존 state을 덮어 씌우는 것이 아닌 `merge`를 하는 동작을 하는이유는, 기존 state역시 object에 여러 키로 이루어져 있고, update하지 않는 `key`, `value`는 그대로 유지해야 하기 때문이다.

`nextState param`에 올 수 있는건 `Object` 또는 `Function`이다. 코드로 살펴보겠다.

nextState에 Object로 set 하는 경우.

```js
this.setState({myKey: 'my new value'});
```





nextState에 Function을 set 하는 경우.

```

```

forceUpdate\(\)

참고

[react document](https://facebook.github.io/react/docs/react-component.html)

