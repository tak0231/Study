# Pods
- 한개 이상의 컨테이너로 이루어진 요소
- 언제든 죽을 수 있음
- 어플리케이션의 최소 단위
- 같은 host, network 스택, volume 등의 리소스를 공유하는 요소

- eth0(NIC) -> docker0(bridge) -> veth0(container)로 접근됨

## docker0
- 컨테이너 네트워킹에서 브릿지 역할
- 컨테이너의 가상 어댑터인 veth의 defulat gateway 역할을 함

## docker0(bridge) <-> container
- 다음 두 네임스페이스 간에 통신 이루어짐
	- container : network namespace
	- docker0 : root network namespace

- 정확한 내용은 검색 필요




# 관련 용어

### gateway = 라우터
- 다른 대역으로 라우팅 해줄 수 있는 L3 스위치급 이상의 장비
	- 같은 대역 (즉, 같은 

- 강조하는 측면에 따라
	- 하드웨어적 측면을 강조 : 라우터
	- 소프트웨어적 측면을 강조 : 게이트웨이
	
- 네트워크 to 네트워크로 이동하기 위해 거쳐야하는 지점
	- 외부에서 오는 여러 프로토콜에서 전성되는 패킷을 받고, 패킷을 전송하기 위한 최적의 경로 선택하며, 흐름제어를 함

- ** 라우팅 (=경로 설정)**
	- 인터넷 공간에서 다른 호스트간을 연결해주는 기능
		- LAN 영역 사이를 연결해주는 기능
	- 즉, 라우터는 패킷을 분석하여 어디로 보낼지를 결정함
		- 데이터 전송 위해 최적의 경로를 설정해줌
		
- 네트워크 간 프로토콜이 다를 경우 이를 중재하는 역할을 함
	- 프로토콜 간 변환
	- 주로 하위계층 (1~3 Layer)에서 라우터가 담당

- 일반적인 가정
	- 집 -> 공유기 -> Provider의 라우터 -> 인터넷 망
		- 여기서 Provider의 라우터가 게이트 웨이 역할을 함
		- 공유기는 정확한 의미에서는 NAT 장비
	
- 가정에서는 라우터 필요 X
	- 가정에서 발생한 패킷 -> 무조건 ISP(한국통신 or 하나로 통신 등)로 가게 되어 있음
		
### 공유기 vs 라우터
- 공유기 : 
	- NAT 장비
	- 단일 회선, 단일 IP에서 PC가 인터넷 연결할 수 있도록 IP 변환해주는 장치
- 라우터 : 
	- 내부, 외부 가리지 않고 서버와 연결된 PC 간 연결 할 수 있게 해주는 장치
	- 라우팅 해주는 기계

### NAT (Network Address Transport)
- 공인 IP, 사설 IP간 변환
- 즉, 사설  IP가진 호스트가 외부와 통신할 수 있게 변환해주는 기술
		
- ** Hop Count (홉 수) **
	- 네트워킹을 위해 거치는 게이트웨이 수

### default gateway (기본 게이트웨이)
- 호스트에 여러 NIC 장착 가능, 서로 다른 네트워크 연결 가능함
- 그 중, 기본으로 사용할 네트워크가 기본 게이트웨이
- 기본 게이트웨이와 IP 주소의 네트워크 ID는 동일

### IP 주소 클래스
- 총 5가지 종류
	- A, B, C, D, E
		- 보통 A,B,C만 사용
		- D, E는 멀티캐스트, 연구용

### 서브넷 마스크
- C class 하나를 4개의 네트워크로 Subnetting 해 놓은 상태의 IP
	- IP 주소에서 어디까지가 네트워크 ID인지 알려줌

- ** 서브넷팅 (subnetting) **
	- 네트워크 영역, 호스트 영역 분리하는 작업
	- 목적 : 네트워크 성능 보장, 자원의 효율적 분배
- ** 슈퍼넷팅 (supernetting) **
	- 서브넷팅의 반대
	

### IP 주소
- 네트워크 ID + 호스트 ID

### 네트워크 ID
- 여러 호스트가 포함된 네트워크를 관리하기 위한 식별자

### 호스트 ID
- 동일한 LAN 영역에서 호스트 구별하기 위한 식별자

### 브로드 캐스트 도메인
- 라우터, 네트워크 장비 없이 통신할 수 있는 영역

### WAN
- 광역 네트워크
- ISP (인터넷 서비스 제공자)가 제공하는 서비스 통해 구축된 네트워크
- ** public IP **
	- 공용 IP

### LAN
- 동일한 네트워크 ID를 공유하는 영역
- 건물 안 or 특정 지역
- ** private IP **
	- LAN 안에서만 사용 가능한 IP
	
	











## 참고자료

###
- 쿠버네티스 네트워킹 이해하기 / PODS : https://coffeewhale.com/k8s/network/2019/04/19/k8s-network-01/

### 관련 용어
- gateway : https://melonicedlatte.com/network/2020/04/28/201100.html
- 라우터 : https://sites.google.com/site/21herecomeputer/123123
- 공유기 vs 라우터 : http://m.enuri.com/knowcom/detail.jsp?kbno=19984
- IP 주소 클래스 : https://limkydev.tistory.com/168
- 서브넷 마스크와 게이트 웨이 : https://medium.com/pocs/tcp-ip-%EC%9D%B4%EB%A1%A0-ip-%EC%A3%BC%EC%86%8C-%EC%84%9C%EB%B8%8C%EB%84%B7-%EB%A7%88%EC%8A%A4%ED%81%AC-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EA%B8%B0%EB%B3%B8-%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%9B%A8%EC%9D%B4-ccd6d832711e
- WAN vs LAN : https://unit-15.tistory.com/110
