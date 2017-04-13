### React 에서 event handling 하는법 정리

React elements에 대한 event handliing은 DOM element에 대한 event handling와 매우 유사 하다.

하지만 몇가지 차이점이 있다. 해당 차이점을 소개하고, React에서 React element에 대한 event handling 하는 방법을 간단히 소개한다.

#### React element 이벤트 핸들링 기본

* React 에서 events 이름은 반드시 camelCase\(카멜케이스, 낙타표기\)를 사용해야 한다.
* React 에서 event handler\(이벤트 핸들러\)는 JSX 형태로 함수를 표기한다.

```js
// 기존 DOM element 에서 사용하는 방법
<button onclick="activateLasers()">
  Activate Lasers
</button>

// React에서 사용하는 방법
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

위 예제를 살펴보면, `onClick` 와 같이 `camelCase`로 이벤트 이름을 설정하고, event handler의 연결은 `JSX` 표기인 `{ }`을 사용하고 있다.

##### React element 이벤트 핸들러 연결시 this 객체 사용 주의

다음 코드를 살펴보자

```js
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

위 코드의 로직은, `button` 에 `onClick`이 발생하면, `Toggle` 컴포넌트에 있는 `handleClick` 함수를 호출하도록 연결하고 있다.

여기에서 `constructor`에 `binding` 부분을 주목해보자

```js
constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
}
```

살펴보면, `this.handleClick` 함수에 `this`을 `bind` 하고 있다.

여기서 this 는 Toggle 컴포넌트를 뜻한다. 왜 this을 bind 해야 할까?

사실 내용은 간단하다. **JSX을 사용해서, this.handleClick을 연결했을때, JSX는 this을 bind  하지 않는다**.

따라서, 해들러 함수 내부에서, `this` 객체\(컴포넌트 자기자신\)를 사용하려면, 그전에 미리 `this`를 `bind` 해주어야 한다.

```js
handleClick() {
    // 여기서 this(Toggle)을 사용하려면, 반드시 this가 bind 되어 있어야 한다.!!!!!
    this.setState(prevState => ({
        isToggleOn: !prevState.isToggleOn
    }));
}
```

##### this를 bind 하는 기본적인 방법

```js
// 1. constructor에서 method에 this을 bind 하기
constructor(props) {
    super(props);

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
}

또는 아래와 같이 this를 bind할 수도 있다.

// 2. event hanler함수를 연결할때 bind를 하기
render() {
    return (
        <button onClick={this.handleClick.bind(this)}>
            {this.state.isToggleOn ? 'ON' : 'OFF'}
        </button>
    );
}
```

위 2가지 방법이 가장 일반적으로 `event handler` 함수에 `this`을 `bind` 하는 방법이다.

[react document](https://facebook.github.io/react/docs/handling-events.html)에서는 추가적으로 2가지 방법을 더 소개 하고 있다.

##### this를 bind 하는 추가적인 방법

* `event handler`을 정의하는 시점에 `es6`문법의 `array function` style로 정의

```js
class LoggingButton extends React.Component {
  // This syntax ensures `this` is bound within handleClick.
  // Warning: this is *experimental* syntax.
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

이 방법은 `babel` 컴파일중 에러가 발생하는 경우도 존재한다. 이 방법이 되는 이유는 `es6`의 `() =>` 문법이 `this`을 `bind` 해주는 형태이기 때문이다.

* `JSX`로 `event handler`을 연결할때 `anonymous` 형태의 `arrow function` 을 사용하기

```js
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}

// 이 방식은 사실 아래 코드처럼 되는 것과 같다.
let self = this;
onClick = function (e) {
    self.handleClick(e); // caller인 self (LogginButton) 컴포넌트가 this로 bind
}
```

이 방법은 `babel` 컴파일중 에러가 발생하지는 않는다. 이 방법이 되는 이유는 `anomymouse`로 생성된 `function`에서 명시적으로 `this.handleClick(e)`을 호출하기 때문에 `this`가 `handler`에 `bind` 된다.

이 방법의 단점은 `component(LogginButton)` 이 `render`될때마다 **그때마다 **`anonymouse`** 함수가 생성**되는데 있다. 일반적으로 대부분의 경우에는 큰 문제 없지만, 해당 `event handler` 함수 내부에서, 자식 `component`로 `prop`를 전달하거나, 해당 `component`가 대용량의 `re-rendering`이 필요한 경우에는 성능상의 문제가 발생 할 수 있다.

#### 정리

* React 에서 event handler함수는 camelCase\(낙타표기\)를 사용해야 한다.
* React 에서 event handler\(이벤트 핸들러\)는 JSX 형태로 함수를 표기한다.
* React 에서 event handler 표기시 this 객체 bind 에 주의하여야 한다.
* React 에서 event handler에 this 객체 bind는 일반적으로 생성자\(constructor\)에서 하는 것을 권장한다.

#### 



