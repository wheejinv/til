**Table Of Contents**

- [Pipeline Syntax](#pipeline-syntax)
    - [Sections](#sections)
        - [Agent](#agent)
        - [Post](#post)
        - [Stages](#stages)
        - [Steps](#steps)
    - [Declarative](#declarative)
        - [Environment](#environment)
        - [Parameter](#parameter)
        - [Triggers](#triggers)
        - [When](#when)

# Pipeline Syntax

참고: [Jenkins Pipeline Syntax Docs](https://www.jenkins.io/doc/book/pipeline/syntax/#declarative-pipeline)

## Sections

한개 이상의 `Directives` 나 `Steps` 를 포함 하는 것.

### Agent

젠킨스는 많은 일들을 해야 하기 때문에 혼자 하기 버겁다.

여러 slave node를 두고 일을 시킬 수 있고,
젠킨스 노드 관리에서 새로 노드를 띄우거나 혹은 docker 이미지등을 통해 처리할 수 있다.

### Post

스테이지가 끝난 이후의 결과에 따라서 후속 조치를 취할 수 있다.

성공시에 성공 이메일, 실패하면 중단 혹은 건너뛰기

### Stages

어떤 일들을 처리할 건지 일련의 stage 를 정의함.

### Steps

- Steps 내부는 여러가지 스텝들로 구성
- 여러 작업들을 실행가능
- 플러그인을 깔면 사용할 수 있는 스텝들이 생겨남

아래 코드는 stages, steps, post 를 활용한 기본 문법이라고 보면 될 것 같다.

```groovy
stages {
    stage('Prepare') {
        steps {
            // ...
        }

        post {
            success {
                echo 'success'
            }
        }
    }

    stage("Build") {
        steps {
            // ...
        }
    }
}
```

## Declarative

`environment`, `options`, `parameters`, `triggers`, `Jenkins cron syntax`,
`stage`, `tools`, `input`, `when` 등이 있음.

### Environment

어떤 pipeline 이나 stage scope의 환경 변수 설정

### Parameter

파이프라인 실행 시 파라미터 받음.

### Triggers

어떤 형태로 트리거 되는가

### When

언제 실행되는가.

아래 코드의 경우 production 인 경우에만 해당 스테이지를 실행하겠다는 의미.

```groovy
stage('Only for production') {
    when {
        branch 'production'
        enviroment name: 'APP_ENV', value: 'prod'
        anyOf {
            enviroment name: 'DEPLOY_TO', value: 'production'
            enviroment name: 'DEPLOY_TO', value: 'staging'

        }
    }
}
```
