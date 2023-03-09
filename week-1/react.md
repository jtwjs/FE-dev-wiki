# React

## 강의 요약

### 리액트로 사고하기

> **상태**를 골라 내는게 핵심

**상태를 골라내는 방법**

- 시간이 지나도 변하지 않는 값은 상태가 아니다.
- `props`로 전달된 데이터는 상태가 아니다.
- **컴포넌트의 기존 상태 및 `props`에서 계산될 수 있는 값은 상태가 아니다**

### ReacfDOM.createRoot

> 리엑트가 만든 가상 돔을 실제 돔에 안착시키는 함수!

- `createRoot(domNode, options?)`는 브라우저 DOM 요소 내부에 콘텐츠를 표시하기 위한 React 루트를 생성한다
- 여러개의 Root를 만들거나, 여러번 렌더 해도 된다.

```typescript
import ReactDOM from 'react-dom/client';

import App from './App';
import Demo from './Demo';

const main = () => {
  const element = document.querySelector('#root');
  const ohterElement = document.querySelector('#other-root');

  if (!element || !ohterElement) {
    return;
  }

  const root = ReactDOM.createRoot(element);
  const otherRoot = ReactDOM.createRoot(ohterElement);

  root.render(<App />);

  let count = 0;
  // 여러 Root 생성 가능
  otherRoot.render(<Demo count={count} />);

  setInterval(() => {
    count += 1;
    // 여러번 렌더 가능
    otherRoot.render(<Demo count={count} />);
  });
};

main();
```

**Root 내장 메소드**

> createRoot는 `render` 및 `unmount`라는 두 가지 메서드가 있는 객체를 반환합니다.

```typescript
export interface Root {
  /* 내부적으로 리엑트가 만든 가상돔 객체를 이전 가상돔 객체와 비교해서 다른 부분만 실제 돔에 반영 */
  render(children: React.ReactNode): void;
  /* */
  unmount(): void;
}
```

### 리엑트 컴포넌트는 언제 리렌더링이 될까?

- 상태 또는 `props`가 변경이 될 때
- 부모 컴포넌트가 리렌더링이 되면 자식 컴포넌트도 리렌더링이 된다.
- [React는 언제 다시 리렌더링이 될까요?](https://velog.io/@surim014/react-rerender)
- [왜 리액트에서 리렌더링이 발생하는가](https://medium.com/@yujso66/%EB%B2%88%EC%97%AD-%EC%99%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8%EC%97%90%EC%84%9C-%EB%A6%AC%EB%A0%8C%EB%8D%94%EB%A7%81%EC%9D%B4-%EB%B0%9C%EC%83%9D%ED%95%98%EB%8A%94%EA%B0%80-74dd239b0063)
- [React 렌더링 동작 완벽 가이드](https://velog.io/@superlipbalm/blogged-answers-a-mostly-complete-guide-to-react-rendering-behavior)

### 리액트는 라이브러리? 프레임워크?

> React 개발자의 답변은 "It's both"

### 제어의 역전(Inversion of Control, IoC)

> 'Don't call us, we'll call you' (우리한테 연락하지 마세요. 우리가 당신에게 연락할게요.)

- 프레임워크의 주요한 특징
- 기존의 모든 제어를 클라이언트 코드가 가지도록 구현 하던 것에서 기존 구조적 설계와 비교해 프레임워크(Container)가 제어를 나누어 가져가되되어 의존 관계가 방향이 달라지게 되는 것을 제어가 반전,역전 되었다 라고 한다.
- React는 **IoC를 통해 상태와 업데이트가 얽힌 복잡한 상황을 간단히 선언형 UI로 구성하는 혜택**을 누린다
- [react 및 redux의 IoC 와 DI](https://medium.com/@magnusjt/inversion-of-control-and-di-in-reactjs-and-redux-35161fcef847)

{% hint style="info" %}
**선언형 이란?**

프로그램이 어떤 방법으로 해야 하는지를 나타내기보다 **무엇과 같은지를 설명하는 경우**에 **선언형**이라고 한다

즉, **값이 무엇이 되어야 하는가?** 라는 선언전인 사고를 의미한다.
{% endhint %}

## 키워드

### React란?

> 가상 돔(Virtual DOM)으로 DOM이 가지고 있는 문제점을 해결하여 UI 개발을 쉽게 도와주는 Library

**DOM이 가진 문제점이 뭐야?**

- DOM API는 기능이 많고 복잡한 구조를 가지고 있어서 사용하기 어렵고 이해하기 어렵다.
- DOM을 안쓰면 HTML을 조작할 수 없다. 즉, DOM을 써야만 UI 개발을 할 수 있다.

```js
// DOM API로 자바스크립트를 통해 HTML을 조작하는 것은 구조를 파악하가 어렵다.
const todoContainer = document.createElement('li');
const todoItem = document.createElement('div');

todoContainer.classList.add('list-group-item');
todoContainer.appendChild(todoItem);
```

**그래서 React는 DOM을 어떻게 해결했는가?**

- 사용자인 개발자에게 다루기 어려운 `DOM`을 쓰게 하지말고 친숙한 HTML 마크업 문법을 통해 UI를 개발하도록 제공
- 리엑트는 마크업 구조인 `jsx` 문법을 제공하며 사용자에게 쉬운 구조(HTML) -> 어려운 구조(DOM API)로 변환하는 트랜스파일링을 필요로 한다.

{% hint style="info" %}
다루기 까다로운 무언가가 있다면 다루기 쉬운 포맷으로 바꿔서 그 쉬운 포맷으로 다뤄보자는 아이디어
{% endhint %}

### React 컴포넌트

### React 리렌더링

>

### IoC(Inversion of Control)

> 제어 반전, 제어의 반전, 역제어는 프로그래머가 작성한 프로그램이 재사용 라이브러리의 흐름 제어를 받게 되는 소프트웨어 디자인 패턴

전통적인 프로그래밍에서 흐름은 프로그래머가 작성한 프로그램이 외부 라이브러리의 코드를 호출해 이용합니다.

하지만 제어 반전이 적용된 구조에서는 외부 라이브러리의 코드가 프로그래머가 작성한 코드를 호출합니다.

- [제어의 역전이란?](https://develogs.tistory.com/19)

**IoC Container**

> IoC (Inversion of Control) 를 구현하는 프레임워크로 객체를 관리하고, 객체의 생성을 책임지고, 의존성을 관리하는 컨테이너 입니다.

- 모든 의존성을 컨테이터(Container)를 통해서 받아오게 되니 실수로 서로 다르게 구현 될 일이 없다.

**의존성 주입(Dependency Injection, DI)**

> 외부로부터 전달받는 것을 의존성 주입이라 한다.

**Ioc의 목적**

- **작업을 구현하는 방식과 작업 수행 자체를 분리**
- 모듈을 제작할 때, 모듈과 외부 프로그램의 결합에 대해 고민할 필요 없이 **모듈의 목적에 집중**할 수 있다..
- 다른 시스템이 어떻게 동작할지에 대해 고민할 필요 없이, **미리 정해진 협약대로만 동작하게 하면 된다.**
- 모듈을 바꾸어도 다른 시스템에 부작용을 일으키지 않는다.

### Library vs Framework
