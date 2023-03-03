## 개요
- K8S => 다수의 컨테이너화된 프로세스 관리 위한 툴
  - 컨테이너 방식 -> Host의 OS를 사용하는 방식 (Type 2)
  - 컨테이너화 된 애플리케이션을 탄력적으로 실행하기 위한 프레임워크
  - 로드밸런싱. 시크릿 관리 등을 도움
  - 문제시 유연한 대처 가능 (롤백)

- 사용자 -> 서버 직접 제어 X
  => K8S라는 "추상화된 레이어"를 이용하여 클러스터를 제어함
    - K8S의 Pod는 다른 Pod와 통신할 떄 service라는 추상화된 layer를 통해서 통신함
  
- K8S 서버
  - 모든 서버 -> Master, Worker 노드로 구분

### 로드 밸런싱
- 컴퓨터 네트워크 기술의 일종
- 둘, 셋 이상의 중앙처리장치, 저장장치 등과 같은 컴퓨팅 자원들에게 작업을 나누는 것을 의미함

## 컨테이너 오케스트레이션
- 복잡한 컨테이너 환경 관리 위한 툴
- 특징
  - Cluster (클러스터)
  - State (상태 관리)
  - Scheduling (배포 관리)
  - Rollout/Rollback (버전관리)
  - Service Discovery (서비스 등록 및 조회)
    - 서비스 등록, 조회 절차를 중앙관리화 함
  - Volume (볼륨 스토리지) 
    - 각 노드에 해당하는 스토리지 연결을 중앙에서 관리, 유지보수

### 클러스터
- 컨테이너 형태의 애플리케이션을 호스팅하는 물리/가상환경의 노드들로 이루어진 집합
  -  K8S에서는 호스트 환경에 구성된 자원들을 클러스터 단위로 추상화 하여 관리함
  -  클러스터 내부에 "마스터 노드"를 두어 내부 요소들을 제어함 (컨트롤 플레인)
- K8S에서 가장 큰 단위
- 가상 서버들이 속한 "클라우드"를 의미
- 즉, "클러스터 = 마스터노드 + 워커노드" 쯤으로 생각하면 됨

- 클러스터 -> 물리 서버 or 가상 서버의 집합
  - 여기서 말하는 가상서버가 node
  - K8S는 호스트 환경에 구성된 자원들을 "클러스터"라는 단위로 "추상화" 해서 관리함

### 노드
- 클러스터 내 가상 서버
- 즉, 컴퓨팅 엔진 단위
- **마스터 노드**
  - 컨트롤 플레인을 관장 (전체 쿠버네티스 시스템 관리, 통제)
  - 마스터 노드 사망시 -> 관리할 노드가 사라짐 => 떄문에 보통 마스터 노드는 3개정도 띄워 관리함
- **워커 노드**
  - 배포하고자 하는 어플리케이션의 실제 실행을 수행하는 노드

### 파드
- 쿠버네티스에서 생성, 관리, 배포 가능한 가장 작은 컴퓨팅 단위
- 하나 이상의 컨테이너 그룹
  - ex) 어떤 웹사이트를 띄우는 Pod -> Tomcat 컨테이너, mariaDB 컨테이너 등... 여러 컨테이너를 띄울 수 있음

### 워크로드
- 쿠버네티스에서 구동되는 어플리케이션
  - 일련의 pod 집합
  - 특정 어플리케이션(ex.jar)를 실행할 때 필요한 IT 리소스의 집합

## Desired State
- 바라는 상태
- K8S 핵심 원리
  - 사용자가 사전에 정의해둔 상태(=바라는 상태)를 유지하기 위해 현재 상태에서 정해진 특정 작업을 수행함

## Controller
- 현재 State -> Desired Sate로 변경하는 주체
- Pod를 관리하는 역할
- contorl-loop
  - 이 루프가 돌면서 지속적으로 특정 리소스를 모니터링
 
- 하나의 K8S 리소스는 하나의 컨트롤러를 가짐

- 상태를 유지하지 않아도 되는 pod를 관리하는 경우 (Statless)
  - 상태 유지하지 않아도 되는 파드 (언제든 죽을 수 있음 -> 파드 죽으면 데이터 날아감)
  - Replication Controller
  - Replica Set
  - Deployment

- 클러스터 전체에 배포가 필요한 파드를 관리하는 경우
  - Deamon Set

- 상태관리가 필요한 파드를 관리하는 경우 (Stateful)
  - Stateful Set

- 배치성 작업을 진행하는 파드를 관리하는 경우
  - Cronjob

### Replication Controller
- 실행할 Pod 갯수 지정 가능, 지정한 수만큼만 유지되도록 관리함
- 초기부터 존재하던 컨트롤러, 최근에는 비슷한 역할 하는 다른 컨트롤러들을 많이 사용함

### Replica Set
- Replication Controller와 유사
- 집합 기반의 Selector 지원
  - in, notin, exists 등의 연산 제공
  - 조건에 따라 필요로 하는 lable을 선택 가능
- Pod Update 시 rolling-update 가능

### Deployment
- 상태가 없는(stateless) 앱을 배포할 때 사용하는 가장 기본적인 컨트롤러
- ReplicaSet을 관리하면서 배포에 대한 자세한 작업도 가능
- 배포 방식, 버전 롤백 등의 설정도 가능

### Daemon Set
- 클러스터 전체 노드에 특정 파드 배포시 사용
- 데몬셋이 대상으로 삼던 노드가 클러스터에서 제거되면 -> 다른 노드로 이동해서 Pod를 실행시키지 않고 제거시킴

### Stateful Set
- 컨테이너가 종료되더라도 데이터가 남는 파드관리시 사용
  - 파드 재시작시 데이터 보존 가능

### Job
- 배치성 작업 위해 사용하는 컨트롤러
- 실행 후 종료해야하는 작업 실행시 사용되는 컨트롤러

### Cronjob
- 시간 기준으로 관리, 진행하는 Job

## K8S resource
- K8S에서 사용하느 모든 것
- Pod / ReplicaSet / Deployment / Service / Ingress / PVC 등...
- 상위 / 하위 개념의 리소스도 존재
  - ex. Deployment > ReplicaSet 

## 선언형 커맨드 (Declarative Command)
- K8S -> 직접 시스템의 상태 바꾸지 않음
  - Desired State를 선언적으로 기술 -> 이 기술을 바탕으로 커맨드 방향을 지향함
- Yaml 
  - K8S 리소스를 정의할 때  정의서 형식
  
### 명령형 커맨드 vs 선언형 커맨드
- 명령형 : How(어떻게)를 기술
  - ex) SQL 쿼리 (어떻게 테이브르이 데이터를 질의할지?) 
- 선언형 : What(무엇)을 해야할지를 기술
  - ex) HTML 문서 => 무엇을 해야하는지 기술

## namespace
- K8S 리소스 구분하는 개념
### namespace 레벨 리소스
- 특정 namespace 안에 속하여 존재하는 리소스
- Pod / ReplicaSet / Deployment / Service / Ingress 등...
- 대부분의 K8S 리소스
  - ``` >> kubectl *** -n 네임스페이스명 ```
  - 이렇게 사용하거나 기술해야하는 리소스들

### 클러스터 레벨 리소스
- namespace 영역 상관없이 클러스터 레벨에 존재하는 리소스
- node / PV / Storage Class

### K8S 기본 네임스페이스
- **default**
  -  기본 네임 스페이스
  -  아무 옵션 안주면 default로 지정됨
- **kube-system**
  - K8S 핵심 컴포넌트가 존재하는 namespace
  - 네트워크 설정 / DNS 서버 등의 컴포넌트 존재
- **kube-public**
  - 외부로 공개 가능한 리소스 담고 있는 namespace
- **kube-node-lease**
  - node가 살아있는지 master에게 알리는 용도로 존재하는 namespace

## Label & Selector
- K8S 리소스 태깅 및 타겟팅을 가능하게 해주는 라벨링 시스템
- 이를 이용한 메커니즘 덕에 K8S 리소스간 관계 느슨한 연결 유지 가능
  - => 필요한 경우 특정 리소스에 명령 전달, 정보 확인 등이 가능해짐

### Label
- K8S 리소스에 존재하는 Key-value 형식의 태그 정보

### Selector
- 태깅한 Label 찾기 위한 것

## 설정 관리
- K8S에는 필요한 설정값, Credential를 플랫폼 레벨에서 관리하도록 지원하는 메커니즘이 존재

### ConfigMap
- 필요한 설정값, 환경변수
- Pod 내 컨테이너가 사용할 구성을 저장할 수 있는 오브젝트

### Secret
- Credential
- 구성 정보 중, 민감한(Credential) 정보를 저장하기 위해 사용되는 오브젝트


# Node
## Master
- K8S 클러스터 구성하는 핵심 컴포넌트들이 존재
- 단일 서버 or 여러 서버로 클러스터 마스터 구성 가능



### 참고
- K8S 개념 : https://velog.io/@neity16/%ED%95%B5%EC%8B%AC%EB%A7%8C-%EC%BD%95-%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-2-k8s-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90
- 클러스터 / 노드 / 파드 : https://eng-sohee.tistory.com/129
- 워크로드1 : https://kubernetes.io/ko/docs/concepts/workloads/
- 워크로드2 : https://liveloper-jay.tistory.com/101
- 클러스터 : https://seongjin.me/kubernetes-cluster-components/
- 컨트롤러 : https://real-dongsoo7.tistory.com/134
- ConfigMap & Secret : https://waspro.tistory.com/681
