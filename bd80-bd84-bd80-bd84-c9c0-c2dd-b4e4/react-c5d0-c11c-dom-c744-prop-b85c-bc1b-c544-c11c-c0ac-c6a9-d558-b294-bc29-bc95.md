#### React, rendering 요소\(dom, component\)를 prop로 받는 컴포넌트와 사용법

##### 개념 설명

react는 UI를 component로 나누어서 만들고, 각각의 component는 독립적으로 존재하며, 재사용 가능한 형태로 하는것이, react의 기본적인 concept이다.

따라서 개발자가 react로 개발을 하다보면\(또는 개발을 위한 설계를 하다보면\), 중복적인 UI 요소들을 component로  빼서 작성하는 형태가 된다.

이때 **여러 component에서 공통적으로 사용하는 어떤 layout을 나타내는 component을 만들고**, 해당 layout component는 자식 요소로 부모에서 전달한 dom 이나 component을 받는 형태의 구현이 필요 한 경우가 있다.

이와같이 `dom` 이나 `component`을 합쳐서 `rendering 요소`라고 한다.

위에서 글로 설명한 부분을 그림으로 다시 한번 설명해 보겠다.



![](/assets/react-rendering-prop-component-draft.png)

보통 application을 만들때 전체적인 layout을 component화 할때, 많이 사용 될 듯 하다.

\(e.g. BorderContainer, TabContainer 와 같이 layout을 공통화 하는 component을 만드는 경우\)



##### 실제구현

```js
class MyTitleContainerLayout extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        return (
            <div className='layout-pane'>
                <div className='title'>
                    {this.props.title}
                </div>
                <div className='contents'>
                    {this.props.contents}
                </div>
            </div>
        );
    }
}
MyTitleContainerLayout.propTypes = {
    title: React.PropTypes.string,
    contents: React.PropTypes.element
};
```

사실 어려울것 없이 간단하게 `title`와 `contents`을 부모로 부터 입력 받아서 화면을 그리는 component을 생성하였다.

여기서 propTypes 체크를 위한 type설정을 한 부분을 주목해보자

```js
MyTitleContainerLayout.propTypes = {
    title: React.PropTypes.string,
    contents: React.PropTypes.element
};
```

* title은 string만 받는다.
* contents는 React element만 받는다.
  * 즉 또다른 React component만 props로 받는다.

만약 conents을 React element가 아닌 DOM node와 같이 rendering 되는 type으로 받고 싶으면 `React.propTypes.node` 로 작성해야 한다.

```js
MyTitleContainerLayout.propTypes = {
    title: React.PropTypes.string,
    contents: React.PropTypes.node
};
```

React의 propTypes는 다음 링크를 참고하면 된다.

참고: [Reactd.PropTypes](https://facebook.github.io/react/docs/typechecking-with-proptypes.html)



##### 사용

위에서 만든 component을 사용하는 부모 component는 아래와 같이 작성하면 된다.

```js
class AboutMe extends React.Component {
    constructor(props) {
        super(props);

        this.state = {
            label: 'About Me'
        }
    }

    createAboutContents() {
        return (
            <div>
                <MyImage />
                <TextDesc />
            </div>
        ;
    }

    render() {
        let contents = this.createAboutContents();

        return (
            <MyTitleContainerLayout label={this.state.label}
                                    contents={contents}/>
        );
    }
}
```

위에 예제에서는 `lable`을 부모인 `AboutMe` 컴포넌트가 `state`로 관리하면서 넘겨주고 있다. 

부모의 `state`을 자식의 `props`로 넘겨주는 방식은 React에서 많이 사용하는 패턴이다.

필요한 부분인 `contents` 는 `render` 함수에서, 생성해서 자식에 `prop`로 넘겨 주고 있다.

위의 예제와 같은 패턴으로 작성하면.

모든 부모들은 동일한 형태의 `container (MyTitleContainerLayout)`을 사용하면서, 각각이 자신만의 `contents`을 사용하는 형태의 구현이 가능하다.

끝으로 몇가지 기억할 점을 정리하겠다.



##### 기억할점

* `props`는 `read-only`다.
  * 보통 부모가 전달해 주거나, default값을 사용한다.
  * 자신이 자신의 props을 변경하지 말자.
* `state`는 `change`가능한 요소이다.
  * 상태에 따라 변경되는 요소를 작성한다. 보통 component자기 자신이 관리하고 사용한다.
* dom 이나 react element을 자식의 props로 전달할때는, `render` 함수 내부에서 생성해서 전달하면 된다.



##### 끝으로...

물론 어차피 각각을 별도의 component로 만들어야 하니, 각각이 알아서 하게 하고, 공통적으로 나타내고 싶은 제목과 contents 영역만 class로 관리 할 수도 있다. 이는 개발자, 각각의 설계에 다라 다른 부분이고, 여기서는 `rendering요소`를 `props`로 관리하고, 부모가 `rendering요소`를 전달하는 형태에 대한 내용을 설명하였다.





