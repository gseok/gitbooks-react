### React state나 prop 조건에 따른 rendering 하기

#### Conditional Rendering

React에서 어떤 컴포넌트의 랜더링을 state나 prop와 값에 따라 서로 다르게 하고 싶은 경우가 있을 수 있다. 즉 조건에 따른 랜더링이 필요한 경우가 있다.

React에서는 이러한 조건에 따른 랜더링\(Conditional Rendering\)을 Javascript에서 `if`문을 사용하는것 같이 `조건문`을 사용해서 구현 할 수 있다.

##### 예제로 살펴보기

아래와 같이 사용자의 로그인 상태에 따라 서로 다른 형태를 출력하는 각각의 컴포넌트가 존재한다고 하자.

```js
function UserGreeting(props) {
  return <h1>Welcome back!</h1>;
}

function GuestGreeting(props) {
  return <h1>Please sign up.</h1>;
}
```

이제 우리는  `loggedIn`이라는 `prop` 에 따라\(즉 `조건`에 따라\) 서로 다른 형태로 랜더링 되는 `Greeting` 이라는 컴포넌트를 아래와 같이 구현 할 수 있다.

```js
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

ReactDOM.render(
  // Try changing to isLoggedIn={true}:
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```

말 그대로, `props`에 따라 `조건`을 확인해서, 서로 다른형태의 `Rendering`을 할 수 있다.

`es6` 형태로 나타내면 아래와 같다.

```js
class UserGreeting extends React.Component {
  render() {
    return <h1>Welcome back!</h1>;
  }
}

class GuestGreeting extends React.Component {
  render() {
    return <h1>Please sign up.</h1>;
  }
}

class Greeting extends React.Component {
  constructor(props) {
    super(props);
  }

  render() {
    // 간단하게 render() 함수에서, 조건에 따라 render되는 부분을 만들면 된다
    const isLoggedIn = this.props.isLoggedIn;
    if (isLoggedIn) {
      return <UserGreeting />
    } else {
      return <GuestGreeting />
    }
  }
}
```

`state`값을 조건으로하여, 서로 다른 형태의 `Rendering`을 하는 것 역시 동일한 형태로 가능하다.

아래는 `state`값에 따라 서로 다른 형태의 `Rendering`을 하는 예제이다.

```js
class UserGreeting extends React.Component {
  render() {
    return <h1>Welcome back!</h1>;
  }
}

class GuestGreeting extends React.Component {
  render() {
    return <h1>Please sign up.</h1>;
  }
}

class Greeting extends React.Component {
  constructor(props) {
    super(props);
    this.toggleLoginState = this.toggleLoginState.bind(this);
    this.state = {
      isLoggedIn: false
    };
  }

  toggleLoginState() {
    this.setState((prevState) => {
        return {
          isLoggedIn: !prevState
        };
    });
  }

  render() {
    // 간단하게 render() 함수에서, 조건에 따라 render되는 부분을 만들면 된다
    const isLoggedIn = this.state.isLoggedIn;

    if (isLoggedIn) {
      return (
        <div>
          <button onClick={this.toggleLoginState} />
          <UserGreeting />
        </div>
      );
    } else {
      return (
        <div>
          <button onClick={this.toggleLoginState} />
          <GuestGreeting />
        </div>
      );
    }
  }
}
```

위 예제에서는, 간단하게, `isLoggedIn` 이라는 `state`을 `toggle` 하는 버튼과 함수를 추가했다.

사실상 핵심은 `render`에서 실제 조건에 다라 다르게 그리게 하는데 있다.

#### 참고

[react document - conditional rendering](https://facebook.github.io/react/docs/conditional-rendering.html)

