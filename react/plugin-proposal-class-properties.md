**Table Of Contents**
- [개요](#개요)
- [참고](#참고)
    - [proposal-class-properties 를 사용한 문법](#proposal-class-properties-를-사용한-문법)
    - [proposal-class-properties 를 사용하지 않는 문법](#proposal-class-properties-를-사용하지-않는-문법)
- [단점](#단점)
- [적용](#적용)
- [삽질 과정](#삽질-과정)
    - [일단 설치](#일단-설치)
    - [플러그인 빼고 했는데도 빌드 에러가 나질 않네?](#플러그인-빼고-했는데도-빌드-에러가-나질-않네)
    - [왜 잘되지?](#왜-잘되지)


# 개요

`리액트를 다루는 기술`이란 책으로 공부하다가 알게된 플러그인이다.

초판 102페이지를 보게 되면 `transform-class-properties 문법`을 사용한 예제가 소개 되어 있다.

클래스에 화살표 함수 형태로 메서드 정의를 가능하게 해준다.

이 문법은 이벤트 핸들러에 많이 쓰이게 되는데 콜백 함수 내에서 `this.state`에 접근할 일이 매우 많기 때문이다.

# 참고
- [@babel/plugin-proposal-class-properties](https://babeljs.io/docs/en/babel-plugin-proposal-class-properties)
- [preset-env 에 shippedProposals 항목](https://babel.dev/docs/en/babel-preset-env#shippedproposals)

## proposal-class-properties 를 사용한 문법
```javascript
class Component {
    handleClick = () => {
    	console.log(this); // this는 자동으로 바인딩 되어 Component 인스턴스를 가리킨다.
    };
}
```
## proposal-class-properties 를 사용하지 않는 문법
```javascript
class Component {
	constructor() {
    	this.handleClick = this.handleClick.bind(this);
        // 2. this바인딩을 매번 해줘야 해서 귀찮다..
    }

    // 1. function으로 선언하는 경우
    handleClick() {
    }
}
```

# 단점
[Class에서 arrow function을 사용하지 말아야하는 이유](https://simsimjae.tistory.com/452) 에 따르면 3가지 단점이 있다고 한다.
- 이슈 1. method override 문제
- 이슈2. Prototype 문제 (Performance 저하)
- 이슈3. 상속 문제

# 적용

`preset-env`는 compiling ES2015+ syntax 를 위한 것으로 리액트 프로젝트 셋팅 시 `@babel/preset-react` 과 더불어 필수로 셋팅된다.

2020년 5월에 `preset-env`에 기본 포함되었다.
- [Add class proposals to shipped proposals #11451](https://github.com/babel/babel/pull/11451)

# 삽질 과정

## 일단 설치

인터넷 검색으로 `@babel/plugin-proposal-class-properties` 설치 하는 방법을 검색함.

```shell
npm install --save-dev @babel/plugin-proposal-class-properties
```

`.babelrc` 파일에 플러그인으로 추가
```json
{
	"presets": [
		"@babel/preset-env",
		"@babel/preset-react"
	],
	"plugins": [
		"@babel/plugin-proposal-class-properties"
	]
}
```

## 플러그인 빼고 했는데도 빌드 에러가 나질 않네?

`.babelrc` 파일에 플러그인 지우고 테스트 해봤는데 잘 됨.
```json
{
	"presets": [
		"@babel/preset-env",
		"@babel/preset-react"
	]
}
```

## 왜 잘되지?

왜 2018년에 소개된 책에서는 plugin 설치를 하라고 했는데 왜 그냥 잘될까 하다가 찾은 이슈는 아래와 같다.
(대충 기본 스펙에 포함되었는데 왜 안되는건가요 라는 분위기의 이슈)
- [@babel/preset-env not including classProperties #11796](https://github.com/babel/babel/issues/11796)

해서 `@babel/preset-env` 에 기본 포함되어 있다는 걸 알게 되었다.

사실 [@babel/plugin-proposal-class-properties](https://babeljs.io/docs/en/babel-plugin-proposal-class-properties) 문서 제일 위에도 안내가 된 부분이었는데 잘 안읽어서 고생했다.

> NOTE: This plugin is included in @babel/preset-env, in ES2022
