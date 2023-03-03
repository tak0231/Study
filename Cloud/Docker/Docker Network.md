
![image](https://user-images.githubusercontent.com/80720210/182799438-4e0f30b3-4903-4430-add8-8ce03c1f398d.png)

- Linux에 Docker를 설치하면
  - 서버의 물리 NIC(Network Interface Card)가 docker0라는 가상 브리지 네트워크로 연결됨
    - docker0 : virtual interface
    - 
  - docker0는 Docker를 실행시키면 기본적으로 만들어짐
  - docker 컨테이너가 실행되면 -> 컨테이너에 172.0.0.0/16라는 서브넷마스크를 가진 Private Ip가 eth0으로 자동 할당됨
  - 이 가상 NIC는 OSI Layer 2인 가상 네트워크 인터페이스로,  NIC와 터널링 통신을 함
  - 가상 NIC(vethxxx)는 컨테이너에서 etho0로 인식됨

- docker는 기본적으로 NAT 환경이 적용됨

### NIC
- Network Interface Controller

### docker0

### Private Ip

### eth0
- 이더넷카드 번호 0번
  - 보통 이더넷카드하면 랜카드를 말하는 것
- 0번은 기본적인 랜카드 번호
- 랜카드 설치 위치에 따라 eth1, eth2 ... 이런식으로 늘어남

### NAT
- Network Address Translation
- 하나의 공인 IP를 여러개의 사설 IP로 변환하는 시스템

### NAPT
- Network Address Port Translation
- Docker에서는 NAPT 기능을 사용하여 

<br/>

### 참고
- https://5kyc1ad.tistory.com/254
- https://imjeongwoo.tistory.com/107
- https://bluese05.tistory.com/15
- https://nomad-programmer.tistory.com/291
