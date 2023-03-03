### 가상화
![image](https://user-images.githubusercontent.com/80720210/182736048-47869588-edc4-49d3-bd8e-081c6eb65ca2.png)

- 물리적인 컴포넌트(Components, HW 장치)를 논리적인 객체로 추상화 하는 것
  - 하나의 장치를 여러개처럼 동작시킬 수도 있고
  - 여러개의 장치를 하나의 장치처럼 동작시킬 수도 있음

- 가상화를 통해 높은 수준의 자원 사용률, 분산처리 능력을 제공할 수 있음

- 다양한 가상화 유형 존재
  - 서버 가상화
    - 가상화 개념의 시초
    - 서버 효율성을 높이기 위해 등장
    - Hypervisor를 통해 제어
  - 데스크탑 가상화
  - ![image](https://user-images.githubusercontent.com/80720210/182736458-2c65bb86-9871-4fb7-a2fd-55d10fe965ed.png)
    - 데이터센터의 서버에서 운영되는 가상의 PC환경을 의미
    - 가상의 컴퓨터 환경을 중앙서버에서 제공하는 소프트웨어 기술
    - 중앙에 있는 가상화 기술을 사용, 생성된 VM 컴퓨팅 환경을 사용자 PC에서 사용할 수 있도록 화면 값을 전달해 주는 형태

  - 네트워크 가상화
  - 스토리지 가상화
  - 데이터 가상화
  - 애플리케이션 가상화
  - 데이터 센터 가상화
  - CPU 가상화
  - GPU 가상화
  - Linux 가상화

<br/>

### Virtual Machine
- 하드웨어를 소프트웨어적으로 구현한 것
- Host라고 하는 컴퓨팅 환경에서 생성됨

<br/>

### 참고 : VirtualBox
- 오라클에서 만든 가상머신(VM) 솔루션 중 하나
- 오픈소스

<br/>

### Hypervisor
![image](https://user-images.githubusercontent.com/80720210/182738596-256c088a-9cbd-4538-a01f-70bc027ce9d3.png)

- 호스트 컴퓨터에서 다수의 운영 체제를 동시에 실행하기 위한 논리적 플랫폼, 소프트웨어
- 하드웨어 스택전체를 가상화 하는 방식
- 하드웨어(물리적 장비)를 가상화하여 VM을 띄워 Controll 하는 것을 의미함
  - 즉, 특정 Hardware의 resource를 할당하여 동작하는 방식
  - Physical HW를 추상화하여 동작함
  - 하나의 Machine 위에서 여러개의 VM이 동작함
    - 각각의 VM은 OS, Binary(Bins), Library(Libs) file이 있어야함
    - 하나의 Machine위에서 동작하는 VM이 증가할수록 저장공간의 낭비가심해짐
  
- 호스트 컴퓨터에서 다수의 운영체제를 동시에 실행할 수 있음

<br/>

### Container
![image](https://user-images.githubusercontent.com/80720210/182738622-2962c64c-071e-4b36-956b-da18850388f9.png)

- 운영체제 수준(OS level)에서 프로세스를 컨테이너 형태로 격리하는 방식
  - 호스트의 OS 커널을 공유함
- 기존의 시스템에 존재하는 프로세스를 해당 시스템에서 격리하여, 독자적인 시스템 환경을 구축하는 기술
  - 리눅스 커널의 기능 중 namespace와 cgroup을 이용한 방법

- 중요 : Container는 VM이 아님
  - 경우에  따라 Type3라고 분류하긴 하지만
  - Container는 VM도 아니며, Hypervisor를 이용하는 방식이 아님 
  - 이 점을 기억해야 함
- OS 자체를 가상화하는 방식 -> 논리적으로 OS를 할당함
- OS 레벨에서 프로세스를 격리시킴
- 모듈화된 프로그램 패키지(image)를 수행함 => Hypervisor와 GuestOS가 불필요함
- 모든 컨테이너들이 하나의 OS커널을 공유함
  - 때문에 가벼움
  - 단, OS 커널에 장애 발생시 모든 컨테이너가 영향을 받음

<br/>

### namespace
- 하나의 system에서 수행되지만, 각각 별개의 공간처럼 격리된 환경을 제공하는 lightweight 가상화 기술
- 리눅스 커널은 6종류의 namespace를 지원함
  - mnt (파일 시스템 마운트)
    - filesystem의 mount 지점을 분할하여 격리 
  - pid (프로세스)
    - 독립적인 프로세스 공간 할당, pid 분할 관리 
  - net (네트워크)
    - network 리소스와 관련된 정보를 분할 (network interface, iptables 등)
  - ipc (SystemV IPC)
    - 프로세스간 독립적인 통신통로 할당
  - uts (hostname)
    - 독립적인 hostname 할당
  - user (uid)
    - uid, gid 분할 격리 

### cgroup (control groups)
- 자원에 대한 제어 가능하게 해주는 리눅스 커널 기능
- 제어하는 리소스
  - 메모리
  - CPU
  - I/O
  - 네트워크
  - device 노드 (/dev/)

### Hypervisor vs Container

![image](https://user-images.githubusercontent.com/80720210/182737209-f2b2a9fd-7b85-4179-bb1c-77b56040ea19.png)

<br/>

## Hypervisor의 분류
![image](https://user-images.githubusercontent.com/80720210/182731677-d93d6213-8649-4c5b-aa23-aba62abd7ade.png)

<위치와 역할에 따른 분류>
### Type1 = Native 또는 Baremetal형 Hypervisor
- Host의 하드웨어 위에 Hypervisor를 설치하는 방식
  - Host에 OS 없이 Hypervisor 바로 설치하는 
- 베어 메탈 = 하드웨어 상에 어떤 소프트웨어도 설치 X인 상태
- 가상화를 위한 하이퍼바이저 OS 없이 물리서버를 그대로 제공하는 방식
  - 즉, Host의 하드웨어 위에 Hypervisor 설치하여 물리서버 그대로 제공하고, 여기에 OS 등은 사용자가 설치하는 방식
  - OS 없이 하드웨어 직접 제어 가능하며, OS 설정까지 가능하다는 이점 존재함

<br/>

### Type2 = Hosted형 Hypervisor
- Host의 OS 위에 Hypervisor를 설치하는 방식
  - 즉, Host 위에 OS가 이미 설치되어 있고, 여기에 다시 Hypervisor를 얹은 뒤, 각 머신별로 OS를 설치하는 방식
  - VMware workstation를 생각하면 됨


### 정리
- Hypervisor는 Host 머신 위에 Hypervisor를 얹고, 그 위에 머신들을 생성, 이 머신들은 각각의 OS를 가짐
  - Type 1 : Host에 OS 설치 X => 바로 Hypervisor를 얹음
  - Type 2 : Host에 설치된 OS 존재 => 그 위에 Hypervisor를 얹는 방식

- 컨테이너는 Host 머신에 설치된 OS 위에 컨테이너 런타임을  얹고, 그 위에 컨테이너들을 생성
  - 각각의 컨테이너는 Host의 OS 커널을 공유함 (별도의 OS 설치 필요 X)

<br/>

<가상화 방식에 따른 분류>


<br/>

### 참고
- https://velog.io/@bonjaski0989/%EA%B0%80%EC%83%81%ED%99%94-Virtualization
- http://cloudrain21.com/hypervisor-types
- https://velog.io/@_gyullbb/Hypervisor-Container-tjhd5c1w
- https://blog.naver.com/PostView.naver?blogId=ssang8417&logNo=222633834165&parentCategoryNo=&categoryNo=34&viewDate=&isShowPopularPosts=true&from=search
- https://spidyweb.tistory.com/71
- 베어메탈 서버 : https://library.gabia.com/contents/infrahosting/9300/
- 하이퍼바이저 개념 : https://dora-guide.com/%ED%95%98%EC%9D%B4%ED%8D%BC%EB%B0%94%EC%9D%B4%EC%A0%80/
- namespace & cgroup 1 : https://tech.ssut.me/what-even-is-a-container/
- namespace & cgroup 2 : https://bakery-it.tistory.com/29
