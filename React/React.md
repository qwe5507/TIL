# 리액트란 무엇인가? 왜쓰는가?

**컴포넌트란?**

- 데이터와 화면을 하나로 묶어둔 덩어리
- 데이터는 state
- 화면은 return

**리액트가 해결하려고 하는것은?**

- 데이터와 화면의 일치
- 데이터가 바뀌면 화면이 자동으로 바뀌게끔
    - 기존의 html을 js로 변경하는것 과 달리, 리액트는 반대

**JSX란?**

- JSX(JavaScript XML)는 JavaScript를 확장하여 React.js에서 사용되는 문법입니다. JSX는 HTML과 유사한 구문을 사용하여 JavaScript 코드 안에서 UI를 작성할 수 있게 해줍니다.
- JSX 코드는 Babel과 같은 도구를 사용하여 일반 JavaScript 코드로 변환되어 실행됩니다. 이렇게 변환된 코드는 React 컴포넌트를 생성하고 렌더링하는 데 사용됩니다. JSX를 사용하면 React 애플리케이션의 가독성과 유지 보수성을 향상시킬 수 있습니다.
- 예전에는 `React.createElement` 같은 문법을 사용해야 했는데, 가시성 때문에 jsx문법이 나왔다.

```java
// return React.createElement('button', {onClick: () => this.setState({liked: true})}, 'Like');
// 위의 문법이 불편하여, React가 제공해준 JSX(javascript XML)문법
return (
    <button onClick={() => this.setState({liked: true})}>
        Like
    </button>
);
```

- 바벨은 jsx문법을 `React.createElement`  로 변경시켜 준다.
    - js는 태그(`<>`)를 모른다. 바벨이 변경해주는 것
    - 실제 html태그와 다르게 반드시 소문자로 태그를 명시해야 한다.
    - React내의 JS코드는 `{ }` 로 감싸주어야 한다.
        - JS에서 객체를 표현하는 `{ }` 를 REACT내에서 표현할떄는 `{{ }}` 이런식으로 표현해야 한다.
    - return되는 컴포넌트는 하나의 태그 여야 한다.

```java
// ReactDOM.render(<LikeButton/>, document.querySelector('#root')); // React 17 버전
ReactDOM.createRoot(document.querySelector('#root')).render(<LikeButton/>);
```

- React18버전은 17버전의 코드도 인식은 하지만, React 18버전의 기능이 동작하지 않는다.

---

- **리액트는 항상 데이터를 중심으로 생각해야 한다.**
    - 데이터가 변경 되는 부분을 state로 지정해야 한다.
- **객체를 변경하지 말고, 복사해라(불변성)**
    - React는 state객체를 복사하여 대체 할 수 있게, 변경 함수를 제공
- `{}` 안에는 JS문법을 사용 할 수 있다.**
- **컴포넌트는 하나의 태그로 감싸져야 한다.**
    - 바벨툴을 설치해야 컴포넌트의 최상단에 `<>` `</>` 를 지원한다.
- **Dom에 직접 접근하고 싶을 땐, ref를 사용한다.**

---
# Hooks란?

```java
const GuGuDan = () => {
		return <div>Hello, Hooks</div>;
}
```

위와 같은 문법을 함수 컴포넌트 라고 한다.

- 클래스 컴포넌트에서 `ref`나 `setState`같은 기능들을 사용할 필요가 없을 때 사용했다고 한다.

사람들은 함수 컴포넌트에서도 `setState`나, `ref`를 사용할 수 있게, 요청함

이런 기능들을 React팀이 추가한게 `Hooks`임

- `use` 붙은게 Hooks임
- 코드량이 훨씬 줄어든다.
- Hooks는 `useRef` 로 DOM에 접근한다.
