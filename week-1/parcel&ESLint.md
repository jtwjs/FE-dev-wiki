# Parcel & ESLint

## 강의 요약

### Parcel

> 특별한 설정 없이 바로 사용 가능한 빌드 도구

- 내부적으로 SWC를 사용해 기존 도구보다 빠르다.
- ES Module을 적극 활용하는 **Vite**도 엄청 빠름
- 참고:
  - [https://github.com/ahastudio/til/tree/main/parcel](https://github.com/ahastudio/til/tree/main/parcel)
  - [https://github.com/ahastudio/til/tree/main/vite](https://github.com/ahastudio/til/tree/main/vite)

**번들러? 빌드?**

- 번들러 → 여러개 파일을 하나의 파일로 합쳐주는 도구
- 번들링 → 하나의 시작점(entry point)로부터 의존적인 모듈을 전부 찾아내서 하나의 결과물로 묶는(번들) 과정을 의미하고 이것을 **빌드**라고도 한다.

**설정이 필요없다지만 추가하면 좋은 설정**

1. package.json 파일에 source 속성 추가.
   매번 번들링할 때 일일히 띄울 타겟을 명시하는 것은 귀찮음
   `npx parcel index.html --port: 8080`

```json
"source": "./index.html",
```

2. parcel-reporter-static-files-copy 패키지 설치 후 `.parcelrc` 파일 작성.
   이렇게하면 static 폴더의 파일을 정적 파일로 Serving 할 수 있다.(이미지 등 Assets)

```json
{
  "extends": ["@parcel/config-default"],
  "reporters": ["...", "parcel-reporter-static-files-copy"]
}
```

**빌드 + 정적 서버 실행**

```bash
npx parcel build

npx server ./dist
```

- [servor - zero config 정적 서버 실행 lib](https://github.com/lukejacksonn/servor)

### ESLint

> 린트(lint), 린터(linter)는 소스 코드를 분석하여 프로그램 오류,버그,스타일 오류, 의심스러운 구조체에 표시(flag)를 달아 놓기 위한 도구들을 가리킨다.

**왜 사용하니?**

- 스타일 통일
- 잠재적 문제 발견
- 베스트 프렉티스(코딩 컨벤션) 추천 → 최신 트렌드 학습 활용
- 일관성 있는 코드를 작성할 수 있다.

**정적 프로그램 분석**

> 실제 **실행 없이 컴퓨터 소프트웨어를 분석**하는 것

> 린트가 정적 프로그램 분석에 속한다.

**`.vscode/setting.json`**

> 파일을 추가해 JS/TS 파일을 저장할 때마다 ESLint를 실행하고 문제점을 고치게 설정 가능

```json
{
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
```

**설정(아샬 Ver)**

- [VS Code 기본 세팅](https://github.com/ahastudio/CodingLife/blob/main/20211008/react/.vscode/settings.json)
- [Trailing Spaces](https://marketplace.visualstudio.com/items?itemName=shardulm94.trailing-spaces)

### CLI

```
npm run check // 타입 체크 검사
// -> "check": "tsc --noEmit",
npm run lint // 린트 검사
// -> "lint": "eslint --fix --ext .js,.jsx,.ts,.tsx .",
```

## 키워드

### Bundler(번들러)

    - Parcel

### Lint(린트)

lint는 보푸라기라는 뜻
프로그래밍 분야에서는 소스 코드를 분석하여 오류 또는 버그가 있는 코드에 표시(flag)를 달아놓기 위한 도구를 가리킨다.
문법적인 오류 또는 안티 패턴을 찾아주어 일관된 코드스타일을 유지하고 가독성 높은 코드 작성을 목표로 한다.

{% hint style="info" %}
**Lint란?**

- lint는 보푸라기라는 뜻

- 문법적인 오류 또는 안티 패턴을 찾아주어 일관된 코드스타일을 유지하고 가독성 높은 코드 작성을 목표로 한다.
  {% endhint %}

**.eslintrc**

- eslint 설정파일
- eslint를 실행하기 위해서는 설정파일이 꼭 필요하다.

**eslint 실행**

- [규칙 목록](https://eslint.org/docs/latest/rules/)중 왼쪽에 렌치 표시가 붙은 것이 `--fix` 옵션으로 자동 수정이 가능한 것

```bash
npx eslint [검사할 파일] // --fix 옵션으로 자동으로 고쳐질수 있는 코드한해서는 eslint가 수정해준다.
```

**Rules**

- ESLint는 코드 검사 규칙을 미리 정해 놓음
- ESLint 설정파일(.eslintrc.js)에 규칙을 작성
- 규칙을 설정하는 값은 3가지 ("off" or 0, "warn" 또는 1, "error" 또는 2)
- [규칙 목록 보기](https://eslint.org/docs/latest/rules/)
- [React 추천 규칙](https://github.com/jsx-eslint/eslint-plugin-react)

```js
module.exports = {
  rules: {
    'no-unexpected-multiline': 'error',
  },
};
```

**Extensible Config**

- 규칙을 여러개 미리 정해 놓은 것이 `eslint:recommended`
- [규칙 목록](https://eslint.org/docs/latest/rules/)중 왼쪽에 체크 표시되어 있는 것이 이 설정에서 활성화 되있는 규칙들

```js
module.exports = {
  extends: ['eslint:recommended'],
};
```

**그 외 규칙 (airbnb)**

- [airbnb: airbnb 스타일 가이드](https://github.com/airbnb/javascript)를 따르는 규칙 모음
- [eslint-config-airbnb-base](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb-base) 패키지로 제공

```js
module.exports = {
  extends: ['airbnb'],
};
```

**git hooks**

> git commit 또는 git push와 같은 git 이벤트가 일어나기 전에 우리가 원하는 스크립트를 실행하기 위해서 git 이벤트 사이에 갈고리(hook)를 걸어주는 것

- 깃 커맨드 실행 시점에 끼여들수 있는 훅을 제공
- `husky` 와 `lint-staged` 를 활용하면 커밋을 푸시하기 전에 린트 검사를 수행
- 즉, 커밋 혹은 리모트 푸시 직전에 린트 검사를 하여 검사를 통과해야만 푸시를 허용하는 방식

{% hint style="info" %}
**husky**

- git hook 동작에 대한 정의를 .git 파일이 아닌 .husky 에서 관리하여 repository에서 공유가 가능하도록 함

- .git 파일은 애초에 .gitignore에 등록되어 있고 배포전에 어떤 동작을 하려던것이 코드에는 적용이 안될 수 있어서 .husky에서 관리 필요.

**lint-staged**

- git staged 상태의 파일들만 뭔가를 할 수 있게 해준다.

{% endhint %}
