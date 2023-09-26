---
title: useEffect의 의존성 배열
author: hyebeen
date: 2023-09-26 16:14:00 +0800
categories: [React, Hooks]
tags: [hooks, useEffect]
image: /assets/img/posts/react.jpeg
---

[🏃🏻‍♀️ [원티드 프리온보딩](https://www.wanted.co.kr/events/pre_ob_open?utm_source=google&utm_medium=sa&utm_campaign=kr_career_edu_web_sa_application&utm_term=%ED%94%84%EB%A6%AC%EC%98%A8%EB%B3%B4%EB%94%A9&gclid=CjwKCAjwgsqoBhBNEiwAwe5w03KWAY6SbHLCk7wchiG9o1e4i2-6AqgWMsHzEg2pOS70Rf12g6JFhRoCi9UQAvD_BwE) 학습 후 정리한 내용입니다.]

## 📝 의존성 배열

### 1. 의존성 배열이란?

- 얕게 이해하기

  - useEffect에 두번째 인자로 넘기는 배열입니다.
  - 두번째 인자를 넘기지 않으면 Effect는 매번 실행되고, 빈 배열을 넘긴다면 컴포넌트의 첫번째 렌더링 이후에만 실행됩니다.

- useEffect의 문법
  - effect는 함수의 형태로 표현
  - 의존성은 여러 의존성들을 한번에 전달하기 위해서 배열의 형태로 표현

```js
useEffect(effect, 의존성);
```

💡 useEffect에서 의존성 배열이란?

- “무언가(effect 함수)가 의존하고 있는 요소들의 모음”
- 즉, useEffect의 의존성 배열은 **“effect 함수가 의존하고 있는 요소들의 모음”** 이라고 할 수 있습니다.
- **“의존하고 있다”** = effect 함수가 사용하고 있는 외부의 값들이 의존성

```js
function Component(){
	const [count, setCount] = useState(0);

	const effect = () => {
		document.title = `you clikced ${count} times`
	};

	useEffect(effect, [count]];
}
```

위의 예시에서 effect 함수는 count 라는 외부의 값을 내부에서 사용하고 있습니다.
따라서 effect 함수의 의존성은 count 이며 count를 의존성 배열에 넣어줘야하는 것입니다.

이렇게 되면 useEffect는 리렌더링이 된 후 의존성 배열을 검사해서 의존성 배열에 있는 값들이 변경되었을 경우에 다시 새로운 의존성을 가지고 effect를 실행시켜 줍니다.

### 2. useEffect 의존성 배열의 잘못된 활용

```js
function Component() {
  const [count, setCount] = useState(0);

  // 잘못된 사용: 빈 의존성 배열
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, []); // 의존성 배열이 비어있음
}
```

- 잘못된 사용 : 예시에서 useEffect의 의존성 배열이 [] (빈 배열)로 설정되어 있습니다.
- 빈 의존성 배열을 사용하면 효과 함수가 컴포넌트가 처음 렌더링될 때만 실행되고, 그 이후에는 더 이상 실행되지 않습니다.
- 따라서 count 상태값이 변경될 때마다 페이지 제목을 업데이트하려면 count를 의존성 배열에 추가해야 합니다.
- 이렇게 하지 않으면 컴포넌트가 리-렌더링될 때마다 페이지 제목이 업데이트되지 않으며, count 값이 변경되어도 이 변경이 반영되지 않습니다.
- 올바른 사용 방법은 [count]와 같이 count를 의존성 배열에 명시하는 것입니다.

### 3. useEffect 의존성 배열을 잘 설정하는 법

#### **“가능하다면 의존성을 적게 만들어라”**

```js
// bad
function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const intervalID = setInterval(() => {
      setCount(count + 1);
    }, 1000);

    return () => clearInterval(intervalID);
  }, [count, setCount]); // count와 setCount 둘 다 의존성 배열에 포함

  return (
    <div>
      <h1>count:{count}</h1>
    </div>
  );
}
```

```js
// good
function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const intervalID = setInterval(() => {
      //함수 형태로 setCount를 사용하여 이전 상태 값을 기반으로 업데이트
      setCount((prevCount) => prevCount + 1);
    }, 1000);

    return () => clearInterval(intervalID);
  }, [setCount]); //setCount만 의존성 배열에 명시

  return (
    <div>
      <h1>count:{count}</h1>
    </div>
  );
}
```

- 함수 형태로 setCount를 사용할 때

  - 의존성을 최소화하고 효율적으로 업데이트를 처리하는 방법
    - 함수형 업데이트는 이전 상태 값을 기반으로 하기 때문에 의존성 배열에서 해당 상태를 제거할 수 있습니다.
    - 이렇게 하면 count를 의존성 배열에서 제외함으로써 의존성을 최소화하고 불필요한 실행을 방지합니다.

#### **“모든 의존성을 빼먹지 말고 의존성 배열에 명시해라”**

```js
// bad
function Component(){
	const [count, setCount] = useState(0);

	useEffect(()=>{
		document.title = `you clikced ${count} times`
	}, []]; //count 값이 변경되어도 useEffect가 다시 실행되지 않습니다.
}

// good
function Component(){
	const [count, setCount] = useState(0);

	useEffect(()=>{
		document.title = `you clikced ${count} times`
	}, [count]]; // count 값이 변경될 때마다(count에 대한 의존성) useEffect 콜백 함수가 실행
}
```

BUT, 함수 컴포넌트의 내부에서 선언한 Object, Function는매 호출마다 새로운 인스턴스가 생성되고,
참조형 데이터 타입의 특징을 가지므로 객체 내부의 요소들이 동일하더라도 새롭게 생성된 객체와 이전 객체를 비교하면 서로 다른 객체로 간주됩니다.

따라서 아래의 코드는 무한 루프를 반복

```js
function Component(){
	const [count, setCount] = useState(0);

	const increaseCount = () => {
		setCount(prev => prev + 1);
	}

	useEffect(increaseCount, [increaseCount]];
}
```

#### 💡 해결방안

- 의존성을 제거하기 ⇒ 함수를 effect 안에 선언하기
- 함수를 컴포넌트 바깥으로 이동시키기
- 메모이제이션 사용하기

📚 참고: [A Complete Guide to useEffect](https://overreacted.io/a-complete-guide-to-useeffect/)
