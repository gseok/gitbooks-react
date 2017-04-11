### React component lifecycle, API 정리

#### 전체 도식

![](/assets/react-life-cycle.png)그림: [https://staminaloops.github.io/undefinedisnotafunction/understanding-react/](https://staminaloops.github.io/undefinedisnotafunction/understanding-react/)  을 수정함.

기존 mounting 단계에서, getDefaultProp\(\), getInitialState\(\) 는 deprecated 되었다.

현재는 deprecated된 함수 대신 constructor에서 state을 초기화 하고, prop는 super에 던지게 되어 있다.

좀더 알기 쉬운 그림을 보자

![](/assets/react-life-cycle-2.png)그림: [https://tylermcginnis.com/an-introduction-to-life-cycle-events-in-react-js/](https://tylermcginnis.com/an-introduction-to-life-cycle-events-in-react-js/) 을 수정함

#### 도식 설명

위 그림은 아래와 같이 ReactDOM.render\(\) 을 호출한 이후 벌어지는 component의 life cycle을 정리한 도식이다.

```js
const root = document.getElementById('root');
ReactDOM.render(<App />, root);

이 코드가 수행되면, 위 도식처럼 component가 진행된다.
```

복잡해 보이지만 첫번째 그림처럼 몇가지 세션으로 나누어서 보면 이해하기 쉽다.

##### Mounting

마운팅 관련 함수들은, 처음 render가 호출된 이 후, component가 생성\(instance화\) 되고, 실제 DOM으로

적용 될 때 까지를 담당합니다. render을 제외하고 한번씩만 호출된다고 보면 됩니다.

* [`constructor()`](https://facebook.github.io/react/docs/react-component.html#constructor)
* [`componentWillMount()`](https://facebook.github.io/react/docs/react-component.html#componentwillmount)
* [`render()`](https://facebook.github.io/react/docs/react-component.html#render) - update의 render와 동일한 함수 입니다.
* [`componentDidMount()`](https://facebook.github.io/react/docs/react-component.html#componentdidmount)

##### Updating

업데이트 관련 함수들은, props 나 state가 변화된경우, 이런경우 component가 다시 그려져야\(re-rendered\) 하는 경우를 담당합니다. mount와 다르게 여러번 호출 될 수 있습니다.

* [`componentWillReceiveProps()`](https://facebook.github.io/react/docs/react-component.html#componentwillreceiveprops)
* [`shouldComponentUpdate()`](https://facebook.github.io/react/docs/react-component.html#shouldcomponentupdate)
* [`componentWillUpdate()`](https://facebook.github.io/react/docs/react-component.html#componentwillupdate)
* [`render()`](https://facebook.github.io/react/docs/react-component.html#render) - mounting에 render와 동일한 함수 입니다.
* [`componentDidUpdate()`](https://facebook.github.io/react/docs/react-component.html#componentdidupdate)

##### Unmounting

이 경우는 유일하게, component가 DOM에서 제거될때 불리게 됩니다. component제거시 한번 호출된다고 보면 됩니다.

* [`componentWillUnmount()`](https://facebook.github.io/react/docs/react-component.html#componentwillunmount)

#### API Detail

**constructor\(\)**

```js
constructor(props)
```

* React component가 mount과정에서 가장 먼저 호출 됩니다. \(즉 아직 DOM으로 mount되기 전에 호출\)
* 해당 함수 내부에서, component의 state을 초기화 할 수 있습니다.
* state을 별도로 사용할 필요가 없다면, constructor을 작성하지 않아도 무방합니다.
* 주의
  * 해당 함수의 첫번째 라인은 반드시 `super(props)` 을 호출해야 합니다. \(미호출시 prop가 생성되지 않음\)
  * component가 prop을 새로 받았을때 \(부모가 prop을 다시 전달했을때\), 해당 component가 prop값으로 자신의 state을 업데이트 해야하는 경우, 여기서 작성하지 마세요!! 해당 동작은 `componentWillReceiveProps(nextProps)` 에서 작성하세요.
  * 즉 여기서는 state init 정도만 하세요~

**componentWillMount\(\)**

```js
componentWillMount()
```

* DOM 마운팅 이전에 \(한번\)호출됩니다. 사실 `constructor()` 호출 이후 `render()`이전에 호출됩니다.
* 따라서, 여기서 `state`을 `setting` 해도 `re-rendering(update)`에 영향을 주지 않습니다.
* 주의
  * 서버 사이드 랜더링에서 DOM마운트 전에 호출 가능한 유일한 함수 입니다.
  * 일반적으로는 `constructor()`을 사용하는 것을 추천합니다.

**render\(\)**

```js
render()
```

* 랜더링을 담당합니다
* 필수 함수, components에 해당 메소드를 반드시 만들어야 합니다
* 랜더링을 할 것이 없는경우, `null` or `false`  을 리턴합니다.
* `pure`해야 합니다. 의미는 `render()`에서 `state`을 수정하면 안됩니다.
  * \(state을 수정하면 돌고 돌아 다시 render\(\) 가 호출될 수 있습니다.\)
* 주의
  * browser와 직접 interaction을 여기서 하지 마세요. \(즉 DOM을 여기서 조작하지 마세요\)
  * browser와의 interaction은 `componentDidMount()` 나 다른 lifecycle api에서 하세요
  * `shouldComponentUpdate()` 가 `false`을 리턴하면, `render()`는 호출되지 않습니다.

**componentDidMount\(\)**

```js
componentDidMount()
```

* DOM 마운팅 이후 \(한번\) 호출됩니다.
* DOM을 얻어올 수 있습니다. \(DOM이 마운트 된 다음 호출되니까..\)
* 원격\(예를들어 서버...\)에서 `data`을 가져올 필요가 있는경우 \(즉 가져와서 state을 변경해야 하는경우\), 이 함수에서 사용하면 좋습니다. \(ajax call === `network request` 하기 좋은위치...\)
  * 왜냐면, 이미 DOM은 mount했고, `state`을 변경해서 `re-rendering`하기 좋은 위치
* 주의
  * 여기서 `state`을 `setting` 하면 `re-rendering(update)` 과정이 수행됩니다.

**componentWillReceiveProps\(\)**

```js
componentWillReceiveProps(nextProps)
```

* 마운트 이전에는 절대 불리지 않습니다.
* 이미 마운트가 된 이후, 마운트된 component가 새로운 props을 받게 되면 호출 됩니다.
* prop 변화에 따라 자신의 state을 갱신 해야 하는 경우 사용하면 좋습니다.
  * this.props와 nextProps의 비교가 가능합니다.
* 주의
  * 여기서 `state`을 `setting`해도 `re-rendering(update)` 과정에 영향을 주지 않습니다.

**shouldComponentUpdate\(\)**

```js
shouldComponentUpdate(nextProps, nextState)
```

* `re-rendering(update)`을 진짜 할지 말지 결정하는 함수 입니다.
* 기본적으로 `props`나 `state`가 변경되면, `shouldComponentUpdate()`가 호출된 이후, `render()`을 호출할지 말지 결정하게 됩니다.
* react의 `기본은 true`을 리턴해서 `항상 re-rendering(update)` 가 되도록 합니다 .\(즉 render\(\)를 호출하도록\)
* 이 함수의 목적은, 현재 props 와 state &lt;-&gt; next props 와 state을 비교하여, 진짜 `re-rendering`할지를 결정하게 합니다.
* 주의
  * 만약 `false`를 리턴하면 다음 과정인, `componentWillUpdate()`, `render()`, `componentDidUpdate()` 는 호출되지 않습니다.
  * 만약 부모\(상위\) 컴포넌트가 re-rendering되면, 자식\(하위\) 컴포넌트도 모두 render 됩니다.
  * 부모\(상위\) 컴포넌트에서 최적화를 할때 사용합니다.

**componentWillUpdate\(\)**

```js
componentWillUpdate(nextProps, nextState)
```

* `re-rendering(update)`가 이루어지기 직전에 \(즉 render\(\)이전에\) 호출됩니다.
* 이 함수는, 실제 `update`이전에 어떤 일을 준비해야 하는 경우 사용 할 수 있습니다. \(prop이나 state와 무관하게 class에서 무언가 준비해야 한느경우?\)
* 주의
  * 이 함수는 init과정 \(최초 처음 render하는 과정\)에서는 호출되지 않습니다.
  * 여기서 `state`을 변경하면 `무한루프`에 빠지게 됩니다. 절대 `this.setState()` 을 여기서 호출하지 마세요. 만약 `prop`변경에 따른 `state`의 변경은, `componentWillReceiveProps()`을 사용하세요.
  * `shouldComponentUpdate()`가 `false`을 리턴한 경우 호출되지 않습니다.

**componentDidUpdate\(\)**

```js
componentDidUpdate(prevProps, prevState)
```

* `component`가 `rendering`을 마친 이후 \(`render()` 호출이후\)즉시 호출됩니다.
* `DOM`을 조작 할 수 있습니다. \(`component`가 `Update`된게 반영된 `DOM`\)
* network requesets을 사용하기 좋은 위치입니다. 특히 현재props와 이전props을 비교 할 수 있습니다.
* 주의
  * 이 함수는 init과정 \(최초 처음 render하는 과정\)에서는 호출되지 않습니다.
  * `shouldComponentUpdate()`가 `false`을 리턴한 경우 호출되지 않습니다.



**componentWillUnmount\(\)**

```js
componentWillUnmount()
```

* 이 함수는, `component`가 `unmounted`되고 `destroyed`되기 직전에 불립니다. \(아직 unmount되기 전에\)
* 사용한 `resource`을 `cleaning` 가능한 공간입니다. 즉 network request을 취소하거나, timer을 초기화 하거나, `componentDidMount`에서 생성한 DOM elements을 제거하는 등의 cleaning작업을 할 수 있습니다.
* 주의
  * 이 함수는 상위 `component`가 `unmount`했을때, 하위 `component`의 `unmount`가 이루어 집니다.





**re-rendering\(update\)에 영향을 주지 않는다는 의미는?**

* `this.setState()` 함수를 호출하면, `life-cycle` 함수중 `shouldComponentUpdate()` 가 호출되고, 최종적으로  `render()` 함수를 호출하게 됩니다.
* 만약 모든 함수가 `re-rendering(update)`에 영향을 준다면, `무한루프`에 빠지거나, `render()`을 `여러번` 호출하게 되는 현상이 발생 할 수 있습니다.
* 따라서 react 에서는 어떤 `life-cycle` 함수에서는 `this.setState()`을 호출해도, 자연스럽게 `render()`로 가게 되는 경우, `update 루틴`을 발생하지 않고, 자연스럽게 `render()`로 연결되도록 합니다. \(즉 state 변화에 따라 event가 발생해서, update을 타게 하지 않아도, render\(\)가 되면, 굳이 event을 발생하지 않습니다.\)

#### 참고

[The component lifecycle in react](https://facebook.github.io/react/docs/react-component.html#constructor)

[simple 정리](https://gist.github.com/fisherwebdev/8f6cb895348c587c8f1e)

```
Lifecycle:                      |    Update: 
Mounting and Unmounting         |    New Props or State
--------------------------------+-----------------------------------
                                |
getDefaultProps()               |    componentWillReceiveProps()*
                                |
getInitialState()               |    shouldComponentUpdate()
                                |
componentWillMount()            |    componentWillUpdate()
                                |
render()                        |    render()
                                |
----------------------- DOM Mutations Complete ----------------------
                                |
componentDidMount()             |    componentDidUpdate()
                                |
--------------------------------|
                                |
componentWillUnmount()          |
                                |
--------------------------------+------------------------------------                                
                                     *Called only with new props
```



