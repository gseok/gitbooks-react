#### React child에서 parent의 함수로 event handler 등록하기

#### Advanced

React에서 event를 JSX로 연결 할 수 있다는 것은, 아주 손쉽게, 어떤 component에서 event에 따른 동작을 해당 component이외에, 자신을 사용하는 component \(부모 component\)에서 컨트롤 할 수 있도록 제공 가능하다는 이야기 이다.

즉 prop나 state 을 이용해서 event handler을 연결 할 수 있다는 이야기 이다.

예제로 살펴보자

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

위 예제에서는 TestParent에서 정의한 함수를 prop으로 자식에게 전달하였다.

TestChild는 부모가 전달한 함수를 자신\(Child\)의 event handler로 등록하였다.

좀 더 생각해 보면 다음과 같은 일이 가능하다.

* 자식에서 발생하는 이벤트에 따라, 부모 자신이 어떤 동작을 하게 할 수 있다.
* 자식에서 발생하는 이벤트에 따라, 자식 자신의 일을 한다음 부모에게 어떤 동작을 하게 할 수 있다.

this에 대한 bind을 좀더 응용하면 다음과 같은 것도 가능하다.

* 자식에서 바로 부모를 알 수 있다. \(부모가 this를 bind 한 상태로 전달해주면...\)
* 자식에서 자식자기 자신을 부모에게 전달하면서, 부모에게 어떤 동작을 하게 할 수 있다.

위 설명에서, 부모에게 어떤 동작을 하게 할 수 있다는 것은, 부모가 전달한 함수를 call 할 수 있다는 뜻이다.
