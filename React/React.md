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
- Hooks는 `useRef` 로 DOM에 접근한다.
- Hooks는 state가 변경되면, 함수 자체가 다시 실행 된다.
    - 클래스문법은 render()만 다시 실행 된다.
    - setState는 비동기 이기 때문에, 여러번 setState를 실행해도, 한번 실행된걸로 간주하고 함수는 한번 재 실행된다.
- Hooks도 클래스문법과 마찬가지로 setState의 이전값을 활용 할 수 있다.
    
    ```jsx
    setResult(prevResult => {
        return '정답 :' + prevResult
    });
    ```

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
