# 구성요소 관련 용어

## 마스터노드

### kubectl
- 클러스터에 명령 내리는 역할
- 주로 API 서버와 통신
- 파드 생성, 상태 관리, 복구 등에 매우 중요한 역할

### API 서버
- 클러스터의 중심 통로
- 주로 etcd와 통신
- 다른 구성요소도 이 API 서버를 중심으로 통신함

- status가 Spec과 다를 경우
	- 감시 -> 차이 감지 -> 상태 변경 -> 변경 완료 후 감시
	- 파드가 죽었을 경우 -> 복구시킴 (self healing)

### etcd
- 구성요소 상태값이 저장되는 곳
- key-value 저장소
	- 분산 저장 가능

### 컨트롤러 매니저
- 클러스터 오브젝트 관리 담당

### 스케쥴러
- 어떤 노드에 파드 띄울지, 파드 할당 일정 관리
	- 노드 상태, 자원, 레이블, 요구 조건 등을 고려 함
	
### cordon 기능
- 특정 노드에 scheduling 못하도록 막음
	- 기존에 해당 pod에 배포된건 상관 X
	- 새로 replicas 늘리는 등 pod 추가적으로 배포할 때 해당 노드에 배포되지 못하도록 함
	- 노드의 자원 보호등이 목적

	
## 워커노드 관련

### kubelet
- Pod Spec (파드 구성 내용)을 받아서 CRI로 전달
	- kubernetes에서 자체적으로 제공되는 CNI 플러그인, but 기능 매우 제한적
- 파드 안의 컨테이너 정상 구동하는지 모니터링 하는 역할

### 파드
- 단일 목적을 위해 구성된 한개 이상의 컨테이너
- 애플리케이션의 최소 단위
- 언제라도 죽을 수 있도록 설계됨
- K8S => 효과적인 파드 제공이 목적



## 선택 가능 요소

### 네트워크 플러그인
- 클러스터가 통신 위해 필요한 요소
- 일반적으로 네트워크 플러그인은 CNI로 구성함
- CNI 종류 : Calico, Flannel, Cilium, Kube-router, Rommana, WaeveNEt, Canal 등

### CoreDNS
- 클러스터 구성, 접근시 사용하는 도메인네임 관리하는 서버
- 도메인 네임을 관리



## 기타

### CNI (컨테이너 네트워크 인터페이스)
- 컨테이너 간 네트워킹을 제어할 수 있는 "플러그인"을 만들기 위한 표준
- 멀티 호스트로 구성된 컨테이너간 통신 위해서는 CNI가 반드시 설치 되어 있어야 함


- 쿠버네티스에서는 POD간 통신을 위해 CNI를 사용
- 쿠버네티스는 자체적으로 

- Flannel
	- L2 네트워크
- Calico
	- L3 네트워크
	

## Pod 생명주기
1) kubectl => API 서버에 파드 생성 요청

2) API 서버 => etcd에 모든 전달된 내용 기록
	- API 서버에 (각 요소에 대해) update된 내용이 전달될 때마다 이행
	- 아래 과정들 거치면서 계속 API 서버가 감시함, 그 내용들이 지속적으로 etcd에 반영되는 것
	- 그러면서 클러스터의 상태값을 최신으로 유지함
	
	- API 서버는 계속 일련의 과정들이 진행되는 과정마다 요청, 내용 확인을 진행함
		- 파드 생성 요청들어옴
		- 컨트롤러 매니저에서 진행한 Pod 생성 요청, 완료된 내용 감시
		- 스케쥴러에서 진행한 노드 배정 요청, 완료된 내용 감시
		- 정상적으로 파드 동작하는지 감시

3) 컨트롤러 매니저
	=> API 서버에 전달된 파드 생성 요청을 인지
	=> Pod를 생성 (컨트롤러 매니저가 API 서버에)
	=> 그리고 이 상태를 API 서버에 전달
	- API 서버에 Pod가 생성만 된 상태, 특정 Node에 아직 할당 X

4) 스케쥴러 
	=> API 서버에 Pod 생성된 것 인지
	=> 워커노드별 조건 고려
	=> 워커노드에 Pod 띄우도록 요청 (즉, 특정 노드에 Pod 속하도록)

5) 스케쥴러
	=> Kubelet 통해 Pod가 제대로 떴는지 확인
	- API 서버에 전달된 정보대로 해당 노드의 Pod가 떴는지 확인

6) Kubelet => 컨테이너 런타임으로 파드 생성을 요청

7) Pod 생성됨
	- 정확히는 Pod 자체는 3단계 때 생성, 4단계 때 특정 노드에 속하는 것 완료, 5단계 때 해당 과정 완료됬는지 확인 완료

8) Pod 사용 가능한 상태




## K8S 핵심
- 선언적인 시스템 구조
	- 추구하는 상태 선언하면 -> 현재 상태와 맞는지 점검하고 -> 그것에 맞추려 노력하는 형태가 K8S의 철학
	- 즉, "상태변경 -> 감시 -> 차이발견 -> 상태발견"의 순환

- 순차적 워크 플로우가 아님 X
	- 즉, 일련의 작업 기술하고, 그것들을 이행하기만 하면 되는 것 X
	
- 이런 선언적인 구조 때문에 etcd 필요
	- API 서버는 현재 상태를 가지고 있고, 이를 보존하기 위해 etcd 필요
	


## 오브젝트
- API 서버, 컨트롤러 매니저
	=> 실제로 Pod 생성 등을 감시한다는 개념 X
	=> Object의 생성을 감시하는 개념

### Object Spec
- Object 상세 설정
- YAML 문법으로 작성함

### Deployment
- K8s는 Deployment라는 관리 그룹 아래에 Pod가 생성됨
	- run => 단일 파드 1개 생성
	- create deployment => deployment 아래에서 pod들 생성
	
- 즉, Deployment = Pod + ReplicaSet
	- Deployment => ReplicaSet의 상위 object
	- Pod, ReplicaSet을 (간접적으로) 생성하는데 사용됨
	
- Deployment를 사용하면 Pod, ReplicaSet를 직접 생성할 필요가 없음

- 주요 배포 전략
	- Recreate
		- 
	- Rolling Update
	

### Self-Healing (셀프 힐링)
- 제대로 동작하지 않는 컨테이너를 다시 시작하거나, 교체하는 것
	=> 제대로 돌아가도록 "바라는 상태"에 가깝게
	
	
### ReplicaSet
- 파드의 수를 보장해주는 오브젝트
- 그리고 Pod를 복구시켜주는 대표적 Resource
	- Deployment 때문에 단독으로 사용하는 경우는 적음
- 단순히 Pod를 3개 만드는 것보다 Replica 활용하는 것이 관리에 용이
	- Pod 3개 vs Pod - Replica 3개
	- Replication 컨트롤러는 ReplicaSet의 하위 호환

## Controller
- Pod를 관리하는 역할을 함
- 각 컨트롤러는 특정한 목적을 위해 만들어짐
- 종류
	- 상태 유지하지 않아도 되는 파드 관리하는 경우
		- Replication Controller
			- Pod 죽었을 때 복구와 관련 있음
			- 현재는 거의 사용X(ReplicaSet의 하위 버전)
		- ReplicaSet
			- 구버전 : Replication Controller
		- Deployment
	- 클러스터 전체에 배포가 필요한 파드를 관리하는 경우
		- Daemon Set
	- 상태관리가 필요한 파드를 관리하는 경우 
		- StatuefulSet
	- 배치성 작업을 진행하는 파드를 관리하는 경우
		- CronJob



## Lable & Selctor
### Label
- key /value 쌍의 meta 정보
- 쿠버네티스 리소스를 논리적 그룹으로 나누기 위한 tag

### Selctor
- Label을 이용하여 원하는 resource 필터링하는 방법
- label query



### 참고 자료
- 책 : 컨테이너 인프라 환경 구축을 위한 쿠버네티스/도커
- 네트워크 플러그인 : https://kubernetes.io/ko/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/
- Deployment 1 : https://kingofbackend.tistory.com/165
- Deployment 2 : https://willseungh0.tistory.com/62
- Controller : https://real-dongsoo7.tistory.com/134
- ReplicaSet : https://frozenpond.tistory.com/103
- Label & Selector : https://velog.io/@ghdud0503/Kubernetes-%EA%B8%B0%EC%B4%88-6-Lable-Selector
