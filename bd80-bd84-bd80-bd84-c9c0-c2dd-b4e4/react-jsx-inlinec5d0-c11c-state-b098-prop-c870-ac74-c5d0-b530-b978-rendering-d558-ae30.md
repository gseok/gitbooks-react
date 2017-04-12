### React JSX Inline에서 state나 prop 조건에 따른 rendering 하기

#### Conditional Rendering

`React`에서 `state` 나 `prop` 조건에 따른 `rendering`은 `render()`함수에서 조건에 따라 서로 다른 `render()`형태로 간단히 구현 할 수 있다.

여기서 더 나아가서, `React`에서 사용하고 있는 `JSX`에 조건 검사를 추가하여, `Inline`에서 동일한 형태로, 조건에 따른 서로 다른 `rendering` 구현이 가능하다.

##### && Operator 을 사용한 방법

```js
class Mailbox extends React.Component {
    render() {
        const unreadMessages = this.props.unreadMessages;
        return (
            <div>
                <h1>Hello!</h1>
                // 아래와 같이 { } 로 묶은 구문 안에서, && 연산을 이용해서 if 처럼 사용가능하다
                {unreadMessages.length > 0 &&
                <h2>
                    You have {unreadMessages.length} unread messages.
                </h2>
                }
            </div>
        );
    }
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```

동작하는 이유

* Javascript 에서 `true && expression` 은 `express`을 수행한다.
* Javsscript 에서 `false && expression` 은 `false`을 수행한다.

따라서 어떤 조건이  true 인 경우만 rendering이 필요한 경우 위 예제와 같이 작성하면된다. 하지만. && 방식으로는 if-else 형태에 대한 부분을 처리 하기 힘들다. 아래에 if-else와 같은 형태를 처리 하는 방법을 소개한다.

##### condition을 사용한 방법, Inline If-Else with condition ? true : false

아래와 같이 간단하게 text을 조건에 따라 나타나게 할 수 있다.

```js
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```

또한 `{ }` 구문 내부에서  `condition ? true : false` 을 사용해서 나태낼 수도 있다.

```js
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      // 핵심, { } 구문 내부에서 condition ? true에 해당하는 render : false에 해당하는 render
      {isLoggedIn ? (
        <LogoutButton onClick={this.handleLogoutClick} />
      ) : (
        <LoginButton onClick={this.handleLoginClick} />
      )}
    </div>
  );
}
```

`React`에서 어떤 조건에 따른 `rendering`을 하는 방법은 간단하다.

하지만 만약 조건이 많아져서, 복잡한 형태가 된 경우, `component`을 분리 하는것을 공식문서에서는 추천하고 있다.

> remember that whenever conditions become too complex, it might be a good time to
>
> [extract a component](https://facebook.github.io/react/docs/components-and-props.html#extracting-components).

#### 참고

[react document - conditional rendering](https://facebook.github.io/react/docs/conditional-rendering.html)

