# Testing Library

{% hint style="success" %}
**Testing이란?**

- 소프트웨어의 버그를 찾음

- 제품이 예상하는대로 동작하는지 확인하는 단계

{% endhint %}

## 왜 테스트 코드를 짜야할까?

- 본인이 작성한 코드에 자신감이 생긴다.
- 기능이 정상 동작하는지 확인할 수 있다.
- 요구사항이 만족하는지 점검 가능
- 이슈에 대한 예측이 가능해진다.
- 버그를 빠르게 발견할 수 있다.
- 자신감있게 리팩토링이 가능하다.
- 테스트 코드를 작성하기 위해서 코드간 의존성을 낮추고 코드의 품질을 향상시킬 수 있다.
- 좋은 문서화 도구가 된다.

## 강의 요약

### Jest

> 거의 모든 것을 갖춘 테스팅 도구.

> Mocha와 Chai처럼 RSpec의 describe-it을 지원하고, expect로 단언(assertion)할 수 있다. Mocking도 다양한 레벨에서 쉽게 사용할 수 있다.

{% hint style="info" %}

[BETTER SPECS](https://www.betterspecs.org/) → RSpec **베스트 프랙티스 모음**

[Let's RSpec](https://github.com/ahastudio/til/blob/main/ruby/20161206-rspec-let.md) → 핵심 아이디어 참고용

지원하는 기능과 형식이 완전히 동일하지 않아 그대로 사용할 수 없지만 테스트 코드를 어떤식으로 구성을해야 할지 좋은 예가 되니 많이 참고하자!

{% endhint %}

### 테스트 구성 단계(Give-When-Then)

> 테스트 구성 할 때 Givn, When, Then 이 3가지 단계로 나뉜다

- **Given(Arrange)**: 테스트를 위한 오브젝트를 생성하고 여러가지 **데이터를 준비**하는 단계
- **When(Act)**: **테스트 하고자 하는 코드를 실행**하는 단계
- **Then(Assert)**: 실행된 값을 우리가 **예상하는 값으로 검증**하는 단계
- [Give-When-Then](https://megaptera.notion.site/Given-When-Then-10ab818cd55f4cbca4c93e3c57146c39)

**각 단계에서 중요 포인트**

- **Given**
  - 준비 과정을 여러 테스트에 걸쳐서 반복해서 사용하고 있다면 재사용할 수 있도록 유틸리티 함수로 정의해서 사용
- **When**
  - 내가 코드를 실행했을 때 의도적으로 실패해본다. → expect에 다른 값을 넣어본다.
  - 실패 했을 때 실패하지 않도록 하기 위해서 어떻게 코드를 수정해야 하는지 확인
  - 버그를 수정할 때는 실패하는 테스트를 먼저 만들어서 **버그가 발생하는 상황을 먼저 검증하고 버그를 수정해서 성공하게 만드는 것이 중요**
  - **실패하지 않는 테스트 코드는 있으나 마나하다는 것을 명심!**
  - 모든 테스트 코드는 의도적으로 실패할 수 있어야 한다.
- **Then**
  - 가장 마지막에 작성
  - 내가 하나의 테스트 함수안에서 검사하는게 많다면 여러개의 테스트로 분리할 수 없는지 고민해보자.

**기본적인 테스트 코드**

```jsx
test('add', () => {
  expect(add(1, 2)).toBe(3);
});
```

**BDD 스타일의 테스트 코드**

> 표현력이 좋아지고, 좀 더 고민할 기회를 제공.

- **describe** → 테스트를 할 대상(주어)를 명시
- **context** → 테스트 대상이 놓인 상황
  - 영어로 작성시 반드시 `with` 또는 `when` 으로 작성하는 것이 좋다.
- **it** → 테스트 대상의 행동을 설명
  - it 구문은 최대한 심플하게 작성

```jsx
const context = describe;

describe('add 함수는', () => {
  context('두개의 양수가 주어졌을 때', () => {
    it('두 숫자의 합을 반환한다.', () => {
      expect(add(1, 2)).toBe(3);
    });
  });

  context('하나의 양수와 음수가 주어지면', () => {
    it('항상 하나의 작은 양수보다 작은 값을 반환한다.', () => {
      expect(add(1, -2)).toBe.LessThan(1);
    });
  });
});
```

{% hint style="info" %}

`npx jest --watchAll` → 매번 npm test를 입력하기 귀찮으니 해당 명령어로 간편하게 코드 작성하자 (`"watch:test": "jest --watchAll"` 스크립트 등록하자.)

`npx jest --verbose ` → 내부의 코드설명 까지 디테일하게 보여준다. (`npm run watch:test -- --verbose` )

{% endhint %}

### React Testing Library

> UI에 특화된 라이브러리, 거의 E2E 처럼 쓸 수 있다.

{% hint style="warning" %}

단, “F/E 테스트 = only React 컴포넌트 테스트”가 되는 상황은 최대한 피하는 게 좋다.

본질에 집중하지 못하고 너무 많은 테스트 코드를 작성할 위험이 있다.

유지보수를 돕기 위해 테스트 코드를 작성하는데, 테스트 코드를 잘못 작성하면 오히려 유지보수를 저해할 수 있다.
{% endhint %}

**두 번 세번 봐야하는 영상**

- [프론트엔드(Front-end)도 테스트해야 하나요?](https://youtu.be/-kUmsKRmOnA)
- [Mocking 때문에 테스트 코드를 작성하기 어렵나요](https://youtu.be/RoQtNLl-Wko)

**간단한 테스트 코드**

```jsx
import { render, screen } from '@testing-library/react';

test('Greeting', () => {
  render(<Greeting name='world' />);

  screen.getByText('Hello, world!');

  screen.getByText(/Hello/);

  expect(screen.queryByText(/Hi/)).not.toBeInTheDocument();
});
```

**Jest Matcher**

> Jest는 값을 테스트할 때 다양한 매처함수를 제공한다.

> matcher란 '이거 맞아?' 라고 물어보는 메서드리고 보면 된다.

> 기대한 값이 실제 반환된 값과 일치하는 지를 확인하는 작업을 일컫는다.

- [매처 함수 모음](https://github.com/testing-library/jest-dom)

## 키워드

### Jest

> 자바스크립트 환경에서 테스트할 수 있는 도구

> Test Runner & Assertion 통합 라이브러리

- **Test Runner** → 테스트를 실행 후 결과 생성
- **Assertion** → 테스트 조건, 비교를 통한 테스트 로직

**Jest 특징**

- **Zero config**: 복잡한 설정이 필요없다.
- **Snapshot 제공**: 어떠한 오브젝트 또는 현재의 상태를 캡쳐할 수 있는 기능
- **Isolated**: 독립적이라 테스트 수행 속도 및 성능이 뛰어나다.
- **Great API**: 다양한 테스트 api 제공

### Describe-Context-It 패턴

- 테스트코드를 계층 구조로 만들어 어떤 흐름에서 테스트 코드가 실행되는지 알기 쉽게 설명할 수 있다.

**Describe-Context-It 패턴의 장점**

- 테스트 코드를 계층 구조로 만들어 준다.
- 테스트 코드를 추가하거나 읽을 때 스코프 범위만 신경쓰면 된다.
- "빠뜨린" 테스트 코드를 찾기 쉽다

### React Testing Library

### Test Pyramide

**단위 테스트(Unit Test)**

- 함수, 모듈, 클래스
- **딱 하나의 단위를 각각 테스트**
- 작성하기도 쉽고, 실행, 자동화가 쉽다.

**통합 테스트(Integration Test)**

- 모듈들, 클래스들
- 여러가지의 단위를 묶었을 때의 관계 **상호작용을 테스트**

**UI 테스트(E2E 테스트)**

- 끝과 끝(end-to-end)을 테스트
- **사용자가 실제로 소프트웨어를 사용했을 때 플로우를 테스트**

### 테스트 주도 개발 (Test-Driven Development, TDD)

> 개발(코드 작성)전 테스트 코드를 먼저 작성

**TDD 순서**

1. 테스트 코드 작성
2. 테스트 코드 실행 (Fail)
3. 작성된 테스트가 통과할 수 있도록 간단한 샘플 코드 작성
4. 테스트 코드 실행 (Success)
5. 3,4번 반복하며 코드 완성
6. 코드 리팩토링

**TDD를 하는 이유**

- 어떤식으로 코드를 설계할 것인지에 큰 틀을 잡을 수 있다.
- 모든 요구사항(목표)에 대해 점검하는 시간을 갖을 수 있다.
- 단순히 구현하는것을 넘어서 사용하는 사용자 입장에서 코드를 작성할 수 있다.
  - 인터페이스 위주의 코드 작성
  - 시스템 전반적인 설계 향상
  - 개발 집중력 향상

**언제 TDD를 해야하나?**

> TDD를 따라 코드를 작성하기 전에 테스트 코드를 작성하든, 코드를 다 작성한 뒤에 테스트 코드를 작성하든 중요하지 않다.

- 다만, **코드리뷰를 요청하기 전**, **메인 레포에 머지하기 전**, **사용자에게 배포되기 이전**에 꼭 그에 해당하는 **테스트 코드를 포함시켜야 한다.**
- 요구사항이 명확할 때
- 비즈니스 로직인 경우
- 설계에 대한 고민이 필요할 때

**TDD를 사용하지 않는 경우**

> UI(컴포넌트)를 작성할 때

- UI에 필요한 비즈니스 로직을 따로 빼서 테스트 코드를 작성하기 때문에 UI코드는 TDD를 작성하지 않는다.
