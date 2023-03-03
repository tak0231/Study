
### Docker 전체적 구조
![image](https://user-images.githubusercontent.com/80720210/183785374-7d36956c-aa6c-41b5-ae47-870ce982b5de.png)

- Docker Engine = dockerd

### Docker 0.9
![image](https://user-images.githubusercontent.com/80720210/183378602-2d1fe51e-6ce5-43f4-bc05-76128400c1ac.png)
- docker 0.9버전의 구조
- 본래 이와 같이 Doker 하나로 구성되어 있었음

### Docker 1.1이후
![image](https://user-images.githubusercontent.com/80720210/183378560-fcf62f42-2a32-4e6b-94ef-f8ee4fd9fd4d.png)
![image](https://user-images.githubusercontent.com/80720210/183377674-5241cf2e-6412-4b01-9aa8-c3ceaf17e851.png)
- Docker 구조
  - 아래는 Linux 커널이 포함된 구조

- 이후 고수준 컨테이너 런타임 / 저수준 컨테이너 런타임 프로젝트로 분리되며 위와같은 구조가 됨
- 0.9의 libcontainer 프로젝트는 OCI에 기부되고, runc라는 이름으로 변경이 됨

```
ps aux | grep docker
```
- 도커 구조를 확인할 수 있는 명령어

### dockerd
- 컨테이너를 지속적으로 관리하는 데몬 프로세스
- 사용자가 입력한 docker 명령어 (docker cli) -> dockerd로 전달되고 실행됨
- Docker CLI로부터 RESTful API 형식의 요청을 수신하여 처리함
- dockerd는 세가지 소켓 유형을 통해 API 요청을 수신할 수 있음
  - unix
  - tcp
  - fd
- default 경로 : /usr/bin/dockerd
- containerd에 의존적 => containerd 없이 단독으로 실행 불가
- dockerd = dockerengine 이라고 불리기도 함
- 다음의 docker 기능을 담당하고 있음
  - container build
  - security
  - volume
  - networking
  - secrets
  - orchestration(오케스트레이션)
  - distributed state

### containerd
- 일반적으로 docker = container runtime tool 이라 말함
- but 엄밀히는 docker의 구성 중 containerd가 container runtime tool에 해당됨
- docker -> 1.1부터 contiainerd를 container runtime tool로 사용
  - 기존의 컨테이너 런타임을 containerd로 분리시킴
  - 오픈소스로 발표하고 MS, Google, AWS, IBM 등과 공동 개발하기로 함
- container의 기능만 가지고 있음
  - network, volume같은 기능들은 다른 구조에게 맡김
- containerd는 독립 실행형 고수준 컨테이너 런타임
  - 즉, containerd는 runC같은 저수준 컨테이너 런타임에 명령을 전달하여 컨테이너의 실행 같은 라이프사이클을 관리함
  - 독립적 = 컨테이너 런타임을 특정 벤더에 의존하지 않음
    - 벤더 = 제조업체 or 판매업체

- 데몬 프로세스 형태로 실행됨(dockerd와 동일)
- containerd -> Namespace 기능을 제공함
  - containerd가 "moby"라는 이름의 Namespace에 모든 Contianer를 생성함

<containerd의 기능>
1) Container 생성
  - Container를 구동(run) 하지는 않음
  - Container-shim과 runC를 "이용하여" Container를 생성하는 역할을 함
    - containerd-shim과 runC를 이용하여 생성함
2) Image pull
- container에 필요한 image가 존재하지 않는다면 -> Image register Server에서 Pull하는 역할을 수행함

3) Container lifecycle 관리
- container start, stop 상태를 관리함


### Container-shim
- 


## 컨테이너 런타임(Container runtime)
### 저수준 컨테이너 런타임
![image](https://user-images.githubusercontent.com/80720210/181204613-600d20c2-6617-4086-9893-f174fe8cd441.png)

- 대표적 저수준 컨테이너 런타임 : runC
  - OCI를 준수하는 대표적 저수준 컨테이너 런타임
- 컨테이너 -> 리눅스의 namespace와 cgroup을 이용하여 구현됨
  - namespace : 각 컨테이너에 대해 시스템 리소스를 가상화(파일 시스템이나 네트워킹 같은)
  - Cgroup : 각 컨테이너가 사용할 수 있는 CPU 및 메모리 등의 리소스 양을 제한하는 역할을 함
    - controll group
- 저수준 컨테이너 런타임 
  - namespace / cgroup을 설정한 후 -> 해당 namespace 및 cgroup 내에서 명령을 실행함
  - 컨테이너를 실제 실행하는 역할을 함
    - but 이미지로부터 컨테이너를 실행하는 기능은 없음
    - 이미지와 관련된 API 등의 기능이 필요하기 때문
    - 저수준 런타임은 제공 X => 고수준 런타임만이 제공

- docker에는 docker-runc라는 저수준 런타임이 존재함

### 고수준 컨테이너 런타임
![image](https://user-images.githubusercontent.com/80720210/183375754-111287b9-4b33-47b7-bfa1-9fb2f4e780d5.png)


- 저수준 런타임 위에 배치되어 있음
- 이미지로부터 컨테이너를 실행할 수 있음
- docker에는 docker-containered라는 고수준 런타임이 존재함


![image](https://user-images.githubusercontent.com/80720210/181181732-5dfd9431-954e-4fc4-8aa9-fd09f3c95199.png)
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
### OCI
- 컨테이너 런타임에 대한 표준
- 컨테이너를 실행하기 위한 저수준 런타임

### CRI
- Container Runtime Interface
- 쿠버네티스 측에서 제공하는 런타임 추상화 계층
-  kubelet과 컨테이너 런타임과의 표준

### CRI-O



<br/>

### 참고
- Docker 구조1 : https://ikcoo.tistory.com/194
- Docker 구조2 : https://kangwoo.kr/2020/07/26/%EB%8F%84%EC%BB%A4-%EA%B5%AC%EC%A1%B0/
- - OCI / CRI / 저수준 * 고수준 컨테이너 런타임 : https://s-core.co.kr/insight/view/oci%EC%99%80-cri-%EC%A4%91%EC%8B%AC%EC%9C%BC%EB%A1%9C-%EC%9E%AC%ED%8E%B8%EB%90%98%EB%8A%94-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-%EC%83%9D%ED%83%9C%EA%B3%84-%ED%9D%94%EB%93%A4%EB%A6%AC%EB%8A%94/
- 컨테이너 런타임 정리() : https://velog.io/@sjoh0704/Container-Runtime-%EC%A0%95%EB%A6%AC
- containerd에 대한 이해 : https://kr.linkedin.com/pulse/containerd%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%99%9C-%EC%A4%91%EC%9A%94%ED%95%A0%EA%B9%8C-sean-lee
