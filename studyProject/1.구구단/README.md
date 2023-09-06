# 섹션0-구구단

자바스크립트 최신 문법은 2015년이다. → 결국 최신이라 할 것도 없이 알아야하는 거임.

class 를 쓰는 경우는 1% errorBoundary 에서만 쓴다.

- 컴포넌트 : 데이터와 화면으로 이루어져 있다.
    - 화면의 바뀌는 부분을 state 로 만든다 생각하자.
    - render(){} 의 return : 화면
    - state : 데이터

```jsx
<script>
	// root id 에 LikeButton 을 그린다.
  ReactDOM.createRoot(document.querySelector('#root')).render(e(LikeButton));
</script>
```

- babel : javascript 를 해석해서 표시해준다. - return을 주석처럼 변경 시켜줌

```jsx
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>

<script>
	// return React.createElement('button', {onClick: () => this.setState({liked: true})}, 'Like');
	return (
    <button onClick={() => this.setState({liked: true})}>
      Like
    </button>
  );
</script>
```

- ReactDom : React 17 과 18에서 문법이 다르다.
    - render 를 사용해도 되지만 18 버전 코드가 돌아가지 않는다.
    - 현재 대부분 개발은 React 17 되어있을 것이다. 이걸보고 버전을 확인 할 수 있다.

```jsx
<script type="text/babel">
	// react 17
	ReactDOM.render(<LikeButton/>, document.querySelector('#root'));
	// react 18
  ReactDOM.createRoot(document.querySelector('#root')).render(<LikeButton/>);
</script>
```

- React 조심해야 할 것
    - React 에서는 컴포넌트는 반드시 대문자, 기존 Html 태그들은 소문자이어야 함.
        - JSX는 html이 아니라 xml 이기 때문이다.(xml은 엄격하다)
    - <input type=”text”/> 와 같이 /> 닫는 태그 반드시 붙혀줘야한다.
    - javaSciprt 코드의 경우는 반드시 {} 로 감싸줘야한다.
        - 객체의 경우 obj = {{a:’b’, c:’d’}} 와 같이 써줘야 함.
        - 삼항연산자도 똑같다. {this.state.liked ? ‘Liked’ : ‘liked!’}
            - if 문 못쓴다.
        - {[1, 2, 3].map((i) ⇒ { return <div>i</div>  })}
            - for 문도 못쓴다.
    - return 에는 반드시 하나의 태그만 와야 한다.
        - <> </> 프래그먼트

리액트는 데이터를 중심으로 생각 화면에서 달라질 부분을 state를 만들어야한다

- react 에서 객체를 함부로 바꾸지 마라.
    - 새로운 객체를 만든 후에 바꿔줘라( 복사해라 )
        - react 에서 만들어줬다. setState
    - pop, push, shift, unshift, splice 등 사용 X
    - const array = []; // 배열도 객체이다.
    - function a() {}; // 함수도 객체이다.
    
    ```jsx
    render() {
      if (this.state.liked) {
        return 'You liked this.';
      }
    
      return (
        <button onClick={() => {
    			// this.setState({liked: true})
    			// pop, push, shift, unshift, splice -> 배열을 직접적으로 수정 react 에서는 사용 하면 안된다.
    			// concat, slice -> 새로운 배열을 만들어 냄
    			this.state.liked = true; // 객체를 함부로 바꾸지 마라.(불변성)
    		}}>
          Like
        </button>
      );
    }
    ```
    

예전에 사용했던 문법들(참고만)

```jsx
class LikeButton extends React.Component {
  constructor(props) {
    super(props);
    this.state = {liked: false};
    
    // v1 아래 return this.onClickButton을 실행 시킬려면 이런식으로 하기도 했다.
    this.onClickButton = this.onClickButton.bind(this);
  }
  // v1
  onClickButton() {
    this.setState({liked:true});
  }

  // v2 또 이렇게 하면 된다.
  onClickButton = () => {
    this.setState({liked:true});
  }

  render() {
    if (this.state.liked) {
      return 'You liked this.';
    }

    return (
      // <button onClick={() => this.setState({liked: true})}>
      //   Like
      // </button>
      <button onClick={this.onClickButton} >
        LIKED  
      </button>
    );
  }
}
```

이때부터 클래스는 잘 안쓰게 되는데 …

함수 컴포넌트(함수형 아님)

- this 를 쓸 필요가 없다.
- 구조분해문법
    
    ```jsx
    const [liked, setLiked] = React.useState(false); // 구조분해
    // const result = React.useState(false);
    // const liked = result[0];
    // const setLiked = result[1];
    
    // 객체는 완전히 다르다.
    const {liked, setLiked} = React.useState(false); // 객체
    ```
    

React DevTools

- [https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)