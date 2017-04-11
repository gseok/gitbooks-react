React, Props vs State 개념 정리

React에서 props와 state는 개념과 사용이 서로 다르다. 해당 내용을 정리한다.



React Props

Props are Read-Only

참고: [https://facebook.github.io/react/docs/components-and-props.html](https://facebook.github.io/react/docs/components-and-props.html)

공식 document에 아래와 같이 정의 되어 있다.

React is pretty flexible but it has a single strict rule:

> **All React components must act like pure functions with respect to their props.**



React에서 Props는 component의 수명주기동안 불변이라고 생각 하면된다.

따라서 component 내부에서 변경해서는 안된다. 절대 안된다. 그냥 Read-Only로 봐야 한다.

이전 버전에서는 `setProps`나 `replaceProps` 함수를 제공하였지만, 모두 `deprecated`되었다.



Props 설정

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



React State

React State is changeable

참고: [https://facebook.github.io/react/docs/state-and-lifecycle.html](https://facebook.github.io/react/docs/state-and-lifecycle.html)

공식 document에 아래와 같이 정의되어 있다.

> State allows React components to change their output over time in response to user actions, network responses, and anything else, without violating this rule.



