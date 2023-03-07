# 타입스크립트

{% hint style="success" %}
**목표**

유효한 프로그램은 통과시키고 무효한 프로그램에는 오류를 발생시키는 것
{% endhint %}

## 타입스크립트는 ‘Compiled Language' 이다.

- 전통적인 Compiled Language 와는 다른 점이 많다.
- 그래서 ‘**Transpile**’ 이라는 용어를 사용하기도 한다.
- **자바스크립트는 ‘Interpreted Language’** 이다.

| Compiled                         | Interpreted                     |
| -------------------------------- | ------------------------------- |
| 컴파일이 필요 O                  | 컴파일이 필요 X                 |
| 컴파일러가 필요 O                | 컴파일러가 필요 X               |
| 컴파일 하는 시점 O → 컴파일 타임 | 컴파일하는 시점 X               |
| 컴파일된 결과물을 실행           | 코드 자체를 실행                |
| 컴파일된 결과물을 실행하는 시점  | 코드를 실행하는 시점 O → 런타임 |

## 강의 요약

### ts-node

> TypeScript 코드를 JavaScript로 컴파일하지 않고 직접 실행시키는 라이브러리

- Typescript **REPL**

### 타입의 재사용

> blah blah

### type alias vs interface

> 내가 선호하는 타입 선언 방식은 무엇이고 그 이유는 무엇인가

### 타입 추론

> 모든 타입을 선언을 해줘야 할까?
> 선언이 꼭 필요한 상황과 아닌 상황을 생각해보자.

### 타입을 집합이라 생각하기

> 할당할 수 있는 값들의 집합

## 심화

### 조건부 타입

### 맵핑된 타입

## 키워드 정리

### REPL (Read-Eval-Print-Loop)

> 입력한 소스코드를 읽고(Read) 평가(Eval)하고 결과를 출력(Print)하는 과정을 반복(Loop)

**REPL 개발 도구**

- 브라우저 개발 도구
- 터미널/쉘
- 온라인 컴파일러(예시: Repl.it, jsfiddle.net, jsbin.com)
- 온라인 정규식 유효성 검사기

### Interface vs Type

**Type Alias 정의**

> 어떠한 것을 **구현할 목적이 아닌 데이터를 담을 목적**으로 만들때 `Type` 사용

**Interface 정의**

> 어떤 **특정한 규격을 정의**하거나 이 **규격을 통해서 어떤 것이 구현**된다면 `Interface`를 사용

**공통점**

> 몇가지 기능을 제외하고 타입 별칭과 인터페이스 모두 동일하게 타입을 지정할 수 있다.

```typescript
type PersonType = {
  name: string;
  age: number;
}
interface PersonInterface {
  name: string;
  age: number;
}

const person1: PersonType = {
  name: '정태웅',
  age: 29
}
const person2: PersonInterface = {
  name: '정태웅',
  age: 29
}
// 구현(implements)
class Person1 implements PersonType {
  name: '정태웅';
  age: 29
}
class Person2 implements PersonInterface {
  nameL '정태웅';
  age: 29
}

// 상속(extends) - 상속을 구현하는 방법은 다르지만 동일한 기능
interface GenderPersonInterface extends PersonInterface {
  gender: 'w' | 'm';
}
type GenderPersonType = PersonType & { gender: string };
```

{% hint style="warning" %}
**Type Alias로 상속을 구현할 때 주의점**

Type Alias 내에 union이 사용된 경우 사용 X -> 정적으로 모양을 알수있는 객체 타입만 동작
{% endhint %}

**차이점**

> Type Alias와 Interface 각각 개별적으로 가지고 있는 기능 및 특징들

- **선언 병합(Declaration Merging)**: Interface는 같은 이름으로 여러번 선언해도 컴파일 시점에서 하나로 합쳐진다
  - Public API에서 사용되는 타입은 항상 Inteface로 작성되어져야 하는 이유이기도 하다.

```typescript
interface Person {
  name: string;
  age: number;
}

interface Person {
  gender: string;
}

// 컴파일 시점
interface Person {
  name: string;
  age: number;
  gender: string;
}
```

- **연산 프로퍼티(Computed Property)**: 대괄호([])안에 표현식(expression)을 이용해 객체의 key값을 정의하는 문법
  - 더 활용성이 높은 타입을 선언해서 사용 가능하고 타입의 재사용성이 높아짐!
  - `Index Type`이라고 불린다.

```typescript
type Person = {
  name: string;
  age: number;
};

type Name = Person['name']; // 타입이 string
```

### 타입 추론

> blah blah

### Union Type vs Intersection Type

**유니온 타입(Union Type): OR**

> 발생할 수 있는 모든 케이스 중 하나만 할당할 때 사용

```typescript
type Direction = 'left' | 'up' | 'right' | 'down';
const move = (direction: Direction) {...}
```

**태그된 유니온(tagged Union)**

> **Descriminated Union**이라고도 불리며 **공통된 속성**을 이용하여 구분하기 쉽게 직관적인 코드를 작성할 수 있다.

- 공통된 속성을 갖는 타입들을 구분하기 위해 쓰이는 `type`을 태그(tag) 또는 식별자(descriminator)라고 부른다.

```typescript
type SuccessStatus = {
  status: 'success'; // 태그 또는 식별자
  result: {};
};
type FailedStatus = {
  status: 'failed';
  error: {};
};

type Status = SuccessStatus | FailedStatus;
```

**타입 가드(Type Guard)**

> 태그된 유니온을 활용한 타입 식별 헬퍼 함수
> 반환 타입의 `is` 구문은 함수의 반환이 true인 경우, 타입 체커에게 매개변수 타입을 좁힐 수 있다고 알려준다.

```typescript
// 반환타입이 불린값이므로 보통 네이밍을 is~로 작명한다.
const isSuccessStatus = (status: Status): status is SuccessStatus => {
  return 'result' in status;
};
```

**인터섹션 타입(Intersection Type): AND**

> 다양한 타입을 하나로 묶어서 사용할 수 있다.

```typescript
type Person = {
  name: string;
  age: number;
}
type Worker = {
  work: () => void;
}

const interWork = (person: Person & Worker) => {...};
interWork({name: '정태웅', age: 29, work: () => {}})
```

### Optional Parameter

> 값이 있을수도 없을수도 있는 타입, 앞에 `?` 를 붙여 선언한다.
> Default 값을 설정해도 Optional Parameter가 된다.

```typescript
interface Person {
  name: string;
  age: number;
  gender: string;
  jobs?: string; // string | undefined
}

const hab = (a: number, b?: number): number => {
  return a + (b || 0);
}
hab(1, 2); // 정상 3
hab(1); // 정상 1

// default 값을 지정하면 동일한 효과! 더 깔끔하다.
cosnt hab2 = (a: number, b = 0): number => {
  return a + (b || 0);
}
hab2(1); // 정상 1

```
