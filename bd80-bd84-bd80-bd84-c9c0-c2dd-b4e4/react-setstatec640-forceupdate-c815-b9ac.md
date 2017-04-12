### React setState와 forceUpdate 정리

`react`의 `component`관련 함수는 `component life cycle`에서 살펴보았다. 여기서는 `component`관련 함수중 life cycle에 간접적으로 영향을 주는 `setState()`와 `forceUpdate()`을 정리한다.

#### setState\(\)

```js
setState(nextState, callback)
```

보통 `state`을 변경\(event \| resat api 등을 통해서...\)하여, 연관된 `UI`을 `update` 할 때 사용된다. 기본적으로 현재 state을 setState에서 update하려는 `nextState`을 이용해서 `merge`하는 동작을 한다.

기존 state을 덮어 씌우는 것이 아닌 `merge`를 하는 동작을 하는이유는, 기존 state역시 object에 여러 키로 이루어져 있고, update하지 않는 `key`, `value`는 그대로 유지해야 하기 때문이다.

`nextState param`에 올 수 있는건 `Object` 또는 `Function`이다. 코드로 살펴보겠다.



##### nextState에 Object로 set 하는 경우.

```js
this.setState({myKey: 'my new value'});
```

가장 기본적인 사용이다. 정상적으로 state가 변경되고 나면, component life cycle 로직\(`shouldComponentUpdate()`\)을 타게 된다.



##### nextState에 Function을 set 하는 경우.

```js
this.setState((prevState, props) => {
  return {myInteger: prevState.myInteger + props.step};
});
```

보통 이전 state값을 이용하면서 state값을 update할때 사용된다. 기본사용은 이전 state값을 merge해버리는 반면 명확하게, 이전값을 사용하거나, 이전값 비교등이 필요할때 사용하면 좋다.

Fucntion형태로 set할때 setState가 동작하면서, function에 preveState\(이전 state\)와 props\(현재 props\)을 전달해준다.



##### state의 callback을 명시한 경우.

```js
this.setState({myKey: 'my new value'}, () => {
  console.log('setState complete');
});

또는

this.setState((prevState, props) => {
  return {myInteger: prevState.myInteger + props.step};
}, () => {
  console.log('setState complete');
});
```



#### forceUpdate\(\)

#### 참고

[react document](https://facebook.github.io/react/docs/react-component.html)

