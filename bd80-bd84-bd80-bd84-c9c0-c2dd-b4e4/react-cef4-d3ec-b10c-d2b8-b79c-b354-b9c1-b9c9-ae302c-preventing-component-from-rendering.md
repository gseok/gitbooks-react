### React 컴포넌트 랜더링 막기, preventing component from rendering

아주 특이한 경우에, 컴포넌트가 자기 자신을 숨겨야 하는 \(그리지 말아야 하는\) 경우가 있을 수 있다.

이런 경우 간단하게 `render()` 에서 `null` 을 리턴하면 된다.

아래 예제를 살펴보자.

```js
class WarningBanner extends React.Component {
  render() {
    // 자기 자신의 props중 warn이 false이면 null을 리턴해서 그림을 안그림
    if (!this.props.warn) {
      return null;
    }

    return (
      <div className="warning">
        Warning!
      </div>    
    );
  }
}

class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state = {showWarning: true}
    this.handleToggleClick = this.handleToggleClick.bind(this);
  }

  handleToggleClick() {
    this.setState(prevState => ({
      showWarning: !prevState.showWarning
    }));
  }

  render() {
    return (
      <div>
        // Page의 showWarning을 WarningBanner의 prop로 전달 (토글)
        // 따라서 WarningBanner는 prop에 따라서 hide 되거나 warning을 출력
        <WarningBanner warn={this.state.showWarning} />
        
        // button을 누를때 마다 Page의 showWarning이 토글됨
        <button onClick={this.handleToggleClick}>
          {this.state.showWarning ? 'Hide' : 'Show'}
        </button>
      </div>
    );
  }
}

ReactDOM.render(
  <Page />,
  document.getElementById('root')
);
```

예제 에서는, `<WarningBanner /> `에 `warn` 이라는 `prop`이 `false`이면 `render()`에서 `null`을 리턴하여 자기 자신을 그리지 않도록 하고 있다.

핵심은 자기 자신을 `그리지 않고 싶을때`는, `render()` 에서 `null `을 리턴하면 된다.

