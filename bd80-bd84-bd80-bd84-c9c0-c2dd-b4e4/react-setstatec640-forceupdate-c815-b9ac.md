React setState와 forceUpdate 정리

react의 component관련 함수는 component life cycle에서 살펴보았다. 여기서는 component관련 함수중 life cycle에 간접적으로 영향을 주는 setState\(\)와 forceUpdate\(\)을 정리한다.



setState\(\)

```js
setState(nextState, callback)
```

보통 state을 변경\(event \| resat api 등을 통해서...\)하여, 연관된 UI을 update 할 때 사용된다. 기본적으로 현재 state을 setState에서 update하려는 nextState을 이용해서 merge하는 동작을 한다.

nextState param에 올 수 있는건 Object 또는 Function이다. 코드로 살펴보겠다.



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

