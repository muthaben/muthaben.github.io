---
title: "[TIR] 데브옵스와 CI/CD"
date: 2022-02-15
update: 2021-02-05
tags:
  - AWS
  - TIR
series: TIR
---

## 데브옵스란(DevOps)?

소프트웨어는 개발부터 고객에게 배포될 때까지 개발, 테스트, 인프라 등 다양한 과정을 거치게 된다.  
다양한 과정을 거치는 만큼 회사가 조금만 커져도 자연스럽게 각 단계를 진행하는 데 **반복적이고 불필요한 작업과 오버헤드**가 생기기 마련이다.  
결과적으로 배포 주기가 길어지고 배포되는 변경 사항이 커져 배포의 위험이 증가하고 고객에게 새로운 소프트웨어가 전달되기까지 오랜 시간이 걸리게 된다.

이러한 문제를 해결하기 위해 나온 개념이 데브옵스다.  
데브옵스는 **개발(Deployment)과 운영(Operation)의 합성어**로, 개발과 운영을 하나로 합쳐서 일하는 철학, 도구, 환경, 문화 등의 조합이다.  
개발부터 배포까지 모든 단계에 **자동화와 모니터링**을 도입해서 **더 짧은 개발 주기, 더 많은 배포 빈도, 안정적인 소프트웨어를 배포**하자는 목표를 갖고 있다.

데브옵스는 서버 구성, 배포, 테스트 등의 반복적이고 단순한 작업을 최대한 자동화해서 배포에 들어가는 비용을 최대한으로 줄이는 것부터 시작된다.  
배포가 쉬워지면 작은 단위로도 배포를 여러 번 할 수 있게 되고, 이는 결국 큰 장애가 발생하기 전에 문제를 바로 발견해서 해결할 수 있고 서비스를 민첩하게 운영할 수 있는 결과를 가져온다.  
다만 이를 달성하기 위해서는 시작점인 유닛 테스트나 E2E 테스트 등의 테스트 코드를 포함한 자동화 환경이 잘 갖춰져 있어야 한다.  
데브옵스를 실천하는 방식에는 여러 가지가 있다.  
지속적 통합(CI), 지속적 전달/배포(CD), 마이크로 서비스, 커뮤니케이션 및 협업 등이 있다.

### CI(Continuous Integration)

지속적 통합은 데브옵스 소프트웨어 개발 방식 중 하나로, 개발자들이 코드를 메인 브랜치에 병합하는 시간을 최대한 앞당겨 버그를 빨리 찾을 수 있게 하는 것이다.  
하나의 프로젝트를 여러 명의 개발자와 동시에 진행해본 사람이라면 코드를 메인 브랜치에 커밋할 때 병합 시 발생하는 코드 충돌로 곤혹을 치른 경험이 있을 것이다.  
보통 이런 문제가 생기는 이유는 개발자들이 로컬 혹은 다른 브랜치에서 오랜 시간 동안 많은 양의 코드를 작성하고 배포를 위해 마지막에 메인 브랜치에 병합하기 때문이다.  
너무 많은 코드를 한 번에 병합하다 보니 병합 시간도 오래 걸릴뿐더러 버그의 위험성도 매우 높아진다.  
그리고 일찍 개발된 작은 기능들이 있어도 큰 기능들과 함께 기다렸다가 병합되기 때문에 개선점이 배포되는 시기도 자연스럽게 늦어진다.  
이러한 문제는 최신 버전의 서비스를 고객들에게 최대한 빨리 제공하자는 데브옵스의 목표에 맞지 않는다.  
이런 문제를 해결하고자 하는 것이 지속적 통합이다.

지속적 통합이 도입될 경우 **개발자들이 메인 브랜치에 코드를 커밋할 때마다 자동으로 전체 유닛 테스트를 진행**하게 된다.  
그리고 테스트가 실패한 경우에는 알림을 보내 해당 내용을 커밋한 개발자가 문제를 해결하게 된다.  
개발자들이 코드를 푸시하기 이전에 자신이 작성한 그리고 영향이 미쳤을 것이라고 예상하는 부분에 대해서는 유닛 테스트를 돌려볼 것이지만 전체 유닛 테스트는 돌리는 데 시간도 오래 걸리고, 비효율적이다.  
따라서 개발자는 우선 **메인 브랜치에 커밋을 하고 예상하지 못한 영향으로 인해 테스트 케이스가 실패한 경우에만 문제를 처리**하면 된다.

매번 전체 테스트를 실행하게 되니 개발자들은 자신이 작성한 코드가 다른 곳에 영향을 미치는 것에 대한 걱정을 덜 하고 자주 커밋할 수 있게 된다.  
그리고 작은 단위로 메인 브랜치에 커밋이 일어나다 보니 병합 시 발생하는 코드 충돌 문제에서 상당수 해방될 수 있다.  
또한 전체 테스트가 굉장히 빈번하게 돌아가기 때문에 버그도 굉장히 빨리 발견되고 처리된다.  
다만 지속적 통합을 도입하기 위해서는 **제품의 품질을 보장할 수 있다고 믿을 수 있는 자동화 테스트가 잘 작성**돼 있어야 한다.

### CD(Continuous Delivery/Deployment)

지속적 전달과 배포는 지속적 통합과 마찬가지로 데브옵스 소프트웨어 개발 방식이다.  
지속적 통합에 이어 영역을 더 확장해서 유닛 테스트까지 완료된 코드를 운영 서버에 배포하기 전 단계인 **스테이지 서버**에 배포하고, 해당 서버에서 UI 테스트, 연동 테스트 등 다양한 테스트를 자동으로 진행한다.  
해당 테스트도 모두 통과했다면 이제 운영 서버에 배포하기 위한 준비를 하는데, 이때 **수동으로 사람이 운영 서버 배포를 승인하는 것이 지속적 전달**이고 **운영 서버까지 자동으로 배포되는 것이 지속적 배포**다.

지속적 전달과 배포는 지속적 통합과 비슷하게 개발자의 생산성을 향상하고 버그를 더 빨리 발견할 수 있다는 이점을 가져온다. 더 나아가 작은 기능 하나하나도 커밋만 일어나면 운영 서버에 자동으로 배포 혹은 준비 단계까지 가기 때문에  
고객에게 최신 버전의 서비스를 최대한 빠르게 전달할 수 있다는 장점이 있다.  
하지만 지속적 통합과 마찬가지로 **자동화 테스트가 의미 있는 수준으로 잘 작성**돼 있어야 한다는 전제가 있다.

## AWS와 데브옵스

AWS는 데브옵스에 대한 여러 서비스를 제공한다.  
CodeBuild는 프로젝트 코드를 빌드하고 유닛 테스트를 실행할 수 있게 해준다.  
소스코드를 입력으로 받고 해당 코드를 빌드, 컴파일하는 단계를 대신해서 수행하고 필요한 자동화 테스트를 함께 수행한다.  
CodeDeploy는 배포 자동화를 수행한다.  
CodePipeline은 소스코드의 커밋부터 배포까지 각자 상황에 맞는 파이프라인을 구성할 수 있게 해준다.

### AWS CodePipeline

지속적 통합 및 지속적 전달을 가능하게 하는 서비스다.  
**소스코드 불러오기, 빌드, 배포, 승인 등 다양한 작업을 조합**해서 필요에 맞는 파이프라인을 구성할 수 있게 해준다.  
AWS 내부의 서비스뿐만 아니라 깃허브, 젠킨스 등 외부 서비스와의 연동을 직접 지원한다.  
예를 들어, 소스코드가 깃허브에 커밋되면 해당 코드를 읽어와 CodeBuild에서 유닛 테스트를 실행한 뒤 문제가 없다면  
CodeDeploy를 이용해 배포를 자동으로 진행할 수 있다.

CodePipeline에는 단계라는 개념이 있어서 **단계별로 어쩐 작업을 할지 지정**할 수 있다.  
예를 들어, 스테이지 단계, 운영 단계 등으로 나눠서 사용할 수 있다.  
그리고 **각 단계는 1개 이상의 작업(코드 빌드, 배포, 알림 등)**으로 이뤄진다.  
작업은 직렬로 실행될 수도 있고 병렬로 동시에 실행될 수도 있다.  
파이프라인의 특성상 직렬로 작업이 있을 때 앞의 작업이 성공적으로 끝나야만 다음 작업으로 넘어갈 수 있다.

각 작업이 끝날 때마다 **뒤의 작업에 데이터를 넘겨주기 위한 아티팩트(artifact)**라는 개념이 있다.  
아티팩트는 함수에서 파라미터와 리턴값과 같은 개념으로 **작업이 시작할 때 입력 아티팩트**라는 이름으로 파라미터처럼 넘겨줄 수 있고, **작업이 끝날 때 출력 아티팩트**라는 이름으로 뒤의 작업에게 넘겨줄 수도 있다.

### 👉 CodePipeline에서 제공되는 작업의 종류

#### 1. 승인

파이프라인의 다음 단계로 넘어가기 위해 사용자의 승인을 기다린다.  
사용자가 승인하면 파이프라인의 다음 단계로 넘어가고, 거절한다면 파이프라인은 거기서 끝나게 된다.

#### 2. 소스

소스코드를 가져온다. 모든 파이프라인은 소스 작업으로 시작해야 한다.  
AWS S3, AWS CodeCommit, 깃허브 등의 제공자와 연동할 수 있다.

#### 3. 빌드

뒤의 작업에서 사용할 수 있도록 코드를 빌드한다.  
AWS CodeBuild, 젠킨스 등의 서비스와 연동할 수 있다.

#### 4. 테스트

미리 정의된 유닛 테스트 등의 자동화 테스트 코드로 테스트를 진행한다.  
AWS CodeBuild, 젠킨스, 고스트 인스펙터(Ghost Inspector) 등의 서비스와 연동할 수 있다.

#### 5. 배포

코드를 서버에 배포한다.  
AWS CodeDeploy, AWS Elastic Beanstalk, ECS, CloudFormation 등의 서비스와 연동할 수 있다.

#### 6. 호출

AWS Lambda를 호출해서 임의의 코드를 실행할 수 있다.  
코드를 실행할 수 있기 때문에 이 작업에서 사실상 필요한 모든 작업을 진행할 수 있다.  
예를 들어, 외부 서비스에 알림, 깃허브 API를 호출해서 버전을 기록하는 등의 작업을 할 수 있다.

---

<출처>

- 김담형, AWS 인프라 구축 가이드, 위키북스(2019)
