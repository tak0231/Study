# 시스템 관련 개념
### /etc/profile
-  시스템 차원의 디폴트 변수를 관리함 

- **login shell (profile)**
  - 처음 리눅스 부팅 후 특정 shell로 접속하는 경우
  - 시스템 로그인하면 login shell로 동작하는 것
  - bash(=기본 login shell) -> login shell 동작시 profile을 읽어옴
  - /etc/profile 
    - /etc/profile.d 디렉토리 안의 모든 쉘 스크립트를 실행시킴
  - /etc/profile.d
    - vim, qt, lang, colors 등의 다양한 설정이 sh 파일 형태로 존재함

  - 즉, login shell 동작 
    - -> /etc/profile 읽어옴 
    - -> /etc/profile.d 내의 쉘 스크립트 실행 
    - -> 홈 디렉토리의 profile(~/.profilie)을 읽어옴

- **none-login shell / interactive (rc file)**
  - 특정 shell로 들어가지 않고 직접 sh 등의 명령어로 특정 shell의 들어가는 경우
  - bash (기본 login shell)이 아닌 다른 shell에서 bash 호출시 동작하는 방식
  - 이 방식에서는 bash가 rc file을 읽어옴
  - **bashrc 파일**
    - bash 수행시 함수 제어하는 지역적인 시스템 설정 관련 파일

- 실행 순서
  1. /etc/profile
    - 부팅 후에 적용됨
  2. ~/.bash_profile 또는 ~/.bash_login 또는 ~/.profile
    - ~/.bash_profile
      - 개인 사용자별 환경설정 파일
      - 로그인 할 때마다 읽어옴 (로그인 할 때마다)
  3. ~/.bashrc
    - 개인 사용자 대한 환경 설정 파일
    - bash 실행될 때마다 읽어옴
  4. /etc/bashrc
    - 모든 사용자(=시스템 전역) 대한 환경 설정 파일
    - 새로운 bash 실행 될 때마다 읽어옴 

### sed
- streamlined editor
- 수정 / 치환 / 삭제 / 글 추가 등의 편집기 기능을 수행함
  - vi / vim -> 파일을 열어 편집하는 방식
  - sed -> 명령행에서 파일, 내용을 인자로 전달받아 편집하는 방식 

### EOF
- End Of File
- Shell Script EOF
- 직접 입력한 텍스트를 파일에 저장할 때 사용함
- 내용 작성 후 EOF 단어로 파일을 끝냄
- ``` 
  >> cat << EOF > test.txt
  > Hello
  > World
  > EOF
  ```

### seLinux
- Security Enhanced Linux (보안 강화 리눅스)
- 리눅스의 시스템 액세스 권한을 제어하기 위한 보안 아키텍쳐
- 3가지 동작모드 존재
  - **enforcing**
    - SELinux 활성화된 상태 (보안정책 실행, 로그 기록, 보호 모두 수행) 
  - **permissive**
    - SELinux가 보안 정책에 대해 로그는 기록하지만 실제 차단은 X하는 상태 (제한적 사용)
  - **disabled**
    - SELinux 비활성화된 상태

### Iptables
- 리눅스 패킷 터널링 도구
- 방화벽 구성 NAT(Network Address Translation)에 사용됨
- 리눅스에서 방화벽을 설정하는 도구
  - 커널 2.4 이전에는 ipchains를 사용했음 (iptables로 대체됨)
- 즉, 리눅스에서 방화벽 용도로 사용됨

- **패킷 터널링**
  - 지나가는 패킷의 헤더를 보고 해당 패킷의 허용 / 차단을 결정하는 것
  - 패킷 필터링 / 처리
  - 패킷 => 헤더 + 데이터
  - 헤더 : 출발지 IP:Port, 도착지IP:Port, checksum, 프로토콜 옵션 등을 포함함

### NAT (Network Address Translation)
- 사설망(Private Network) <-> 공인망(대외망)을 연결하기 위한 IP 변환 기술
- IP 패킷에 있는 출발지 및 목적지의 IP 주소와 TCP / UDP 포트 숫자 등을 바꿔 재기록하면서 네트워크 트래픽을 주고 받게하는 기술
- IPv4의 제한된 주소 문제 해결하기 위해 개발됨
  - 사설망 / 대외망의 대역을 분리하여 한정된 대외망의 IP 자원의 소모를 줄임
  - 사설망은 사설망에서만 사용할 수 있는 IP 대역 존재
  - 대외망과 연결되기 위해서는 변환할 필요 있음
  - 사설망과 대외망을 연결하기 위한 IP 변환 기술이 NAT

### IPv4 & IPv6
- **IPv4**
  - 일반적으로 사용하는 IP주소
  - but 주소 표현의 제약 존재
  - 이로 인해 주소 고갈 문제 / 멀티미디어 서비스 대응 문제 등 존재

- **IPv6**
  - IPv4 문제를 해결하기 위한 차세대 인터넷 프로토콜

### IP 마스커레이드 (Masquerade)
- 커널에서 제공하는 NAT(Network Address Transration) 기능
- 리눅스의 NAT 기능
- 내부 컴퓨터들이 리눅스 서버를 통해서 인터넷 등 다른 네트워크에 접속할 수 있도록 해주는 기능
  - 사설 대역 IP를 가진 클라이언트들의 패킷을 전송하면 라우터에서 모두 자신이 보낸 것처럼 해서 패킷을 전송함 (라우터는 공인 대역 IP를 가짐)
- 주소 변환, 포트포워딩 기능 등을 수행함

### 브릿지 네트워크 (Bridged Networking)
- Host와 Guest의 네트워크를 연결(Bridge)하여 Guest의 컴퓨터가 네트워크 하는 방식
- 즉, Host와 Guest를 하나로 연결하여 두 개의 네트워크를 하나의 네트워크처럼 쓰는 것
- Guest와 Host의 네트워크가 브릿지 됬다 = 동등한 수준의 네트워크를 제공받는다
- 동등한 수준, 자격을 부여받음 => Guest 컴퓨터도 공인 IP 할당받을 수 있으며, 사설망에서 Host와 동일한 대역의 IP 할당 가능해짐

- OSI 모델 데이터 링크 계층에 있는 여러개의 네트워크 세그먼트를 연결해줌
  - 물리 계층의 연결 장비들과도 유사 (ex. 리피터, 네트워크 허브)
  - but 네트워크 브릿지는 위의 장비들과 다르게 주변 네트워크 세그먼트로 다시 전송하기만 하지 않음
  - 특정 네트워크로부터 오는 트래픽 자체를 관리할 수 있음


<br>

# K8S 용어
### 온프레미스
- 기업이 자체 시설에서 보유하고 직접 유지 관리하는 프라이빗 데이터 센터

### swap
- 물리 메모리(RAM) 용량 부족시 하드디스크 일부 공간을 메모리 처럼 사용하는 것
  - 즉, 가용된 메모리보다 더 큰 메모리 할당 가능한 기능
- K8S의 kubelet은 메모리 swap을 고려하지 않고 설계되어 있음 
  - 쿠버네티스의 철학 => 주어진 인스턴스 자원 100% 가깝게 사용하는 것
  - swap은 이 철학에 부합되지 않는 기능
  - -> 때문에 K8S node들은 swap을 비활성화 해줘야 함
  - (swap -> 인스턴스 자원의 일관성을 해침 / 가용할 수 있는 메모리가 변동되므로)

### br_netfilter
- 브릿지 네트워크를 필터할 수 있는 모듈
- br = bridge
- iptables가 bridge driver를 사용하여 통신을 하는 패킷들(트래픽)을 filter 할 수 있게 해주는 모듈
  - driver 다른 종률 host driver가 있음 
- 이 기능이 있어야 Pod 간 통신이 가능함

<br>

<hr>

### 참고
- /etc/profile : https://www.ibm.com/docs/ko/aix/7.3?topic=files-etcprofile-file
- login shell : https://coding-chobo.tistory.com/72
- sed : https://etloveguitar.tistory.com/47
- EOF : https://shonm.tistory.com/666
- seLinux : https://jesc1249.tistory.com/337
- Iptables 1 : https://itwiki.kr/w/%EB%A6%AC%EB%88%85%EC%8A%A4_iptables
- Iptables 2 : https://linuxstory1.tistory.com/entry/iptables-%EA%B8%B0%EB%B3%B8-%EB%AA%85%EB%A0%B9%EC%96%B4-%EB%B0%8F-%EC%98%B5%EC%85%98-%EB%AA%85%EB%A0%B9%EC%96%B4
- NAT 1 : https://www.stevenjlee.net/2020/07/11/%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-nat-network-address-translation-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%A3%BC%EC%86%8C-%EB%B3%80%ED%99%98/
- NAT 2 : https://learn.microsoft.com/ko-kr/azure/rtos/netx-duo/netx-duo-nat/chapter1
- Private Network & NAT : https://aws-hyoh.tistory.com/145
- IPv4 & IPv6 : https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=hai0416&logNo=221566797342
- IP 마스커레이드 : https://lascrea.tistory.com/105
- 브릿지 네트워크 1 : https://itmore.tistory.com/entry/NAT-%EC%99%80-Bridged-Networking-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC
- 브릿지 네트워크 2 : https://ko.wikipedia.org/wiki/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC_%EB%B8%8C%EB%A6%AC%EC%A7%80
- 온프레미스 : https://www.hpe.com/kr/ko/what-is/on-premises-vs-cloud.html
- swap : https://kgw7401.tistory.com/50
- br_netfilter : https://morian-kim.tistory.com/18
