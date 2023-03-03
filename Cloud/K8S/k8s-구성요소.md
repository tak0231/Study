# 마스터 구성요소 (=마스터노드 = Control-Plane)
- 클러스터 관리를 위해 필수적 요소

## etcd
- 고가용성을 갖춘 Key-Value 저장소
  - **고가용성**
    - 복원력을 갖추고 신뢰할 수 있음
    - 다운타임 없이 거의 100% 상시 액세스 가능하며, 제대로 작동하는 시스템
    
- 쿠버네티스에 필요한 모든 데이터를 저장함
  - 일종의 DB
 
- 쿠버네티스를 오픈소스 화 하는 과정에서 사용된 요소
  - 이전에는 구글 내부에서 사용하던 보그(borg : 구글 내부 컨테이너 오케스트레이션 툴)에서 처비(Chubby)라는 분산 저장 솔루션을 사용했음

- 일반적으로 etcd 자체를 클러스터링을 구성하여 띄움
  - 데이터의 안정성을 위해 (한쪽에서 죽어도 복구 가능)
  - 물론 프로세스 1개만 띄워서 etcd를 사용 가능함
  - 가급적이면 주기적으로 백업해두는 것이 좋음

## kube-apiServer
- 쿠버네티스 = MSA(Micro Service Architecture) 구조
  - 여러개의 분리된 프로세스로 구성됨

- kube-apiServer => 쿠버네티스 클러스터의 API를 사용할 수 있게 해주는 프로세스
  - 쿠버네티스에 대한 모든 요청은 kube-apiServer를 거침
    - 클러스터로부터 요청(Request) 
    - -> kube-apiServer : 유효한지 검증
    - -> 호출한 프로세스로 요청 전달

- kube-apiServer => 수평적 확장 가능
  - 여러 장비에 kube-apiServer를 어러개 띄워서 사용 가능함
  - 정확한 내용 검색 필요

## kube-scheduler
- 클러스터 내에서 자우너 할당이 가능한 노드 중 적절한 노드를 선택하고 Pod를 띄우는 역할을 함
- 또한, Pod를 띄울 때 지정한 조건에 맞는 적절한 노드를 찾아주는 역할도 수행함
  - 필요한 하드웨어 요구사항 (CPU, Memory)
  - Affinity, Anti-Affinity 
  - 특정 데이터가 존재하는 노드에 할당 등

## kube-controller-manager
- k8s -> Controller가 각 Pod를 관리하는 구조
- kube-Contoroller-manager => 이 컨트롤러를 실행하는 역할을 함

- 각 컨트롤러는 논리적으로 개별 프로세스
  - but 하나의 바이너리 파일로 컴파일 되어 있고, 단일 프로세스로 실행됨
  - 이 바이너리 파일의 구조체를 생성하여 관리하는 방식으로 논리적으로 프로세스를 분리함

- 새로운 컨트롤러가 사용될 때
  - 먼저 해당 컨트롤러에 해당되는 구조체가 생성됨
  - => 그리고 kube-controller-manager가 관리하는 큐에 넣어서 실행하는 방식으로 동작

=> 정확한 내용은 추가 검색 필요

## cloud-controller-manager
- 클라우드 서비스를 제공해주는 곳들(AWS, GCP, Azure 등...)에서 쿠버네티스 컨트롤러들을 자신들의 서비스와 연계하기 위해 사용하는 컴포넌트

- 아래의 4가지 컨트롤러가 관련있는 컨트롤러

### 노드 컨트롤러

### 라우트 컨트롤러

### 서비스 컨트롤러

### 볼륨 컨트롤러


# 노드 구성요소 (= 워커노드)
- 모든 노드에서 실행되는 구성요소
- 각 노드에서 Pod가 실행되는 것을 관리하고, 쿠버네티스 실행환경을 관리하는 역할을 하는 요소들

## kuelet

## kube-proxy

## 컨테이너 런타임(Container Runtime)



# 애드온 (Addon) 구성요소
- 필수는 아니지만 추가로 사용 가능한 쿠버네티스 구성요소

### 참고자료
- 쿠버네티스 구성요소(component) : https://arisu1000.tistory.com/27828
- 쿠버네티스 아키텍쳐,  : https://imjeongwoo.tistory.com/129?category=886926
- 고가용성 : https://www.redhat.com/ko/topics/linux/what-is-high-availability
