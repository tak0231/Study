# Service
- 어플리케이션 (pod 집합)을 외부로 노출하기 위한 추상화 방법, 또는 그러한 기능을 하는 오브젝트
	- Pod 영구적 x -> 언제든 죽었다 살아님 => 이때 Pod IP 변함 (동적)
	- 동적인 Pod Ip 대신 접근할 수단으로 사용되는 것이 서비스
- 서비스 역할 
	- Pod 고유한 IP 주소, 단일 DNS를 부여
	- Replica들에 알아서 로드밸런싱 수행해줌
		- ex) 백엔드 서버는 고가용성을 위해 레플리카를 사용함
	- 서비스를 통해서 접근하면 등록한 내부 DNS를 통해 통신함



# type : Cluster IP
- 클러스터 내의 다른 Pod들에 접근할 수 있는 (내부) IP를 할당하는 서비스
	- 자동으로 지정됨
- 클러스터 외부에서 접근 불가

# type : NodePort
- Node의 port를 고정하는 방식
- 클러스터 내 모든노드에 외부서 접근 가능한 포트를 개방하는 것
	- 모든 노드에 동일한 "특정 포트" 개방 하는 방식
	- 그리고 해당 포트로 오는 요청을 모두 nodePort Service가 처리함

- nodePort -> port -> targetPort 순

- 클러스터 IP 서비스를 자동으로 생성함
- 클러스터 외부서 서비스 접근 가능
	- [Cluster 내부의 임의의 노드의 IP] : Node Port

# type : LoadBalancer
- 외부서 접근 가능한 LB의 공인 IP를 서비스 객체에 할당하는 방식
	- LB : 로드밸런스 장비, 로드밸런서

- 일반적으로 물리장비 필요
	- but 클라우드 환경에서는 벤더사가 제공하는 로드밸런서 사용하면 됨
	
- Traffic LoadBalncer -> NodePort 통해 로드밸런싱 수행 -> 각각 분산

- Service 당 로드밸런서 1개가 필요함
	- 서비스 개수만큼 LoadBalancer(장비)와 각각의 자체 IP를 갖게 되므로 돈이 많이 듦

# 외부 접속
- k8s는 외부에 서비스 노출하는 방법 3가지 존재
- NodePort, LoadBalancer는 OSI Layer 4(TCP/UDP)에서 동작

## Ingress
- {Object: Service}를 묶는 "상위 객체"

- OSI Layer 7 (HTTP/HTTPS)에서 동작

- LB 단점 : 서비스별 LB + IP 관리 필요


# 로드밸런싱

## OSI 7

## L4 vs L7

## MatalLB



## 참고자료
- k8s 서비스 1 : https://velog.io/@hoonki/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-%EC%84%9C%EB%B9%84%EC%8A%A4ClusterIP-NodePort-LoadBalancer%EC%99%80-%EC%9D%B8%EA%B7%B8%EB%A0%88%EC%8A%A4
- k8s 서비스 2 : https://5equal0.tistory.com/entry/Kubernetes-%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-%EC%84%9C%EB%B9%84%EC%8A%A4Service%EC%99%80-%EC%9D%B8%EA%B7%B8%EB%A0%88%EC%8A%A4Ingress-%EB%B9%84%EA%B5%90
- k8s의 외부 네트워킹 2 : https://nearhome.tistory.com/95
- k8s의 외부 네트워킹 1 : https://nearhome.tistory.com/105

- K8S service Deep Dive (구체적 내용)
	- service 개요 : https://anggeum.tistory.com/entry/Kubernetes-%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-%EC%84%9C%EB%B9%84%EC%8A%A4Service-Deep-Dive-1
	- clusterIP : https://anggeum.tistory.com/entry/Kubernetes-%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-%EC%84%9C%EB%B9%84%EC%8A%A4Service-Deep-Dive-2-ClusterIP
	- cluserDNS : https://anggeum.tistory.com/entry/Kubernetes-%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-%EC%84%9C%EB%B9%84%EC%8A%A4Service-Deep-Dive-Cluster-DNS
	- nodePort : https://anggeum.tistory.com/entry/Kubernetes-%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-%EC%84%9C%EB%B9%84%EC%8A%A4Service-Deep-Dive-4-Nodeport
	- LoadBalancer : https://anggeum.tistory.com/entry/Kubernetes-%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-%EC%84%9C%EB%B9%84%EC%8A%A4Service-Deep-Dive-4-LoadBalancer
	- EndPoint : https://anggeum.tistory.com/entry/Kubernetes-%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-%EC%84%9C%EB%B9%84%EC%8A%A4Service-Deep-Dive-4-LoadBalancer
	
- 쿠버네티스 네트워킹 시리즈
	- Pods : https://coffeewhale.com/k8s/network/2019/04/19/k8s-network-01/
	- Service : https://coffeewhale.com/k8s/network/2019/05/11/k8s-network-02/
	- Ingress : https://coffeewhale.com/k8s/network/2019/05/30/k8s-network-03/
	
