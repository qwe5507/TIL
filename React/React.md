# 리액트란 무엇인가? 왜쓰는가?

**컴포넌트란?**

- 데이터와 화면을 하나로 묶어둔 덩어리
- 데이터는 state
- 화면은 return

**리액트가 해결하려고 하는것은?**

- 데이터와 화면의 일치
- 데이터가 바뀌면 화면이 자동으로 바뀌게끔
    - 기존의 html을 js로 변경 하는것 과 달리, 리액트는 반대

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

- **바벨이란?**
    
    `소스 대 소스 컴파일러` 라고 한다. 
    
    js는 인터프리터 언어인데 왜 컴파일러가 필요한가? 
    
    특정 상황(타입스크립트, JSX, 크로스브라우징, 폴리필)에 맞게 코드를 변환해 주는 역할을 한다.
    
    웹팩과 주로 같이 사용 된다고 함
    
    - RF : https://lihano.tistory.com/20

---

- **리액트는 항상 데이터를 중심으로 생각해야 한다.**
    - 데이터가 변경 되는 부분을 state로 지정해야 한다.
- **객체를 변경하지 말고, 복사해라(불변성)**
    - React는 state객체를 복사하여 대체 할 수 있게, 변경 함수를 제공
- `**{}` 안에는 JS문법을 사용 할 수 있다.**
- **컴포넌트는 하나의 태그로 감싸져야 한다.**
    - 바벨툴을 설치해야 컴포넌트의 최상단에 `<>` `</>` 를 지원한다.
- **Dom에 직접 접근하고 싶을 땐, ref를 사용한다.**
- react에서 사용하지 못하는 태그 속성 명이 있다.
    - `class` → `className`
    - `for` → `htmlFor`
- state가 변경 되었을 때, 다시 랜더링 되기 떄문에, 객체의 경우 다른 객체로 대체하여 변경해야 한다.
    - List가 상태인 State에 push해도 다시 렌더링되지 않는다.
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

### Hooks의 특징

이런 기능들을 React팀이 추가한게 `Hooks`임

- `use` 붙은게 Hooks임
- 코드량이 훨씬 줄어든다.
- Hooks는 state가 변경되면, 함수 자체가 다시 실행 된다.
    - 클래스문법은 render()만 다시 실행 된다.
    - `setState`는 비동기 이기 때문에, 여러번 `setState`를 실행해도, 한번 실행된걸로 간주하고 함수는 한번 재 실행된다.
    - `state`나 `props`가 변경 될 때 재 랜더링 된다.
    - 부모 컴포너틑가 재 랜더링되면, 자식 컴포넌트도 재 랜더링 된다.
- Hooks도 클래스문법과 마찬가지로 setState의 이전값을 활용 할 수 있다.
    
    ```jsx
    setResult(prevResult => {
        return '정답 :' + prevResult
    });
    ```
- 
### useState()의 초기값에 함수를 작성 할떄는 주의할 점
```
const NumberBaseball = () => {
    const [result, setResult] = useState('');
    const [value, setValue] = useState('');
    const [answer, setAnswer] = useState(getNumber); // lazy init
    const [tries, setTries] = useState([]);

...
...
```
위 와 같은 함수컴포넌트와 State가 있을 때, 함수안의 내용은 state가 변경되어 재 랜더링 될때마다 실행된다. 
- `const [answer, setAnswer] = useState(getNumber);` 와 같이 `getNumber`함수를 실행하지않고 정의만하면 `answer`의 초기값으로 `getNumber`의 리턴값이 세팅된다.
    - 그 후 재 랜더링 되어도, 함수가 다시 실행되지 않음.
- `const [answer, setAnswer] = useState(getNumber());` 이렇게 `getNumber()`로 작성하여도 초기값이 지정되고, 두번째 랜더링부터는 실행은 되지만, 값을 무시하기 때문에 상관은 없지만, `getNumber`로 작성하는게 좋다
    - 쓸데없이, 함수가 호출되기 떄문에 비효율적이다.

getNumber와 같이 함수만 지정하는것을 lazy init이라고 한다.
- 함수가 호출되어, 값을 세팅해줄때까지 기다린다고 해서 lazy init이라고 함

### useRef()

- Hooks는 `useRef` 로 DOM에 접근한다.
- `useState`는 값이 변경되면 화면이 변경된다.
- Dom의 접근 말고도, 화면의 변경없이 값만 변경할 필요가 있을 때, `useRef` 에 값을 넣고 사용한다.
    - 보통 timeout이나 interval 같은건 ref안에 넣어 사용한다.

---

### 웹팩이란?

- 여러개의 js파일을 하나의 js파일로 합쳐준다.
- node가 필요하다.
    - node는 자바스크립트 실행기
    - 웹팩도 js고, node가 웹팩을 실행한다.

### **create-react-app 기능 엿보기**

여러 js파일들을 웹팩이 `app.js` 파일로 합쳐준다. 

**package.json**

npm을 통해 설치된 패키지 목록을 관리하고 프로젝트의 정보 및 기타 실행 스크립트를 작성하는 파일

**package.json 파일 생성**

```jsx
npm init
```
- 이미 package.json이 존재하면, `npm install`로 라이브러리 다운로드
```jsx
{
  "name": "lecture",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "webpack"
  },
  "author": "simpson",
  "license": "MIT",
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
	  "@babel/core": "^7.22.8", // 기본적인 바벨, 이전 브라우저도 호환되게 최신문법 변환
		"@babel/plugin-proposal-class-properties": "^7.18.6",
	  "@babel/preset-env": "^7.22.7", // 환경에 맞게 변경해주는
	  "@babel/preset-react": "^7.22.5", // jsx파일 변경
	  "babel-loader": "^9.1.3", // 바벨과 웹팩 연결
    "webpack": "^5.88.1",
    "webpack-cli": "^5.1.4"
  }
}
```

- `npm i react react-dom`
    - react와 react-dom패키지를 추가
- `npm i -D @babel/core`
    - 위 명령어로  바벨 관련 패키지 추가
- `devDependencies`는 개발에서만 사용되는 의존성이다.
    - `webpack`과 `babel`은 개발에서만 사용한다.
- 웹팩 다운로드
    
    ```jsx
    npm i -D webpack webpack-cli
    ```
    
    - `-D` : 개발할 떄만 사용
    - 웹팩은 개발할때만 필요하다.
    - `webpack`과 `webpac-cli`를 둘다 설치 해야 한다.
    

```jsx
const React = require('react');
const ReactDom = require('react-dom');
```

이제 npm에 설치했던 패키지를 위와 같이 불러올 수 있다.

**WordRelay.jsx**

```jsx
const React = require('react');
const {Component} = React;

class WordRelay extends React.Component {
    state = {
        text: 'Hello, webpack'
    };
    render() {
        return <h1>{this.state.text}</h1>
    }
}

module.exports = WordRelay;
```

- 컴포넌트 별로 필요한 라이브러리는 위와 같이 추가해야 한다.
- `module.exports` 로 모듈이름 설정

**client.jsx파일 생성** 

```jsx
const React = require('react');
const ReactDom = require('react-dom');

const WordRelay = require('./WordRelay');

ReactDom.render(<WordRelay/>, document.querySelector('#root'));
```

- 밑의 **`webpack.config.js` 파일에서 합칠 js파일에 WordRelay 파일은 추가하지 않아도 된다.**
    - client.jsx파일에서 불러오기 때문에 자동으로 불러와 합쳐준다.
        - `webpack.config.js`의 entry에 추가하지 않아도 된다.

**webpack.config.js 파일 생성**

- 별다른 설정이 없다면, **webpack.config.js 파일명 이어야 한다.**

```jsx
const path = require('path');

module.exports = {
    name: 'wordrelay-setting', //웹팩 설정 이름, 딱히 의미없음
    mode: 'development', // 실서비스에선 production
    devtool: 'eval', // 개발에선 eval, 운영에선 hidden-source-map
    resolve: {
        extensions: ['.js', '.jsx'] // 밑의 엔트리 파일명의, 확장자 알아서 찾아줌
    },

    entry: {
        // client.jsx안에 WordRelay.jsx를 불러오기때문에 안적어줘도 된다.
        app: ['./client']
    }, // 입력

    module: {
        rules: [{
            test: /\.jsx?/,
            loader: 'babel-loader',
            options: {
                presets: ['@babel/preset-env', '@babel/preset-react'],
                plugins: ['@babel/plugin-proposal-class-properties']
            }
        }]
    },

    output: {
        path: path.join(__dirname, 'dist'), // 현재폴더경로/dist
        filename: 'app.js'
    }, // 출력
}
```

- `entry`에 합칠 파일들을, `output`은 합친 파일의 결과물을 의미한다.
    - 입력은 `client.jsx` / `WordRelay.jsx`
    - 출력은 `app.js`
- 여러 파일의 확장자를 일일이 지정하는 것은 어렵기 떄문에, `resolve:extensions:` 옵션을 사용하면 파일의 확장자를 알아서 찾아준다.
- `module`에 바벨 설정을 추가했다.
    - `test` : 정규표현식, `js` 또는 `jsx`파일 인식
    - `loader` : babel-loader를 지정하여, 최신문법 호환되게 변경
    - `options` : babel-loader옵션
        - `presets:` 환경에 맞게 변환적용, jsx변환
            - plugin들의 모음이 presets이다(수십개의 plugin들이 합쳐져있음)
            - presets도 옵션을 지정할 수있다.
            - `presets 옵션 은 아래에 설명`
        - `plugins` : 클래스 프로퍼티(class properties) 문법을 사용할 수 있도록 해준다.

`entry`의 파일들을 읽고, `module`을 적용한 후, `output`으로 뺀다고 생각

**웹팩 실행**

```jsx
npx webpack
```

설정을 마친 후 위 명령어로 `현재경로/dist` 안에 `app.js`파일이 생성된다.

indext.html**파일 생성**

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>끝말잇기</title>
</head>
<body>
<div id="root"></div>
<script src="./dist/app.js"></script>
</body>
</html>
```

- client.jsx파일은 `./dist/app.js`파일만 불러오면 된다.
    - 바벨로 인해 변환된 파일들과, 웹팩으로 합쳐진 결과 파일

**참고 - `create-react-app` 실행 시, webpack.config.js가 없는 이유**

`create-react-app`이 내부적으로 웹팩 설정을 추상화하고 감춘다.

`create-react-app`은 프로젝트의 웹팩 설정을 자동으로 처리하고, 내부에서 사용하는 `react-scripts` 패키지를 통해 웹팩 관련 동작을 제어한다.

`react-scripts`는 프로젝트를 시작하고 개발 서버를 실행하는 데 필요한 모든 설정을 처리합니다.

---

### 웹팩 설정 추가 설명

**webpack.config.js presets옵션 설정**

```jsx
const path = require('path');
const webpack = require('webpack');

module.exports = {
    name: 'gugudan-setting',
    mode: 'development', // 실서비스 : production
    devtool: 'eval',
    resolve: {
        extensions: ['.js', '.jsx'] // 밑의 엔트리 파일명의, 확장자 알아서 찾아줌
    },

    entry: {
        app: ['./client']
    }, // 입력

    module: {
        rules: [{
            test: /\.jsx?/,
            loader: 'babel-loader',
            options: {
                presets: [
                    ['@babel/preset-env', {
                        targets: {
                            browsers: ['> 5% in KR', 'last 2 chrome versions'],
                        },
                        debug: true,
                    }
                    ],
                    '@babel/preset-react'],
                plugins: []
            }
        }]
    },
    plugins: [
        new webpack.LoaderOptionsPlugin({ debug: true}),
    ],
    output: {
        path: path.join(__dirname, 'dist'),
        filename: 'app.js'
    }, // 출력
}
```

위와 같이 `@babel/preset-env` 에 대한 상세 옵션을 지정할 수 있다.

- `@babel/preset-env`  옵션은 최신 문법이 해당 환경에 지원되게 해주는 설정
- 너무 예전 환경까지 포함하면, 바벨이 작업량이 늘어나기 떄문에 설정 해주는게 좋다.
- `한국에서 점유율 5% 이상인 브라우저`, `크롬의 최근 두 번째 버전` 등의 옵션을 지정할 수 있다.
    - `browserslist` 검색
- 추가적인 플러그인은 module과 같은 위치의 plugins에 추가 할 수 있다.
    - 참고 : 위 플러그인은 Lodaer의 Options에 debug true옵션을 전부 넣어주는 플러그인

### 웹팩 데브 서버와 핫 리로딩

**핫리로딩 라이브러리 설치**

```jsx
npm i react-refresh @pmmmwh/react-refresh-webpack-plugin -D
```

- 핫리로딩 기능을 지원하는 라이브러리 두개를 개발용으로 설치

**웹팩 데브 서버 설치**

```jsx
npm i -D webpack-dev-server
```

- 프론트도 개발을 위한 서버가 필요함

**package.json**  변경

```jsx
**{
  "name": "lecture",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "webpack serve --env development" // 변경
  },
  "author": "simpson",
  "license": "MIT",
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@babel/core": "^7.22.8",
    "@babel/plugin-proposal-class-properties": "^7.18.6",
    "@babel/preset-env": "^7.22.7",
    "@babel/preset-react": "^7.22.5",
    "@pmmmwh/react-refresh-webpack-plugin": "^0.5.10",
    "babel-loader": "^9.1.3",
    "react-refresh": "^0.14.0",
    "webpack": "^5.88.1",
    "webpack-cli": "^5.1.4",
    "webpack-dev-server": "^4.15.1"
  }
}**
```

- `npm run dev` 명령어 입력 시, 위의 명령어 실행

**설정**

```jsx
const path = require('path');
const RefreshWebpackPlugin = require('@pmmmwh/react-refresh-webpack-plugin'); // 추가

module.exports = {
    name: 'wordrelay-setting',
    mode: 'development', 
    devtool: 'eval',
    resolve: {
        extensions: ['.js', '.jsx'] 
    },

    entry: {
        
        app: ['./client']
    }, 

    module: {
        rules: [{
            test: /\.jsx?/,
            loader: 'babel-loader',
            options: {
                presets: ['@babel/preset-env', '@babel/preset-react'],
                plugins: [
                    '@babel/plugin-proposal-class-properties',
                    'react-refresh/babel'// (1)추가 
                ]
            }
        }]
    },
    plugins: [
        new RefreshWebpackPlugin() // (2)추가 - 빌드할 때마다 실행 됨
    ],
    output: {
        path: path.join(__dirname, 'dist'), 
        filename: 'app.js',
				publicPath: '/dist', // devServer가 사용하는 결과물간의 상대경로
    }, 
		devServer: { // (3)
				// 웹팩이 빌드한 빌드파일 경로, dev에선 메모리에 저장
        devMiddleware: { publicPath: '/dist' }, 
				// 현재 위치 기준, 실제 존재하는 정적파일 경로 (index.html)
        static: { directory: path.resolve(__dirname) }, 
        hot: true,
				liveReload: false
    }
```

- (2) Refresh 플러그인 추가
- (1) babel-loader에 플러그인 추가
    - 바벨이 변환할 때 핫 리로딩 기능을 추가해준다.
- (3) 데브서버 설정 추가
    - **`webpack-dev-server@4`버전 부터는 위와 같이 devServer설정을 해줘야 함**
    - `hot: true`
        - 핫리로딩 옵션을 사용, build의 결과물을 `/dist` 에 메모리로 저장하고 소스코드의 변경점을 감지하고, 감지되면 결과물을 수정 해준다.
        - 핫리로딩이란, 기존 데이터를 유지하면서 리로딩하는 것을 뜻한다.
        - webpack dev server가 원래 변경점을 감지하였을때, 리로딩은 지원한다
            - 이 경우 데이터 날아감
            - `react-refresh` `@pmmmwh/react-refresh-webpack-plugin` 이 핫리로딩이 가능하게 지원해주는 것
        - `liveReload: false`
            - 이 설정을 해야 리로딩 안하고, 결과가 바뀐다.
    - `npm run dev`  로 실행해도, 메모리에 저장,  실제 빌드 되지 않는다.
- console에 어떤 변경점떄문에 핫 리로드 되는지 알 수 있다.
    ![image](https://github.com/qwe5507/TIL/assets/70142711/da664779-00a1-4323-93de-ef0ae1c4ea59)


### 컨트롤드 인풋 vs 언컨트롤드 인풋

- input에 value와 onChange가 존재하는게 `컨트롤드 인풋`
- input에 아무것도 없는게 `언컨트롤드 인풋`
    - `e.target.children.word.value=''`와 같이 사용하여, value를 대체 할 수 있기 떄문에
    - input의 순서대로 배열 인덱스가 된다.(e.target[0], e.target[1])
    - 순수한 html의 기능
- 기타 여러 기능떄문에 컨트롤드 인풋을 사용하자.
- 리액트는 컨트롤드 인풋 권장


### Require와 import

webpack은 node에서 동작하기 때문에 node문법인 require를 사용해야 한다.

그러나 바벨이 import를 require로 변경해준다. 

둘 다 사용해도 된다.

---

### Map의 Key

- 리액트는 Map안의 컴포넌트에 key를 보고 같은 컴포넌트 인지 판단 및 성능 최적화를 하기 때문에, unique한 값을 넣어줘야 한다.  (ex: 고유한값이나 uuid)
- key에 map의 index를 넣어주면 안됨

---

### 참고: this

화살표 함수를 사용하지 않으면, 함수안에서 this를 사용하지 못한다.(엄격모드에서 undefined)
그러면 constroctor를 사용해야 한다.(코드가 복잡해짐)

왠만하면 화살표함수 사용

- 화살표 함수는 외부 this를 사용

---

### React DevTools

- DevTools에서 React소스는 숨기기 어렵지만, Redux소스는 개발자가 숨겨야 함
    - 데이터 노출 위험

---

### 재 랜더링 방지

react는 state의 값이 변경되지 않아도, `setState()`함수가 호출만 되도 재 랜더링 된다. (class문법 일때)

**class컴포넌트**

```jsx
shouldComponentUpdate(nextProps, nextState, nextContext) {
    if (this.state.counter != nextState.counter) {
        return true;
    }
    return false;
}
```

- class문법에서는 `shouldComponentUpdate` 내장함수를 사용해서 return 값이 true일 경우만, 재 랜더링 하게 할 수 있다.

다른 방법으로는 위 `shouldComponentUpdate`  기능을 구현한 `PureComponent`를 사용해도, 값이 변경되지 않을 때, 재 랜더링이 되지 않는다.

- 참조 관계가 있는 객체의 경우,  값이 변경되었는지, 정확히 판단 할 수 없다.
- class컴포넌트에서만 사용 가능
- `PureComponent`는 내부적으로 `shouldComponentUpdate` 메서드를 구현하여 얕은 비교를 통해 `props`와 `state`의 변경 여부를 확인한다.
- `PureComponent`는 `props`나 `state`의 변경이 없을 경우 재랜더링을 방지하고 이전에 렌더링한 결과를 재 사용한다.
- `shouldComponentUpdate`  가 필요한 경우에는 그냥 `Component`를 쓴다.
- https://ryublock.tistory.com/38

리액트에서 컴포넌트는 `state변경`, `props변경`, `부모컴포넌트가 재랜더링 되었을 떄` 재 랜더링 된다.

`function`컴포넌트는 `PureComponent`가 없지만, 비슷한 기능을 하는 `memo`가 있다.

```jsx
import React, {memo} from 'react';

const Try = memo(({ tryInfo }) => {
    console.log('try 재랜더링')
    return (
        <li>
            <div>{tryInfo.try}</div>
            <div>{tryInfo.result}</div>
        </li>
    )
});
Try.displayName='Try';
export default Try;
```

위와 같은 자식 컴포넌트가 있고, memo함수로 전체 function을 감싼다.

- `memo`는 부모컴포넌트가 재 랜더링 되었을 때, 재 랜더링 되는것을 막아준다.
    - 부모 컴포넌트의 state가 변경되어도 재랜더링 되지 않는다.
    - 부모 컴포넌트에서 전달하는 props의 변경은 재 랜더링 된다.
- `Try.displayName='Try'`
    - `memo`를 사용하면, 랜더링된 컴포넌트명이 이상하게 변경된다, `displayName`으로 다시 Try로 지정해주는게 좋다.
- 일반적으로 반복문을 기점으로 자식 컴포넌트를 사용한다.
    - 자식 컴포넌트는 일반적으로 `PureComponent`나 `memo`로 감싸서 생성한다.

---

### React.createRef

- 클래스에서 Dom을 조작하기 위해 ref를 사용할 때는, creatRef()를 사용해도 되고 기존방식을 사용해도 된다.
- 기존방식
    
    ```jsx
    
    ...
    input; // this.input을 생성
    
    onRefInput = (c) => {
        .. 미세한 로직
        this.input = c;
    };
    
    render() {
        return (
            <>
                <div>{this.state.word}</div>
                <form onSubmit={this.onSubmitForm}>
                    <input ref={this.onRefInput} value={this.state.value} onChange={this.onChangeInput} />
                    <button>클릭!!!</button>
                </form>
                <div>{this.state.result}</div>
            </>
        );
    }
    ...
    ```
    
    - 위 와같이 특정 미세한 로직이 필요한 경우는 기존대로 함수 사용
- createRef()를 사용하면, hooks의 useRef()와 비슷하게 current를 사용한다.

---

### props와 state 연결하기

부모로부터 전달 받은 props는 자식이 변경하면 안된다. 

- 부모 컴포넌트가 뜻하지 않게 변경될 수 있기 때문에
- 부모가 변경해야 함

```jsx
import React, {memo, useState} from 'react';

const Try = memo(({ tryInfo }) => {
    const [result, setResult] = useState(tryInfo.result);

    const onClick = () => {
        setResult('1');
    };

    return (
        <li>
            <div>{tryInfo.try}</div>
            <div onClick={onClick}>{result}</div>
        </li>
    )
});
Try.displayName='Try';

export default Try;
```

자식컴포넌트에서 props를 변경해야 할 경우가 생긴다면, props를 본인의 state로 변경한다음, state를 변경한다.

---
# 라이프사이클

라이프사이클은 컴포넌트가 생성되고 업데이트되며 파괴되는 과정에서 발생하는 일련의 메서드들의 순서와 단계를 의미한다.

라이프사이클 메서드는 컴포넌트가 특정 상태에 도달할 때 실행한다.

## 클래스 문법

`componentDidMount()`

- `render()` 함수가 처음 랜더링 될 때, 실행되는 함수
- state가 변경되어 재 랜더링 될 때는 실행되지 않는다.
- 비동기 요청을 많이한다.
    - ex) `setInterval`..

`componentDidUpdate()`

- 재 랜더링시에 실행되는 함수
- `state`나 `props`가 변경되었을 떄

`componentWillUnmount()` 

- 컴포넌트가 제거되기 직전에 실행 되는 함수
- 부모가 자식 컴포넌트를 없앴을 때
- 비동기 요청에 대한 정리를 많이한다.
    - ex) `setInterval`에 대한 `clear`를 해주지 않으면, 컴포넌트가 없어져도 계속 동작하게 된다.
        - 정리해주지 않으면 여러번 동작하는 것도 문제지만, 메모리 누수가 생긴다.

### 클래스 문법의 라이프 사이클

```jsx
constructor -> 첫 render -> ref -> componentDidMount 
-> (setState/props 바뀔 때 -> shouldComponentUpdate(true) ->  render -> componentDidUpdate)
-> 부모가 나를 없앴을 떄 -> componentWillUnmount -> 소멸
```

## Hooks

hooks에는 라이프사이클이 없지만, 비슷한 기능을 하는 `useEffect`가 있다.

```jsx
useEffect(() => { // componentDidMount, componentDidUpdate 역할 (1대1 대응은 아님)
        
    return () => { // componentWillUnmount 역할
        
    }
}, []);
```

- class의 라이프사이클과 1대1 대응이라고 생각 하면 안되고 조금 씩 다르다
- useEffect의 두번째 인자가 `[]` 빈 배열일 경우, 첫 번째 인자인 함수는 컴포넌트가 처음 랜더링 될 때 실행된다.
    - `componentDidMount`와 유사
- useEffect의 두번째 인자에 요소가 존재하면, 첫 번째 인자인 함수가 해당 요소가 변경 될 때마다 실행 된다. + 처음 랜더링 될 떄도 실행
    - `componentDidUpdate`와 유사 + `componentDidMount`
- useEffect의 두번째 인자가 `[]` 빈 배열일 경우, `컴포넌트가 언마운트 될 때` 첫 번째 인자인 함수의 리턴 값이 실행된다.
    - `componentWillUnmount`  와 같다.
- useEffect의 두번째 인자가 요소가 존재하면, 해당 요소의 변경으로 useEffect가 `새로 시작하기 전에 리턴문 실행` + `컴포넌트가 Unmount`될 때 실행된다.

- 배열안에 여러 개의 요소를 넣으면, 둘 중 하나만 변경되어도 실행 된다.
- 여러개의 useEffect를 사용할 수 있다.
- 두번 째 인자는 꼭 state가 아니여도 된다.
    - useRef등등..
    - 값이 변화하면 재 실행

---

## useMemo

**`useMemo`**는 React의 훅 중 하나로, 불필요한 계산을 피하고 성능을 최적화하는데 도움을 주는 기능이다.

컴포넌트가 리렌더링으로 인해 컴포넌트 내부에서 계산이나 연산 등의 작업이 불필요하게 반복될 수 있는데, 함수의 결과 값을 기억하고, 이전에 계산한 값을 재사용함으로써 불필요한 계산을 피할 수 있도록 도와준다.

동일한 입력 값으로 함수를 호출할 경우 이전에 계산된 결과 값을 반환하며, 입력 값이 변경되었을 때에만 함수를 실행하여 새로운 결과 값을 얻습니다.

```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

- 캐싱 역할
- 두번째 인자인 배열의 값이 변경되면, useMemo안의 함수가 재 실행 된다.
- 두번째 인자가 빈배열인 경우, 처음 한번만 실행 된다.

## useCallback

useCallback은 불필요한 함수 재생성을 방지하고 성능을 최적화하는데 도움을 주는 기능이다

**useMemo와의 차이점**

- `useMemo`는 `함수의 값`을 기억하고 있다.
- `useCallback`은 `함수 자체`를 기억하고 있다.

함수 컴포넌트는 전체가 재 랜더링 되기 때문에, 불필요한 함수 호출이 많아 질 수 있다.

```jsx
const onClickRedo = () => {
    console.log('onClickRedo');
    setWinNumbers(getWinNumbers());
    setWinBalls([]);
    setBonus(null);
    setRedo(false);
    timeouts.current = [];
});
```

위 와 같은 함수를 

```jsx
const onClickRedo = useCallback(() => {
    console.log('onClickRedo');
    setWinNumbers(getWinNumbers());
    setWinBalls([]);
    setBonus(null);
    setRedo(false);
    timeouts.current = [];
}, []);
```

useCallback으로 감싸면 재랜더링이 발생해도 함수를 다시 생성하지 않고 재 사용한다.

- 두번째 인자는 `useMemo`와 같은 매커니즘으로 동작한다.
    - 함수가 재 실행 해야 할 때를 고려하여, 적당히 두번째 인자를 넣어야 한다.
- 호출 빈도가 낮은 경우에는 **`useCallback`**을 사용하는 것보다 그냥 일반 함수로 정의하는 것이 더 효율적일 수 있다.
- **자식 컴포넌트에 함수를 `props`로 넘길 때에는, `useCallback`을 꼭 감싸줘야 한다.**
    - useCallback으로 감싸지 않으면, 자식 컴포넌트는 매번 재 생성되는 함수를 props가 바뀌었다고 생각하고 매번 재 랜더링 된다.

---

### Hooks의 팁

- `Hooks`들은 순서가 중요해서, 조건문, 반복문 등에 넣으면 안된다.
    - `Hooks`는 최상위로 위치하여, 실행순서가 같게끔 유지한다.
- `useEffect`는 두번째 인자에 요소를 넣어도, 항상 처음에 한번 실행 되기 때문에, `componentDidUpdate`처럼 `Update`시에만 동작하게 하고 싶다면 아래와 같은 방식으로, 처음 요청을 무시하고 `Update`시에만 동작하게 할 수 있다.

```jsx
const mounted = useRef(false);
useEffect(() => {
    if (!mounted.current) {
        mounted.current = true;
    } else {
        // ajax
    }
}, []); //componentDidUpdate만, componentDidMount x
```

---

## useReducer

useReducer는 React 컴포넌트 내에서 Redux의 리듀서와 유사한 방식으로 상태 관리를 할 수 있도록 하기 위해 도입된 훅입니다.

- 소규모 앱에서는 useReducer + Context API 조합으로 사용가능 하지만,
    - 비동기 작업에 취약해서
- 대규모 앱에서는 Redux를 사용해야 한다.

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

- **`state`**: 현재 상태를 나타내는 변수입니다.
- **`dispatch`**: 액션을 발생시키는 함수로, 리듀서 함수를 호출하여 상태를 변경합니다.
- **`reducer`**: 리듀서 함수로, 현재 상태와 액션을 받아서 다음 상태를 반환합니다.
- **`initialState`**: 초기 상태로, 컴포넌트가 처음 마운트될 때의 상태를 나타냅니다.

`useReducer`는 주로 다음과 같은 상황에서 사용됩니다:

1. 컴포넌트의 상태가 복잡하고 중첩되어 있는 경우
2. 컴포넌트가 여러 개의 상태를 가지고 있는 경우
3. 상태 변경 로직이 복잡하거나, 여러 컴포넌트에서 공유해야 하는 경우

---

- `setState`와 마찬가지로 변경 감지를 위해 불변성을 지켜야 함으로, state전체를 변경 해야 한다.
- 주의할 점은 `리덕스`는 state가 동기적으로 변경되는데, `useReducer`는 비동기적으로 변경된다.
    - 비동기 작업으로 인해 변경된 값에 대한 change이벤트는 `useEffect`에서 해야 한다.
- 최적화 시, `memo`를 먼저 사용하고, `memo`를 사용해도 쓸데없는 리 랜더링이 발생하면, 그때 `useMemo`로 컴포넌트 자체를 기억하고, 특정 값이 변경되었을 때만 다시 생성하게 한다.

---
## Context API

1. `Context API`
    - React의 기능 중 하나로, 컴포넌트 간에 데이터를 전달하는 방법을 제공한다.
    - 주로 중첩된 컴포넌트 간에 데이터를 전달하거나 글로벌 상태 관리에 사용된다.
    - `React.createContext()`를 사용하여 새로운 컨텍스트를 생성하고, 컨텍스트의 값을 사용할 자식 컴포넌트를 `Provider 컴포넌트`로 감싸 사용합니다.
2. **`useContext`**
    - React의 훅 중 하나로, 함수형 컴포넌트에서만 사용할 수 있다.
    - `useContext`를 사용하여 특정 Context 객체에 접근하여 컨텍스트에서 제공하는 값을 가져올 수 있다.
    - 코드를 간결하게 만들어주고, Context에 저장된 값을 쉽게 사용할 수 있게 해준다.

**context API는 성능 최적화 하기 힘들다.** 

```java
const MineSearch = () => {
    const [state, dispatch] = useReducer(reducer, initialState);
    return (
        <TableContext.Provider value={{ tableData: state.tableData, dispatch }}>
            <Form></Form>
            <div>{state.timer}</div>
            <Table></Table>
            <div>{state.result}</div>
        </TableContext.Provider>
    )
};
```

위와 같이 Provider의 값을 지정하면 `MineSearch`컴포넌트가 리랜더링 될때마다 `Context`의 값이 변경되게 된다.

- `Context`를 사용하는 자식 컴포넌트들도 재 랜더링된다.

```java
const MineSearch = () => {
    const [state, dispatch] = useReducer(reducer, initialState);

    const value = useMemo(() => ({ tableData: state.tableData, dispatch }), [state.tableData]);

    return (
        <TableContext.Provider value={value}>
            <Form></Form>
            <div>{state.timer}</div>
            <Table></Table>
            <div>{state.result}</div>
        </TableContext.Provider>
    )
};
```

위와 같이 Provider의 값을 지정할 떄는, `useMemo`로 감싸주어 캐싱, 특정 값이 변경되었을 때만 재 생성하는게 좋다.

- `dispatch`는 절대 변하지 않는 값이므로, 배열에 추가할 필요가 없다.

위와 같이 적용해도 Context API의 state가 변경되면 어쩔수 없이 재 랜더링 되는 부분이 있다.

```jsx
const Td = memo(({rowIndex, cellIndex}) => {
    const {tableData, dispatch, halted} = useContext(TableContext);

    const onClickTd = useCallback(() => {
			//로직...
    }, [tableData[rowIndex][cellIndex], halted]);

    const onRightClickTd = useCallback((e) => {
			//로직...
    }, [tableData[rowIndex][cellIndex], halted]);

    console.log('td rendered'); // 여기까지 재랜더링

    return useMemo(()=> ( // useMemo로 컴포넌트를 한번더 캐싱
        <td style={getTdStyle(tableData[rowIndex][cellIndex])}
        onClick={onClickTd}
        onContextMenu={onRightClickTd}
        >
            {getTdText(tableData[rowIndex][cellIndex])}
        </td>
    ), [tableData[rowIndex][cellIndex]])// 해당 cell이 변경되었을 때만 컴포넌트 재 생성
});
```

 `td rendered` 부분은 재랜더링 되지만, 실제 컴포넌트를 useMemo로 캐싱하여, 실제 컴포넌트는 값이 변경되었을 때만 재 생성 되게 한다.

- 함수자체는 여러번 실행되지만, 컴포넌트 부분은 한번만 실행된다.

```jsx
 // 로직...
console.log('td rendered');

    return <RealTd onClickTd={onClickTd} onRightClickTd={onRightClickTd} data={tableData[rowIndex][cellIndex]} />;
});
Td.displayName = 'Td';

const RealTd = memo(({ onClickTd, onRightClickTd, data}) => {
    console.log('real td rendered');
    return (
        <td
            style={getTdStyle(data)}
            onClick={onClickTd}
            onContextMenu={onRightClickTd}
        >{getTdText(data)}</td>
    )
});
```

위와 같이 컴포넌트를 분리하여, memo로 감싸는 방법도 있다.


# React-router

### React-router(v6) 설치

```jsx
npm i react-router
npm i react-router-dom
```

- `react-router`
    - react-router 기본기능
- `npm i react-router-dom`
    - 웹 환경에서의 router
    - react native면 `react-router-native` 사용

사용은 `react-router-dom` 만 하고, 내부적으로 `react-router-dom` 가 react-router를 사용한다.

**웹팩 기능 추가**

```jsx
devServer: {
        historyApiFallback: true, // 추가
        devMiddleware: { publicPath: '/dist' },
        static: { directory: path.resolve(__dirname) },
        hot: true,
        liveReload: false
    }
```

**`historyApiFallback`** 설정은 브라우저에서 발생하는 모든 요청을 기본적으로 **`index.html`**로 리다이렉트하여 리액트 SPA의 라우팅을 동작하도록 보장해준다.

- Create React App에는 기본적으로 포함 된다.

- Hooks컴포넌트에서는 React를 불러오는 node_modules가 두 개 이상이면 에러가 발생한다.
    - 클래스컴포넌트는 위와 같은 제약사항이 없음

### `BrowserRouter` vs `HashRouter`

1. `HashRouter`
    - URL에 해시(#) 기호를 사용하여 라우팅을 처리하는 방식
    - URL의 해시 부분을 사용하여 브라우저의 주소창에 표시되는 URL과 실제 라우팅되는 경로를 매핑합니다.
        - 예를 들어, **`http://example.com/#/about`**과 같은 URL을 사용
    - 페이지 새로고침 시에도 서버로 요청을 보내지 않고, 단일 페이지 애플리케이션(SPA)을 구현하는 데 용이하다.
    - URL이 약간 덜 깔끔해 보이고, SEO(Search Engine Optimization)에 불리하다는 단점이 있다.
2. `BrowserRouter`
    - 최신 HTML5 히스토리 API를 사용하여 라우팅을 처리하는 방식
    - HTML5 히스토리 API를 사용하면 브라우저의 주소창에 보여지는 URL과 실제 라우팅되는 경로가 일치한다.
    - 더 직관적이고 깔끔한 URL을 제공하며, 검색 엔진 최적화(SEO)에도 더 유리하다.

대부분의 경우, `BrowserRouter` 를 사용하는 것이 좋다.

### Link

**`Link`** 컴포넌트는 SPA에서 페이지 전환에 최적화되어 있으며, **`<a>`** 태그와 달리 브라우저의 히스토리를 조작하여 새로운 요청 없이 페이지 전환을 구현한다.

- 이를 통해 웹 애플리케이션의 사용자 경험을 향상시키고, 빠른 페이지 이동을 가능하게 합니다

`NavLink` 는 확성화된 Link의 특정 클래스를 추가하여 Link될 떄마다 특정 css나 기능을 추가하기 쉬운 기능이다.

---

### react-router v5 → v6

- 기존의 `Switch`  대신에 `Routes`로 변경되었다.
- `Route` 는 `Routes` 의 직속 자식이어야만 작동한다.
- 서브경로가 필요한 경우 path에 `*` 사용
- 기존의 정확한 path경로를 제공하던 `exact` 키워드는 사라지고, default로 제공된다.
- `/game/:name` 와같은 path 경로명에 값을 추출할때에는 `useParams()` 훅을 사용한다.
- `useHistory`대신 `useNavigate`사용
    - 기능은 거의 동일하나, 뒤로가기 등을 사용할 때, `-1`, `-2` 등 뒤로갈 페이지 숫자를 지정한다.
- 특정 경로로 데이터를 전달 할때에는, navigate의 두번째 인자에 데이터를 실어보낸다.
    
    ```jsx
    const navigate = useNavigate();
    //...
    navigate('/game/number-baseball', {state: {test: 'testtest'}});
    ```
    
    ```jsx
    const location = useLocation();
    
    console.log(location.state); // {test: 'testtest'}
    ```
    
    **`useLocation()`** 훅을 사용하여 현재 페이지의 **`location`** 객체를 가져오고, 그 중에서**`location.state`**를 사용하여 전달된 상태의 **data** 값을 추출합니다.
    
- 특정 쿼리스트링은 `newURLSearchParams` 객체를 통해 간편하게 추출 할 수 있다.
    
    ```jsx
    // url : http://localhost:8080/game/number-baseball?hello=test
    
    const location = useLocation();
    let urlSearchParams = newURLSearchParams(location.search.slice(1)); // 쿼리스트링 추출
    console.log(location.search) // ?hello=test
    console.log(urlSearchParams.get('hello')); // test
    ```
---

![image](https://github.com/qwe5507/TIL/assets/70142711/69b702b7-293f-454c-a23c-43e390e50951)

위 Hooks 라이프 사이클을 보면 Render후에 useEffect를 실행한다.

만약 useEffect에서 값을 변경하면, 처음 화면을 렌더하고, 값을 변경하면 재랜더링 되어 두번 랜더링 되게 된다.

- 고객은 화면의 깜빡임을 느껴 사용자 편의성이 줄어든다.

### useLayoutEffect

useLayoutEffect는 화면을 렌더링 하기 전에 실행 하여 깜빡임을 없앨 수 있다.

- useEffect는 비동기적 실행, useLayoutEffect는 동기적인 실행

### useTransition(출처 : https://doiler.tistory.com/83)

**`useTransition`**은 React의 특별한 훅(Hook) 중 하나로, React v18에서 도입된 기능입니다. 이 훅은 느린 컴포넌트의 업데이트를 비동기적으로 처리하고, 사용자 경험을 향상시키는 데 사용됩니다.

일반적으로 React에서 컴포넌트가 업데이트될 때, 모든 업데이트가 즉시 반영되고 브라우저가 렌더링을 시작합니다. 이 때, 느린 컴포넌트의 업데이트가 발생하면 화면이 끊기거나 사용자 경험이 저하될 수 있습니다. **`useTransition`**을 사용하면 이러한 문제를 해결할 수 있습니다.

```jsx
import { useTransition, config } from 'react';

function MyComponent() {
  const [isPending, startTransition] = useTransition({
    timeoutMs: 3000, // 트랜지션 타임아웃 시간 설정 (옵션)
  });

  const handleClick = () => {
    // 느린 업데이트를 비동기적으로 시작
    startTransition(() => {
      // 업데이트 작업 수행
      // setState, useState 등으로 상태 업데이트

      // 약간의 시간이 소요되는 느린 작업 수행
      // 브라우저가 렌더링을 시작하지 않습니다.

      // 트랜지션이 끝나면 브라우저가 업데이트를 반영합니다.
    });
  };

  return (
    <button onClick={handleClick}>
      {isPending ? 'Loading...' : 'Click Me'}
    </button>
  );
}
```

위의 예시에서 **`useTransition`**을 사용하여 **`isPending`**과 **`startTransition`**을 얻습니다. **`isPending`**은 비동기 업데이트가 진행 중인지를 나타내는 상태 변수입니다. **`startTransition`**은 업데이트를 비동기적으로 시작할 수 있는 함수입니다.

**`startTransition`** 함수를 호출할 때, 느린 업데이트를 수행하는 함수를 전달합니다. 이 함수는 느린 업데이트 작업을 처리하고, 화면 끊김 없이 비동기적으로 처리되도록 합니다. **`startTransition`** 함수를 호출하면 **`isPending`** 상태가 **`true`**로 변경되고, **`timeoutMs`**로 설정한 시간(예: 3000ms)이 지나면 브라우저가 업데이트를 반영합니다. 이렇게 함으로써 브라우저 렌더링이 끊기지 않고 사용자 경험이 향상됩니다.

- **`useTransition`** 훅에서 **`timeoutMs`**를 지정하지 않으면, 기본적으로 약간의 딜레이 후에 비동기 업데이트가 처리됩니다. React는 내부적으로 비동기 업데이트를 최적화하기 위해 자체적인 타임아웃 값을 사용합니다. 이 경우 **`timeoutMs`**를 지정하지 않아도 일반적으로 300ms 정도의 기본 딜레이가 적용됩니다.

### useDeferredValue(출처 : https://doiler.tistory.com/83)

먼저 useDeferredValue는 상태 값 변화에 낮은 우선순위를 지정하기 위한 훅이다.

아래 코드에서 count2의 값을 가장 낮은 우선순위로 상태를 변경하고 싶을 경우에는 useDeferredValue로 count2를 래핑하면 된다.

```jsx
import { useDeferredValue, useState } from "react";

export default function Home() {
  const [count1, setCount1] = useState(0);
  const [count2, setCount2] = useState(0);
  const [count3, setCount3] = useState(0);
  const [count4, setCount4] = useState(0);

  const deferredValue = useDeferredValue(count2);

  const onIncrease = () => {
    setCount1(count1 + 1);
    setCount2(count2 + 1);
    setCount3(count3 + 1);
    setCount4(count4 + 1);
  };

  console.log({ count1 });
  console.log({ count2 });
  console.log({ count3 });
  console.log({ count4 });
  console.log({ deferredValue });

  return <button onClick={onIncrease}>클릭</button>;
}
```

onIncrease 함수를 실행하게 되면 count2의 값은 바뀌게 되었는데 deferredValue의 값은 다른 상태변화가 다 발생하고 가장 나중에 변경되게 된다.

### useDeferredValue vs useTransition

useDeferredValue와 useTransition hook은 상태변화의 우선순위를 낮게 하는 hook이다. 그렇다면 두 hook의 차이점은 무엇일까?

먼저 useDeferredValue는 값을 래핑해서 사용한다. 아래 예시 코드를 보면 count2라는 값을 래핑 해서 count2 값이 바뀌어도 deferredValue는 다른 상태변화가 전부 일어난 후에 바뀌게 된다. 즉, useDeferredValue는 값을 래핑 해서 값의 변화의 우선순위를 낮추도록 한다.

```jsx
const deferredValue = useDeferredValue(count2);
```

다음은 useTransition hook이다. useTransition은 useDeferredValue와 다르게 값이 아닌, 함수를 래핑한다. 아래 예시 코드를 보면  () => { setText(e.target.value) } 함수를 래핑하고 있다. 래핑 된 함수의 우선순위를 낮춰서 다른 상태 변경이 전부 일어난 후 해당 함수를 실행하게 된다.

```jsx
startTransition(() => {
  setText(e.target.value);
});
```

마치 useMemo는 값을 메모이제이션하고 useCallback은 함수를 메모이제이션 하는 것과 같이 값이냐 함수냐로 구분할 수 있다.

### 결론

- useDeferredValue는 상태 값에 우선순위를 낮추는 hook
- useTransition은 상태 변화를 일으키는 함수의 우선순위를 낮추는 hook이다



