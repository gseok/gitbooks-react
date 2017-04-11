### React, Props vs State 개념 정리

React에서 props와 state는 개념과 사용이 서로 다르다. 해당 내용을 정리한다.

#### React Props

##### **Props are Read-Only**

참고: [https://facebook.github.io/react/docs/components-and-props.html](https://facebook.github.io/react/docs/components-and-props.html)

공식 document에 아래와 같이 정의 되어 있다.

React is pretty flexible but it has a single strict rule:

> **All React components must act like pure functions with respect to their props.**

React에서 Props는 component의 수명주기동안 불변이라고 생각 하면된다.

따라서 component 내부에서 변경해서는 안된다. 절대 안된다. 그냥 Read-Only로 봐야 한다.

이전 버전에서는 `setProps`나 `replaceProps` 함수를 제공하였지만, 모두 `deprecated`되었다.

##### Props 생성

React에서 props는 기본적으로 자동 생성해 준다. 우리가 별도의 코드를 작성하지 않아도. 컴포넌트 객체에 props가 생성된다.

```js
class Child extends React.Component {
    render() {
        return <h1>Hello, {this.props.name}</h1>;
    }
}

위 코드는 사실 아래 코드와 동일 하다

class Child extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        return <h1>Hello, {this.props.name}</h1>;
    }
}
```

##### **Props 설정 및 사용**

한번 설정하면, Read-Only로만 사용하는 props는 두가지 방법으로 설정 가능하다.

1. 부모로 부터 설정 \(부모가 해당 component\)을 생성할때 값을 전달해서 설정
2. 기본값을 설정하여, 기본값으로 동작하도록 설정

```js
class Child extends React.Component {
    render() {
        return <h1>Hello, {this.props.name}</h1>;
    }
}

// props의 기본값 설정
Child.defaultProps = {
    name: 'Anyone...'
}


class Parent extends React.Component {
    render() {
        // props의 값을 부모에서 전달해서 설정
        return <Child name='gseok' />
    }
}
```

#### React State

##### **React State is changeable**

참고: [https://facebook.github.io/react/docs/state-and-lifecycle.html](https://facebook.github.io/react/docs/state-and-lifecycle.html)

공식 document에 아래와 같이 정의되어 있다.

> State allows React components to change their output over time in response to user actions, network responses, and anything else, without violating this rule.

React에서 State는 가변적인 값을 표현하는 방법이다. React에서는 기본적을오 어떤 값을 받아서, UI을 처리하는 부분을 Props로 가능하게 하고 있다.

하지만 Props는 고정에 불변적이다. 코드를 작성하다보면, 당연 동적으로 변화하는 값을 사용해야 하는 경우가 필요하다. 이러한 경우를 위해 State을 제공한다.

##### State 생성

React에서 state는 props와 다르게 자동 생성되지 않는다. 즉 아무 설정 없이 코드를 작성하면, state가 없는 상태가 된다. 따라서 props와 다르게, state을 생성하기 위해서는 생성자에서 명시적으로 state을 기술 해야 한다.

```js
  constructor(props) {
    super(props);
    this.state = {date: new Date()}; // 이시점에서 설정한 값이 state의 기본값이 된다.
  }
```

주의점, `getInitializeState`은 `deprecated` 되었다. 따라서 항상 state의 init은 생성자에서 이루어져야 한다.

##### **State 설정 및 사용**

React에서 state는 가변적으로 변경 가능한 값이기 때문에, 값을 `set` 하는게 가능하다 \(변경하는게 가능\)

하지만 **state에 직접적으로 값을 설정하면 안된다**.!!!!!!!!

```js
// 잘못된 방법, 절대 이렇게 하지 말자

this.state.comment = 'Hello';
```

this.state 에 직접적으로 값을 설정 할 수 있는건 유일하게, 처음 state을 초기화 하는 생성자\(constructor\)에서만 가능하다. 그 이외에서는 직접 설정하면 안된다.

**올바른 state 설정은 `setState` 함수를 사용하는것이다.**

```js
// 올바른 방법
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));

// 아래 코드는, 위에 코드와 동일, 이전 문법으로 작성

// 올바른 방법
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});
```

여기서 주의 할 점. `setState`는 `callback function` 형태로 이루어져 있다... WHY?

공식 문서에 다음과 같이 설명되어 있다.

> React may batch multiple`setState()`calls into a single update for performance.

즉 setState는 비동기적으로 구동되고, 성능을 위해서, 여러개의 setState가 실질적으로는 한번만 수행 될 수있다.

**setState을 할 때는 prevState에 setting해야 함을 꼭 명시하자**

state의 set이 비동기적이기 때문에 아래와 같이 this.state을 갱신하면, 동기화를 깨트릴 수 있다.

```js
// 잘못된 state update
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

따라서, state을 update해야 하는 경우 꼭 previous state을 통해 갱신을 해야 한다.

```js
// 올바른 방법, prevState을 통해서 update해야함
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```



#### 정리

* props, state 모두 component에서 사용가능한 정보를 담을 수 있다.
* props는 read-only 이고, 부모로 부터 설정되거나, 기본값으로만 한번 설정된다. \(불변\)
* state는 change가능하고, component에서 처음 init을 해야만 설정된다.
* state의 설정은 setState라는 함수를 통해서 해야하고, 해당 함수는 비동기로 동작한다. 따라서 prevState에 설정하는 형태를 취해야 한다.



#### 참고

[props vs state](https://github.com/uberVU/react-guide/blob/master/props-vs-state.md)

[what is the difference between state and props in react](http://stackoverflow.com/questions/27991366/what-is-the-difference-between-state-and-props-in-react)

##### Changing props and state

```
                                                   props   state
    Can get initial value from parent Component?    Yes     Yes
    Can be changed by parent Component?             Yes     No
    Can set default values inside Component?*       Yes     Yes
    Can change inside Component?                    No      Yes
    Can set initial value for child Components?     Yes     Yes
    Can change in child Components?                 Yes     No

```

* Note that both props and state initial values received from parents override default values defined inside a Component.



