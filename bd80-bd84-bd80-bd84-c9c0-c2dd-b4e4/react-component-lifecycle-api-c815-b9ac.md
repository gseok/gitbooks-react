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

적용 될 때 까지를 담당합니다.

* [`constructor()`](https://facebook.github.io/react/docs/react-component.html#constructor)
* [`componentWillMount()`](https://facebook.github.io/react/docs/react-component.html#componentwillmount)
* [`render()`](https://facebook.github.io/react/docs/react-component.html#render) - update의 render와 동일한 함수 입니다.
* [`componentDidMount()`](https://facebook.github.io/react/docs/react-component.html#componentdidmount)

##### Updating

업데이트 관련 함수들은, props 나 state가 변화된경우, 이런경우 component가 다시 그려져야\(re-rendered\) 하는 경우를 담당합니다.

* [`componentWillReceiveProps()`](https://facebook.github.io/react/docs/react-component.html#componentwillreceiveprops)
* [`shouldComponentUpdate()`](https://facebook.github.io/react/docs/react-component.html#shouldcomponentupdate)
* [`componentWillUpdate()`](https://facebook.github.io/react/docs/react-component.html#componentwillupdate)
* [`render()`](https://facebook.github.io/react/docs/react-component.html#render) - mounting에 render와 동일한 함수 입니다.
* [`componentDidUpdate()`](https://facebook.github.io/react/docs/react-component.html#componentdidupdate)

##### Unmounting

이 경우는 유일하게, component가 DOM에서 제거될때 불리게 됩니다.

* [`componentWillUnmount()`](https://facebook.github.io/react/docs/react-component.html#componentwillunmount)

 

 

 

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



