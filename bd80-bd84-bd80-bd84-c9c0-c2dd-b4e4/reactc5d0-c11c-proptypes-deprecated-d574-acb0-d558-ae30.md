#### React에서 PropTypes deprecated 해결하기

npm으로 React을 새로 버전업 한 이후 아래와 같은 warning 메시지가 발생할때 해결 방법을 소개한다.

> warning.js:36 Warning: Accessing PropTypes via the main React package is deprecated. Use the prop-types package from npm instead.

살펴보니, React \(15.5.0\)에서부터는 React의 main package에 PropTypes가 제거되고, 별도의 라이브러리를  사용하는 식으로 변경되었다.

* 참고: [https://github.com/callemall/material-ui/issues/6542](https://github.com/callemall/material-ui/issues/6542)

홈페이지를 찾아보니 아래와 같이 `prop-types` 라이브러리를 별도로 설치해서 사용하라고 되어 있다.

![](/assets/react-prop-types-guide.png)

React가 가이드하는 대로, `prop-types` 라이브러리를 설치해서 사용하면 간단히 해결된다.

#### 해결방법

**React의  **[**prop-types library**](https://www.npmjs.com/package/prop-types)** 을 설치하기**

* npm 주소: [https://www.npmjs.com/package/prop-types](https://www.npmjs.com/package/prop-types)

```bash
$ npm install --save prop-types
```

[**prop-types library**](https://www.npmjs.com/package/prop-types)** 사용하기**

```js
import React from 'react';
import ReactDOM from 'react-dom';

// npm으로 설치 후, 아래와 같이 'prop-types'을 새로 import 
import PropTypes from 'prop-types';

...

class TestComponent extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        return (
            <div>
                <div className='test-title'>
                    {this.props.label}
                </div>
                <div className='test-contents'>
                    {this.props.contents}
                </div>
            </div>
        );
    }
}
TestComponent.propTypes = {
    label: PropTypes.string,
    contents: PropTypes.element
};
```

기존에 `React.PropTypes.<typeName>` 으로 작성하던 부분을 `PropTypes.<typeName>` 형태로 작성하면 된다.





