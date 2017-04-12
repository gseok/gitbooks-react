### React preventDefault  처리 정리

React에서 event handling을 처리할때, `preventDefault`의 처리 방식이 약간 다르다. 해당 내용을 정리한다.

React에서는 명시적으로 `preventDefault` 을 호출해야 한다. `false`를 리턴하는 형태는 동작하지 않는다.



##### 일반적으로 사용하는 패턴

```html
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```

위 예제는 `anker` 태그의 실제 링크 이동을 막는 동작을 한다. 일반적으로 위와 같이 사용해도 큰 문제가 없으나. React에서는 명시적으로 `preventDefault()`을 호출해야 한다.



##### React에서는 `preventDefault()`을 명시적으로 호출

```js
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

몇가지 알아둘 사항이 있다.

**event 객체**

event handler 함수의 param으로 전달 되는 event 객체 \(위 코드에서는 e\)는, pure javascript 코딩시 전달되는 event 객체와 다른 객체이다. 즉 React가 정의한 [`SyntheticEvent`](https://www.gitbook.com/book/gseok/react/edit#) 객체이다.

하지만 걱정하 필요 없다. React에서는 오히려 사용자가 사용하기 편하게 [`SyntheticEvent`](https://facebook.github.io/react/docs/events.html) 객체를 만들어 전달한다.

React에서는 [W3C spec](https://www.w3.org/TR/DOM-Level-3-Events/) 표준와 동일한 형태로 event 객체를 만들어서 전달한다. 따라서 cross-browser compatibility을 개발자가 걱정할 필요 없이 React를 믿고 사용하면\(cross-browser에서 모두 동작\)된다.



**addEventListener**

React에서는 일반적으로 DOM 생성 후 addEventListener을 개발자가 직접 사용할 필요가 없다. 그 대신 element가 rendered 되는 코드에서 listener을 제공하면 된다. 

이 말은, 실제 DOM의 addEventListener, removeEventListner는 React가 알아서 담당하여, 처리해주고, 개발자는 render의 구현과, event handler구현에 집중하면 된다는 이야기 이다.

