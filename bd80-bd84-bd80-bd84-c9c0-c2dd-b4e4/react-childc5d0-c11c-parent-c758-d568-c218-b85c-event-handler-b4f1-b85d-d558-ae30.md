#### React child에서 parent의 함수로 event handler 등록하기

React에서 `event`를 `JSX`로 연결 할 수 있다는 것은, **아주 손쉽게**, 어떤 `component`에서 `event`에 따른 동작을 해당 `component`이외에, 자신을 사용하는 `component (부모 component)`에서 컨트롤 할 수 있도록 제공 가능하다는 이야기 이다.

즉 `prop`나 `state` 을 이용해서 `event handler`을 연결 할 수 있다는 이야기 이다.

##### 예제로 살펴보자

```js
class TestChild extends React.Component {
  constructor(props) {
    super(props);

    this.ownHandler = this.ownHandler.bind(this);
  }

  ownHandler () {
    // TestChild own handler
  }

  render() {
    // 부모에서 props로 전달한 parentPassedHandler에
    // 부모가 자신(this)을 bind 했으면, 해당 함수가 called 되었을때,
    // 해당 함수의 this는 부모임.
    const parentPassedHandler = this.props.parentPassedHandler;

    return (
      <button onClick={this.ownHandler}>
      </button>
      <button onClick={parentPassedHandler}>
      </button>
    );
  }
}

class TestParent extends React.Component {
  parentHandler () {
    // TestParent's defined handler
  }

  render() {
    const parentPassedHandler = this.parentPassedHandler.bind(this);

    return (
      <TestChild parentPassedHandler={parentPassedHandler}/>
    );
  }
}
```

위 예제에서는 `TestParent`에서 정의한 함수를 `prop`으로 자식에게 전달하였다.

`TestChild`는 부모가 전달한 함수를 자신\(Child\)의 `event handler`로 등록하였다.

좀 더 생각해 보면 다음과 같은 일이 가능하다.

* 자식에서 발생하는 이벤트에 따라, 부모 자신이 어떤 동작을 하게 할 수 있다.
* 자식에서 발생하는 이벤트에 따라, 자식 자신의 일을 한다음 부모에게 어떤 동작을 하게 할 수 있다.



위 설명에서, 부모에게 어떤 동작을 하게 할 수 있다는 것은, 부모가 전달한 함수를 call 할 수 있다는 뜻이다.

