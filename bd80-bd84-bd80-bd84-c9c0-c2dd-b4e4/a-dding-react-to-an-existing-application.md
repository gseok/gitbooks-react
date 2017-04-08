# Adding React to an Existing Application

이미 존재하는 web app에 React을 적용하는 방법이다.

web application을 만들때, 처음 부터 끝까지 모든 부분을 react을 사용해서 만들 수도 있지만. 이미 만들어져 있는 web application나 새로 만들더라도, 일부분만  React을 적용 하고 싶을 수 있다.

이러한 경우 React을 적용하는 간단한 방법을 설명한다.

#### 공식 홈페이지의 설명

참고: [Adding React to an Existing Application](https://facebook.github.io/react/docs/installation.html#adding-react-to-an-existing-application)

You don't need to rewrite your app to start using React.

We recommend adding React to a small part of your application, such as an individual widget, so you can see if it works well for your use case.

While React [can be used](https://facebook.github.io/react/docs/react-without-es6.html) without a build pipeline, we recommend setting it up so you can be more productive. A modern build pipeline typically consists of:

* A **package manager**, such as [Yarn](https://yarnpkg.com/) or [npm](https://www.npmjs.com/). It lets you take advantage of a vast ecosystem of third-party packages, and easily install or update them.
* A **bundler**, such as [webpack](https://webpack.js.org/) or [Browserify](http://browserify.org/). It lets you write modular code and bundle it together into small packages to optimize load time.
* A **compiler **such as [Babel](http://babeljs.io/). It lets you write modern JavaScript code that still works in older browsers.

공식 홈페이지에도 React을 전체 application의 일부에만 적용하여서 사용 할 수 있다고 되어 있다.

기존에 사용하는 web application이 위에서 설명하는 내용을 다시 정리하면

* yarn \|  npm: **react 설치 및 update**을 위함
* webpack \| browerify: React로 작성한 코드를, build하여 **single형태의 js \(bundle.js\)로 생성**해주고, react 코드 내부에서 사용한 **각종 import 된 lib을 하나로 묶어**주기 위함
* Babel: 위 webpack \| browserify을 사용해서 React로 작성한 코드를 single형태의 js로 변환할때, **JSX을 해석 변환하고, ES6문법을 es2015와 같이 이전 버전의 js 문법 형태로 변환**해 주기 위함

정도로 정리 할 수 있다.

문제는 기존에 사용하던 web application나 새로만드는 web application이 yarn이나 npm을 사용하지 않고, webpack이나 browerify와 같은 bundler도 사용하지 않으면서, React 코드를 es6 + JSX로 작성했을 때, React로 작성한 부분을 구동 시키는가? 이다.

#### Browser에서 구동되는 web appication\(clinet\)의 코드에서 바로 React 적용하기

##### 기본적으로 React을 사용하려면 필요한 모듈들이 필요하다.

`react` 와 `react-dom` 에 대한 모듈와 `JSX, es6` 을 지원하는 모듈이 필요.

* React
* React-DOM

각각 browser에서 바로 사용 할 수 있는 버전을 제공한다.

```js
<script src="https://unpkg.com/react@15/dist/react.js"></script>
<script src="https://unpkg.com/react-dom@15/dist/react-dom.js"></script>
```

* JSX, ES6지원 \(Babel\)

동일하게 Babel에서 browser에서 바로 사용 할 수 있는 버전을 제공한다.

```js
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```

##### 코드에서 사용

```html
<div id="react-root"></div>


<!-- react -->
<script src="https://unpkg.com/react@15/dist/react.min.js"></script>
<script src="https://unpkg.com/react-dom@15/dist/react-dom.min.js"></script>

<!-- babel -->
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>

<!-- react code -->
<script type="text/babel">
    // import React from 'react'; 없이 곧바로 React객체 접근 가능
    // 브라우저에서 바로 사용 가능한 모듈 형태로 Load했기 때문

    class App extends React.Component {
        render(){
            return (
                <h1>Hello React</h1>
            );
        }
    }

    const rootElement = document.getElementById('react-root');
    ReactDOM.render(<App />, rootElement);
</script>

<!-- or load react code -->
<script type="text/babel" src="{{ your_site_path }}/js/react/components/reactCode.js"></script>
```

위 코드와 같은 형태로 구현하면, 별도의 package-manager나, bundler없이 필요한 모듈을 browser에서 곧바로 사용 가능하다.

단 단점은. React의 원래 구조가, es6의 문법에 여러 module을 import하는 식으로 구현되어 있고, 이를 bundler로 빌드할때 `여러 module + JSX해석 + es6을 es2015` 와 같이 변환 하는 형태로 되어. 최종 `bundle.js` 을 로드하는 형태인데.

위와 같이 사용하는 경우에는,  브라우저에서 동적으로\(실시간으로\), babel이 구동되어 동작되는 형태가 된다.

\(성능저하가 좀 있을 수 있다\)

정리하면, 위와 같은 방법 이외에도 여러가지 방법을 사용해서 React을 browser에 바로 적용할 수 있다. 여기서는, 최대한 별도의 모듈 설치는 제외하고, React코드를 적용하는 방법을 설명하였다.

