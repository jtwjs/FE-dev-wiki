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

### 타입 추론

> 모든 타입을 선언을 해줘야 할까?
> 선언이 꼭 필요한 상황과 아닌 상황을 생각해보자.

## 더 상세히 공부할 것

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

### 타입을 집합이라 생각하기

> 타입을 **할당가능한 값들의 집합(범위)**이라 생각하자.

> 타입이 값의 집합이라는 건 **동일한 값의 집합을 가지는 두 타입은 같다**는 의미

- 가장 작은 집합은 아무 값도 포함하지 않은 공집합이며 아무것도 할당 할 수 없는 **`never`** 타입이다.
- 그 다음으로 작은 집합은 한가지 값만 포함하는 **리터럴 타입**
- 할당가능한 값들의 집합을 여러개로 묶는 경우 값 집합들의 합집합인 **유니온 타입**을 사용

{% hint style="info" %}
집합의 관점에서 **타입 체커의 주요 역할은 하나의 집합이 다른 집합의 부분 집합인지 검사**하는 것이다.
{% endhint %}

### 구조적 타이핑 & 덕 타이핑

> **타입은 '봉인'되어 있지 않다.**

- 자바스크립트는 본질적으로 덕 타이핑(duck typing) 기반
- 타입스크립트의 타입 시스템은 이러한 자바스크립트 런타임 동작을 모델링하기 위해 **구조적 타이핑**을 사용

{% hint style="info" %}
**구조적 타이핑(Structural Typing)**

명시적으로 선언한 타입들로 타입을 구분짓는 것이 아닌 집합의 관점에서 어느 집합이 특정 집합에 값들을 모두 포함하고 있다면 구조적으로 동일한 타입으로 간주한다는 얘기

즉, **어떠한 값이 다른 속성도 가질 수 있음을 의미**한다.

{% endhint %}

{% hint style="info" %}
**덕 타이핑이란(Duck Typing)?**

객체가 어떤 타입에 부합하는 변수와 메서드를 가질 경우 객체를 해당 타입에 속하는 것으로 간주하는 방식

"만약 어떤 새가 오리처럼 걷고, 헤엄치고, 꽥꽥거리는 소리를 낸다면 나는 그 새를 오리라 부를 것이다."

{% endhint %}

**& 인터섹션**

> & 연산자는 두 타입의 교집합을 계산한다.

> 인터섹션 타입의 값은 **각 타입 내의 속성을 모두 포함**하는 것이 일반적인 규칙이다.

- 두 인터페이스가 겹쳐지는 부분이 없어서 공집합이라고 예상하기 쉽다.
- 하지만 타입스크립트의 구조적 타이핑으로 **추가적인 속성을 가지는 타입도 여전히 그 타입**이라는 점을 생각해본다면 `A & B`는 두 타입을 모두 갖는 타입이 된다.

```typescript
interface Person {
  name: string;
} /* name: string 값을 가지고만 있다면 Person
const birthPerson = {name: 'gg', birth: 0};
 -> const person: Person = birthPerson; // 정상
*/

interface Lifespan {
  birth: number;
} /* 동일하게 birth: Date 값을 가지고만 있다 해도 Lifespan이다.
const nameLifespan = {birth: 0, name: 'gg'};
  -> const lifeSpan: Lifespan = nameLifespan // 정상
*/

/* 결국 {name: string, birth: Date, ...} 와 {birth: Date, name: string ,...}의
 교집합을 생각한다면 {name: string, birth: Date}가 된다. */
type PersonSpan = Person & Lifespan;

const ps: PersonSpan = {
  name: 'string',
  birth: Date,
  others: string;
}; /* 객체리터럴로 바로 할당하게되면 잉여속성체크가 일어나 타입 체커가 오류를 발생시킨다.*/
```

### 잉여속성 체크

> 객체 리터럴에 알 수 없는 속성을 허용하지 않는것, 그래서 ‘엄격한 객체 리터럴 체크' 라고도 불린다.

- **타입이 명시된 변수에 객체 리터럴을 할당 할 때** 타입스크립트는 해당 타입의 속성이 있는지, 그리고 그 외의 속성은 없는지 확인한다.
- 객체 리터럴은 추가적인 속성을 허용하는 **구조적 타이핑의 한가지 예외**라고 생각하자

```typescript
interface Person {
  name: string;
}

const logName = (person: Person) => {
  console.log(person.name);
};

logName({ name: '정태웅', age: 29 });
/*'{ name: string; age: number; }' 형식의 인수는 'Person' 형식의 매개 변수에 할당될 수 없습니다.
  개체 리터럴은 알려진 속성만 지정할 수 있으며 'Person' 형식에 'age'이(가) 없습니다 */

/* 이렇게 오류가 발생하지 않는다면 이 코드를 보는 사람은 위 함수가 age에 관해서도 뭔가 처리할 것이다 라고 오해를 불러 일으 킬 수 있다.*/

const agePerson = {
  name: '정태웅',
  age: 2,
};
// 객체리터럴이 아니라면 잉여속성 체크가 일어나지 않는다
const person: Person = agePerson; // 정상
```

**언제 잉여 속성체크가 수행되는가?**

- 객체 리터럴을 변수에 할당할 때
- 객체 리터럴을 함수에 매개변수로 전달할 때
- **오직 객체 리터럴에만 적용된다.**

**객체 리터럴일 때만 이런 식의 타입 검사가 수행되는 이유**

> 속성이 추가로 입력되었지만 그 속성이 "실제로 사용되지 않는다면" 거의 항상 오타가 발생한 경우이거나 API를 잘못 이해한 경우이기 때문입니다.

### unknwon vs never
