# CI/CD

애플리케이션 개발 단계부터 배포 때까지 이 모든 단계를 자동화를 통해서 조금 더 효율적이고,
빠르게 사용자들에게 빈번히 배포할 수 있게 하는 것을 말한다.

- `CI`: Continuous Integration(지속적인 통합)
  - 버그 수정이거나 새로 만드는 기능들이 메인 repository 에 주기적으로 빌드되고 테스트되어 머지되는 것을 말함.
  - CI를 사용했을 때 이점은 다음이 있을 수 있다.
    - 주기적으로 머지를 하기 때문에 충돌을 피할 수 있어서 개발 생산성을 높일 수 있다.
    - 모든 코드들은 자동으로 빌드되고 테스트되기 때문에 코드의 결함이나 문제점이 빠르게 발견 될 수 있다.
    - CI를 사용한다면 소스 코드들이 자동으로 테스트될 수 있도록 만들기 때문에 조금 더 나은 코드 퀄리티를 가질 수 있다.
- `CD`: Continuous Delivery(지속적인 제공), Continuous Deployment(지속적인 배포)
  - 두 가지 뜻을 구분해서 사용하지 않고 보통 같은 것으로 바라보며, 회사마다 많은 차이가 있음.
  - Continuous Delivery는 릴리즈 단계에서는 수동으로 배포하는 것을 말함.
  - Continuous Deployment 는 배포까지 자동화하는 것이라고 볼 수 있다.

# Jenkins, 젠킨스

젠킨스는 다른 일상적인 개발 작업을 자동화할 뿐 아니라 파이프파인(Pipeline)을 사용해 거의 모든 언어의 조합과
소스코드 리포지토리에 대한 CI/CD 환경을 구축하기 위한 간단한 방법을 제공한다.

## 설치

도커로 설치를 진행하였으며 이미지는 lts 버전을 사용한다. 도커 이미지 태그는 `jenkins/jenkins:lts` 이다.

젠킨스의 기본 포트는 8080 이다. 도커를 사용하기 때문에 기본 포트를 바꾸기 위해 따로 알아본 것은 없다.

설치된 컨테이너의 홈 디렉토리는 `/var/jenkins_home` 이다.

아이템을 하나 생성했을 때의(ex. `sample_test`) 경로는 `/var/jenkins_home/workspace/sample_test` 이다.

## 플러그인

젠킨스에서 사용하는 플러그인은 정말 많다.

[GitHub Jenkins Plugin](https://plugins.jenkins.io/github/): commit 이 push 되었을 때 자동으로 빌드되게 하려면 GitHub hook 을 사용해야 한다.

[Slack Notification](https://plugins.jenkins.io/slack/): 빌드 결과 들을 슬랙으로 알리고 싶은 경우 사용해야 하는 플러그인.

