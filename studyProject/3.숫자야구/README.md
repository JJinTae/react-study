# 섹션2-숫자야구

참고 : 2022년에는 hot 없어졌음 

## import와 require 비교

require : node의 모듈 시스템

import : 

export default 는 import 로

export const 는 hello = ‘hello’;    import { hello }

module.exports = {hello : ‘a’};

exports.hello = ‘a’ 는 같습니다.

require랑 import가 호환이 된다.

exports 되는게 객체나 배열이면 구조 분해 할 수 있다. 

babel 이 import 를 require 로 바꿔주기도 한다.

→ webpack은 노드가 돌리기 때문에 안됌

## 리액트 반복문(map)

반복되는 것은 배열로 만든다.

key는 고유한 값으로 해준다.

리액트가 key를 보고 같은 컴포넌트인지 아닌지 판단한다.

```jsx
<ul>
  {tries.map((v, i) => {
    return (
      <Try key={`${i + 1}차 시도 :`} tryInfo={v} />
    );
  })}
</ul>
<ul>
	// 중괄호가 없는 경우 바로 return 된다.
  {tries.map((v, i) => (
    return (
      <Try key={`${i + 1}차 시도 :`} tryInfo={v} />
    );
  )}
</ul>

```

## 컴포넌트 분리와 props

```jsx
this.props.value, this.props.index
// 할아버지가 손자한테 물려주는 등 깊어지는 경우 사용하는게 redux or contextAPI
// context에서 더 심화 redux 하지만 redux 가 먼저 나옴 redux에도 context 가 있다.
```

## 주석과 메서드 바인딩

```jsx
// 주석
{/* jsx에서 주석 js도  */}
```

```jsx
// 메서드 바인딩
onSubmitForm(e) {
	this.setState({
		// this가 에러난다. 
	)}
}
```

리액트 배열 push 사용할 const arr2 = […arr2, 1] 이런식으로 줘야한다.

리액트는 값이 변해야 리랜더링 하기 때문이다.

```jsx
// 클래스에서 this.state를 구조분해로 쉽게 선언해서 사용할 수 있다.
render() {
	const { result, value, tries } = this.state;
}
```

## 구조분해 하여 props 쉽게 나타내기 hooks

```jsx
import React, { memo } from 'react';

const Try = memo(({tryInfo}) => {
  return (
    <li>
      <div>{tryInfo.try}</div>
      <div>{tryInfo.result}</div>
    </li>
  );
});
Try.displayName = 'Try';

export default Try;
```

```jsx
useState 에 일반 값을 넣은 경우 단순히 초기화된다.
함수를 넣는 경우 return 값이 answer에 들어가고 다시 호출되어 쓰지도 않는 값도 사용된다.
그냥 단순히 useState(getNumbers()) 라 하면 계속 호출됨 useState(getNumbers) 로 lazy init 준다.

useState 말고 set쪽에서는 같이 함수로 넣어주어야함
```

## shouldComponentUpdate

```jsx
state = {
	counter : 0,
};

shouldComponentUpdate(nextProps, nextState, nextContext){
	if (this.state.counter !== nextState.counter){
		return true;
	} 
	return false;
}
```

## 억울한 자식 리렌더링 막기

부모 컴포넌트가 리랜더링되면 자식 컴포넌트도 리랜더링 된다.

- 이것을 막아주는게 PureComponent Props 도 막아준다.
- 함수컴포넌트의 경우 memo 를 사용한다.(부모컴포넌트 리랜더링 막아줌 state, props 변경은 동일하게 리랜더링)
- PureCompnent 남발하면 복잡해지면 안될 수 있다.
    - shouldComponent 는 커스텀이 가능해서 좋다.
    - but 성능최적화 적으로 memo랑 PureComponent 써주면 좋다.

```jsx
// shouldComonent를 간단하게 씀 단점 : 객체 배열 참조 관계 시 판단 어려움
class Test extends PureComponent {

// PureComponent 배열 객체 아래와 같이 새로 만들어줘야 함 아니면 동일하게 인식한다.
// PureComponent 그냥 컴포넌트 아래와같이 작성하는게 원칙이다.
const array = this.state.array;
array.push(1);
this.setState({
	array: [...this.state.array, 1],
});
// 배열이 중첩으로 있으면 판단이 더 어려워진다. -> 객체 내부에 배열 넣고 배열안에 객체넣고 반복하면
// 판단이 매우 어려워진다. 2depth 이하는 비 추천

// memo
import React, { memo } from 'react';

const Try = memo(({tryInfo}) => {
  return (
    <li>
      <div>{tryInfo.try}</div>
      <div>{tryInfo.result}</div>
    </li>
  );
});

// memo를 씌우면 개발자도구에 displayName이 변경된다 아래와 같이 막을 수 있다.
Try.displayName = 'Try';

export default Try;
```

## React.createRef

```jsx
import {createRef} from 'react'

// class 에서만 사용 

inputRef = createRef(); // 간단하게 ref 사용가능
// 이런식으로 선언하면 아래와 같이 this.inputRef.current.focus();
// this.inputRef 와 hooks 와 동일하게 코딩가능
```

## props와 state 연결하기

```jsx
// 1. props의 값은 부모만 바꿔야 한다.
// 2. props를 바꿔야 하는 경우 state에 넣어서 사용해서 바꾸는 경우가 있다.

const [result, setResult] = useState(tryInfo.state);

const onClick = () => {
	setResult('1');
}
```