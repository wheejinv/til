**Table Of Contents**

- [NODE_ENV](#node_env)
    - [NODE_ENV 설정 방법](#node_env-설정-방법)
    - [다른 환경 변수 설정](#다른-환경-변수-설정)


# NODE_ENV

먼저 알아둬야 할 것은 [node 환경 변수 페이지](https://nodejs.org/dist/latest-v16.x/docs/api/cli.html#environment-variables) 에서는 `NODE_ENV`에 대해서 언급하지 않고 있다.

`NODE_ENV` 에 관한 언급은 [webpack 페이지](https://webpack.js.org/guides/production/#specify-the-mode) 에서 찾아볼 수 있다.

아래는 번역기를 돌린 내용이다.

```text
많은 라이브러리는 라이브러리에 무엇이 포함되어야 하는지를 결정하기 위해 process.env.NODE_ENV 변수를 키오프합니다.
예를 들어, process.env.NODE_ENV가 'production'으로 설정되지 않은 경우 일부 라이브러리는 디버깅을 더 쉽게 하기 위해 추가 로깅 및 테스트를 추가할 수 있습니다.
그러나 process.env.NODE_ENV를 '프로덕션'으로 설정하면 실제 사용자를 위해 실행되는 방식을 최적화하기 위해 상당한 부분의 코드를 삭제하거나 추가할 수 있습니다.
```

즉 `NODE_ENV` 설정은 많은 라이브러리들이 해당 값을 보고 있기 때문에 production 으로 설정해주면 성능 상 이득이 있다는 정도로 알고 넘어가면 될 듯 하다.

## NODE_ENV 설정 방법

[NODE_ENV 관련 스택 오버 플로우 페이지](https://stackoverflow.com/questions/9198310/how-to-set-node-env-to-production-development-in-os-x) 를 참고하자.

노드 실행 시 NODE_ENV 설정은 다음과 같이 실행한다.

```shell
NODE_ENV=production node app.js
```

노드 실행 환경에서 NODE_ENV 접근은 다음과 같다.

```javascript
console.log(process.env.NODE_ENV);
// production

```

## 다른 환경 변수 설정

process.env 에 삽입하기 위해서 마찬가지 방법을 사용하면 된다.

참고로 [dotenv](https://www.npmjs.com/package/dotenv) 모듈도 살펴보면 좋을 것 같다.

해당 모듈은 `.env` 파일로 환경 변수를 관리하며 각기 다른 환경에 대해 다른 .env 파일을 매핑하여 사용할 수 있다.

`.env` 파일은 패스워드 등의 민감한 정보를 보관하는 경우도 있으므로 `.gitignore` 에 등록해주는 것도 까먹지 말아야 한다.

```shell
API_KEY=abc DB_PASSWORD=1234 node
```

확인은 다음과 같이 한다.

`DB_PASSWORD`의 타입도 `'`(싱글 코테이션)으로 감싸져 있는걸로 보아 string 형인듯하며 주의가 필요한듯 하다.

```shell
Welcome to Node.js v16.13.2.
Type ".help" for more information.
> process.env.API_KEY
'abc'
> process.env.DB_PASSWORD
'1234'
```
