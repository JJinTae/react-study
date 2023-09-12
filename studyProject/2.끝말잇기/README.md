# 섹션1-끝말잇기

## hooks

리액트는 클래스를 사용하지 마라 (기본은 알아야함 이전 코드가 클래스로 짜져있음)

함수컴포넌트 : setState랑 ref 를 사용하지 않는 컴포넌트는 함수컴포넌트를 사용한다.

hooks : state 바뀔 때 함수 전체가 통으로 재실행된다 → 나중에 성능 최적화

```jsx
class Gugudan extends React.Component {

}

const Gugudan = () => {
	return <div>Hello, Hooks</div>;
}

// 함수컴포넌트에서 state랑 ref를 useEffect 기능을 추가한 것을 말한다.
const Gugudan = () => {
	// 아래와 같이 선언 setFirst 는 first를 set하는 변수에 배열쓰고 객체 쓰는 것구조분해 문법 ?
  // state 합쳐 놓으면 setState 할 때 다 선언해줘야돼서 같이 쓰지않는다.
	const [first, setFirst] = React.useState(Math.ceil(Math.random() * 9));
  const [second, setSecond] = React.useState(Math.ceil(Math.random() * 9));
	const inputEl = React.useRef(null); // null 초기값
	const onSubmitForm = (e) => {
    e.preventDefault();
    if (parseInt(value) === first * second) {
      setResult('정답');
      setFirst(Math.ceil(Math.random() * 9));
      setSecond(Math.ceil(Math.random() * 9));
      setValue('');
      inputEl.current.focus();
    } else {
      setResult('땡');
      setValue('');
			// ref는 current 붙혀줘야한다.
      inputEl.current.focus();
    }
  };

return (
      <React.Fragment>
        <div>{first} 곱하기 {second}는?</div>
        <form onSubmit={onSubmitForm}>
          <input
            ref={inputEl}
            type="number"
            value={value}
            onChange={(e) => setValue(e.target.value)}
          />
          <button>입력!</button>
        </form>
        <div id="result">{result}</div>
      </React.Fragment>
    );
}

```

react 는 class 못쓴다. className을 쓴다.(기존에 class 를 쓰기 때문에)

for도 htmlFor 로 바꿔서 써야한다.

## webpack

쓰는이유 : 

- 실무 컴포넌트 1개인 경우 거의 없다.

아래를 자동화 해주는 것이 create-react-app이다.

1. npm init
2. npm i react react-dom
3. npm i -D webpack webpack-cli
    1. -D 옵션은 개발자 옵션 package.json 에 dev에 생긴다.
4. webpack.config.json 생성
5. client.jsx 생성
    
    ```
    // 아래와 같이 모듈을 가져올 수 있다.
    const React = require('react');
    const ReactDom = require('react-dom');
    ```
    
    babel 같은게 더이상 필요없다.
    
    js와 jsx 차이는 jsx 에 파일안에 jsx 문법을 담고있는 것을 알려줄 수 있다.
    
6. module 시스템
    1. 코드 쪼개면 쉬운데 script 하나만 메인에서 선언할 수 있다.
    2. 여기서 바로 웹팩의 등장이다. 파일을 하나로 합쳐주는 과정
7. webpack
    
    ```jsx
    // 입력 resolve로 확장자 쓸 필요 없다.
    entry: {
      app: ['./client'],
    },
    
    // 입력으로 온걸 모듈스로 빼서 output으로 뺀다.
    modules:{
    	// 모듈 예시 아래에 있음 rules ~
    }
    
    // 출력
    output: {
      path: path.join(__dirname, 'dist'),
      filename: '[name].js',
      publicPath: '/dist',
    },
    
    resolve: {
      extensions: ['.js', '.jsx'],
    },
    ```
    
8. webpack 시작
    1. 터미널창에 webpack 입력
    2. 명령어로 등록이 안되어 있을 경우 있다.
        1. package.json 에 scripts를 강제로 실행할 수있다. (npm run dev)
        2. npx webpack
9. babel 안에서도 jsx 설정을 해줘야한다.
    1. babel은 실제 배포할 때 안쓴다.
    
    ```jsx
    npm i -D @babel/core
    npm i -D @babel/preset-env
    npm i -D @babel/preset-react // jsx 바꿔주는거
    npm i babel-loader // babel 연결
    ```
    
    ```jsx
    // presets 를 배열로 바꾸고 옵션을 추가해줄 수 있다.
    module: {
      rules: [{
        test: /\.jsx?$/,
        loader: 'babel-loader',
        options: {
          presets: [
            ['@babel/preset-env', {
              targets: {browsers: ['last 2 chrome versions']},
              debug: true,
            }],
            '@babel/preset-react',
          ],
          plugins: ['react-refresh/babel'],
        },
        exclude: path.join(__dirname, 'node_modules'),
      }],
    },
    
    plugins: [
      new ReactRefreshWebpackPlugin(),
    ],
    ```
    
    react meta viewport 같은거 다 수동으로 넣어주어야 한다.
    

## 웹팩 데브서버와 핫 리로딩

```jsx
npm i react-refresh @pmmmwh/react-refresh-webpack-plugin -D
npm i -D webpack-dev-server

// 기존에 핫로더 사용안함
scripts
dev : "webpack serve --env development"

RefreshWebpackPlugin() 불러와서 플러그인 추가

// 프론트 개발 편의를 위해 사용 일반도 reloading(그냥 새로고침) 되는데 hotreloading 은 데이터가 안날라간다.
devServer:{
	devMiddleware : {publicPath : '/dist/'},
	static:{directory:path.resolve(__dirname)},
	hot:true,
}
```

## 클래스 hooks로 바꾸는법

1. class 지우고 const 로 해서 함수로
2. state는 const[word, setWord] = useState(’jjintae’) 이런식으로 // 사용 전에 require 필요
3. this를 안쓰고 바로바로 쓴다 ~ setWord ~ 
4. ref 는 .current 반드시 붙힌다.

## 컨트롤드 인풋 vs 언컨드롤드 인풋

인풋은 위 두가지가 있다.

언컨트롤드 인풋 - 원시적임 but 간단함

- 원시적임
- 비밀번호 체크 안됨 - dynamic input
- 비밀번호 서브밋 디스에이블 안됨
- 초기값 defaultValue 로 넣어야함 value 로 넣으면 컨트롤드 인풋으로 간주될 수 있음

컨트롤드 인풋 - 

- 기능이 많기 때문에 컨트롤드 인풋을 권장한다.