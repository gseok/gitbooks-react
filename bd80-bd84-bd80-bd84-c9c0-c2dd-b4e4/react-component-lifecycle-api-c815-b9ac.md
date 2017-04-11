### React component lifecycle, API 정리

#### 전체 도식

![](/assets/react-life-cycle.png)그림: [https://staminaloops.github.io/undefinedisnotafunction/understanding-react/](https://staminaloops.github.io/undefinedisnotafunction/understanding-react/)  을 수정함.

기존 mounting 단계에서, getDefaultProp\(\), getInitialState\(\) 는 deprecated 되었다.

현재는 deprecated된 함수 대신 constructor에서 state을 초기화 하고, prop는 super에 던지게 되어 있다.

좀더 알기 쉬운 그림을 보자

![](/assets/react-life-cycle-2.png)

설명



