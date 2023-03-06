# 개발 환경 세팅

{% hint style="success" %}
매번 개발 환경 세팅 할 때 마다 애먹었던 이유는 **변화가 빠르기 때문!**

**전체적인 흐름을 파악**하고 앞으로의 **변경에 대응할 수 있는 능력을 키우자.**
{% endhint %}

## TypeScript + React + Jest + ESLint + Parcel 개발 환경 세팅

### npm

> npm init

**npm init시 묻는 옵션 내용**

```bash
package name: (my-app)
version: (1.0.0)
description:
entry point: (index.js)
test command:
git repository:
keywords:
author:
license: (ISC)

About to write to /Users/jeongtaeung/Develop/my-app/package.json:

{
  "name": "my-app",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}

Is this OK? (yes)
```

{% hint style="info" %}
npm i -y
-> 추가적인 옵션 물음에 전부 적용하겠다!
{% endhint %}

### package.json

```json
{
  "name": "my-app", // 디폴트 값 상위  디렉토리명, kebab-case로 많이 사용하는 추세
  "version": "1.0.0", // 프로젝트 시멘틱 버전
  "description": "", // 프로젝트 내용 설명
  "main": "index.js",
  "scripts": {
    // 실행 예약어들
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

### .gitignore

> 잊지말고 꼭 작성하자.
> 최소 `/node_modules` 를 통으로 깃에 올리는 일을 사전에 방지할 수 있다.

```bash
touch .gitignore
```

**.gitignore**

```
// 모두 같은 의미
/node_modules/ -> 패키지 내 루트에 있는 녀석 + 폴더 강조
node_modules/  -> 폴더라는 의미만  강조
node_modules   -> 폴더든 뭐든 알게뭐야 그냥 노드 모듈즈지 ㅋㅋ
```

**.gitignore 그냥 가져다 쓰자**

1. [.gitignore 생성 웹사이트](https://www.toptal.com/developers/gitignore)
2. [github gitignore repo 🥰](https://github.com/github/gitignore)

### dependencies

**devDependencies는 뭔데?**

> 배포될 때 쓰이는 것이 아닌 도구로서 쓰이는 것들을 devDependencies로!

```bash
npm install --save--dev [package]
npm i -D [package] // 축약형
```

- 배포하는 경우 devDependencies를 빼고 설치 가능하다
- 배포할 때 devDependencies를 제외하게 되면 배포하는 크기를 줄일 수 있고 악의적인 공격을 막을 수 있다? -> 좀 더 알아 볼 것

{% hint style="warning" %}
dependency와 devDependency 차이를 완벽히 이해해서 매 프로젝트 마다 깔쌈하게 분리하여 작성해보자
{% endhint %}

### 타입스크립트 설치

```bash
npm i -D typescrript
npx tsc --init
```

{% hint style="info" %}
tsconfig.json 파일의 jsx 속성은 변경하자

"jsx":"react-jsx"

-> react-jsx 모드는 jsx가 사용되는 모든 파일에서 import React 없이 사용할 수 있도록 해준다.

-> React17 이후 React는 React.createElement를 사용하지 않고 JSX를 자동으로 변환하는 기능이 추가 되었다.

(물론 React가 제공하는 다른 훅들을 사용할 땐 임포트 해야함)
{% endhint %}

### ESLint 설치

```bash
npm i -D eslint
npx eslint --init
```

**with Jest**

> Jest를 설치하기 전 미리 설정해두자.

```js
env: {
  browser: true,
  es2021: true,
  jest: true,
},
```

**.eslintignore**

> eslint를 실행할 때 제외할 파일들을 지정 가능

```
/node_modules/
/dist/
/.parcel-cache/
```

### React 설치

```bash
npm i react react-dom

// 관련 타입 설치
npm i -D @types/react @types/react-dom
```

{% hint style="info" %}
요즘 나오는 라이브러리들은 타입이 내장되어 있어 @types와 같은 모듈을 따로 설치 안해줘도 되지만 예~전에 출시된 라이브러리들은 따로 존재하기에 각각 설치해줘야 한다.
{% endhint %}

### 테스팅 도구 설치 Jest with SWC

```bash
npm i -D jest @types/jest @swc/core @swc/jest \
  jest-environment-jsdom \
  @testing-library/react @testing-library/jest-dom
```

**jest.config.js 파일을 작성해 테스트에서 SWC를 사용하자**

> 타입스크립트 + SWC 환경에서는 추가적인 설정이 필요하다.

```bash
touch jest.config.js
```

- [코드 링크](https://github.com/ahastudio/CodingLife/blob/main/20220726/react/jest.config.js)

```js
module.exports = {
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: ['@testing-library/jest-dom/extend-expect'],
  transform: {
    '^.+\\.(t|j)sx?$': [
      '@swc/jest',
      {
        jsc: {
          parser: {
            syntax: 'typescript',
            jsx: true,
            decorators: true,
          },
          transform: {
            react: {
              runtime: 'automatic',
            },
          },
        },
      },
    ],
  },
  testPathIgnorePatterns: ['<rootDir>/node_modules/', '<rootDir>/dist/'],
};
```

{% hint style="warning" %}

### SWC가 뭐지? 아~ babel의 대체제구나

-> Rust로 짜여진 컴파일러로, 현재는 JavaScript/TypeScript 트랜스파일링을 주로 담당하고 있다

속도와 성능 개선에 초점을 맞추고 있으며, 기존의 Babel 등의 트랜스파일러를 압도적으로 뛰어넘는 성능을 보여준다.
{% endhint %}

### Parcel 설치

> parcel을 통해 웹 개발 서버를 띄울 수 있다.

```bash
npm i -D parcel
```

```json
{
  "name": "react",
  "version": "1.0.0",
  "description": "",
  "source": "./index.html", // origin "main": "index.js" -> 웹서버를 띄울것이므로
  "scripts": {
    "start": "parcel --port 8080",
    "build": "parcel build", // dist 파일 생성
    "check": "tsc --noEmit", // Only 컴파일, 파일생성 X
    "lint": "eslint --fix --ext .js,.jsx,.ts,.tsx .", // 형식들 적용
    "test": "jest",
    "coverage": "jest --coverage --coverage-reporters html",
    "watch:test": "jest --watchAll"
  },
  ...
}
```

- [package.json 참고용](https://github.com/ahastudio/CodingLife/blob/main/20220726/react/package.json)

**index.html**

```html
<!DOCTYPE html>
<html>
  /.../
  <body>
    <p>Hellow world</p>
    <!-- 원래는 ESmodule로 돌아가야 하는건데 parcel이라는 번들을 사용했기 때문에  module로 사용 -> parcel이 변환해준다. -->
    <script type="module" src="./src/main.tsx"></script>
  </body>
</html>
```

### 더 공부해야 할 것들

1. [**fnm (Fast Node Manager)**](https://fnm.vercel.app/)
2. [**parcel**](https://medium.com/better-programming/all-you-need-to-know-about-parcel-dbe151b70082)
3. [**.bin**](https://simsimjae.medium.com/%ED%8C%A8%ED%82%A4%EC%A7%80-%EC%95%88%EC%97%90%EB%8A%94-bin%EC%9D%B4%EB%9D%BC%EA%B3%A0%ED%95%98%EB%8A%94-%EC%88%A8%EA%B9%80-%ED%8F%B4%EB%8D%94%EA%B0%80-%EC%A1%B4%EC%9E%AC%ED%95%9C%EB%8B%A4-%EC%9D%B4-%ED%8F%B4%EB%8D%94%EB%8A%94-%EB%AD%90%EB%95%8C%EB%A7%A4-%EC%9E%88%EB%8A%94%EA%B1%B4%EC%A7%80-%EA%B6%81%EA%B8%88%ED%95%B4%EC%84%9C-%EC%B0%BE%EC%95%84%EB%B3%B4%EC%95%98%EB%8B%A4-8257ddaa1a7e)
4. [**why use should SWC**](https://medium.com/@KasraKhosravi/why-you-should-use-swc-and-not-babel-45b9dd15d058)
5. [**An In-Depth Explanation of package.json’s Dependencies**](https://medium.com/better-programming/package-jsons-dependencies-in-depth-a1f0637a3129)

### 간단 회고 (2023.03.06 월)

글쓰는건 너무 x 100 어렵다..😭
강의를 들으면서 정리하려고 하니 시간이 너무 소요된다.
비효율적으로 공부하는 것 같은데.. 어떻게 하면 좋을것인가 좋은 방법을 찾아보자!
