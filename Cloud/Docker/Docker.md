## Docker
### Docker
- 리눅스 컨테이너를 관리하는 도구
  - 애플리케이션의 신속한 구축, 테스트, 배포할 수 있는 소프트웨어 플랫폼
  - 컨테이너라는 표준화된 유닛으로 패키징함
  - Docker 사용 ->  환경에 구애받지 않고 신속하게 애플리케이션의 배포 / 확장 가능
- Docker

### Image
- 특정 프로세스를 실행하기 위한 모든 파일과 설정값(환경)을 지닌 것
- 더 이상의 의존성 파일을 컴파일 하거나 설치할 필요 없는 상태의 파일을 의미함
- 서버 프로그램, 소스코드 및 라이브러리, 컴파일된 실행파일을 묶은 형태
- 애플리케이션과 바이너리, 라이브러리 등을 패키지로 묶어 배포

### Layer
- 기존 이미지에 추가적인 파일이 필요할 떄 다시 다운로드 받는 방법이 아닌 해당 파일을 추가하기 위한 개념
- 이미지는 여러개의 읽기 전용(readOnly) Layer로 구성됨
- 파일이 추가되면 새로운 Layer가 생성됨
- 도커는 여러 개의 Layer를 묶어서 하나의 파일시스템으로 사용할 수 있게 해줌
- 이 때문에 이미지 ≒ Layer

### Image Registry
- Docker 이미지를 저장하는 원격 스토리지

### Docker hub
- Docker Hub는 Docker에서 운영하는 Docker 이미지 저장소 서비스
- Docker Hub는 Docker의 기본이자 공식 Image Registry
- 도커 허브에서 다음과 같이 이미지를 내려받을 수 있음
  - $ docker pull [이미지이름]:[이미지버전]
  - 버전 명시 X시 -> 자동으로 최신버전으로 받음

### Container
- 이미지를 실행한 상태
- 응용프로그램의 종속성과 함께 응용프로그램 자체를 패키징 or 캡슐화하여 격리된 공간에서 프로세스를 동작시키는 기술

## Container Runtime 관련 개념
### Container Runtime
- 컨테이너를 다루는 도구
- 컨테이너 실행을 담당하는 도구
- 대표적인 도구 : 도커

![image](https://user-images.githubusercontent.com/80720210/181203810-baf50786-d7cd-4852-b18c-46a0aba529ba.png)

- 컨테이너를 실행하기 위해서는 위의세 단계를 걸쳐야함
- OCI 등장 전에는 docker가 비공식적이지만 표준의 역할을 함
- 이 때, 세번째 단계인 컨테이너 실행 부분만 표준화를 함
- 이 때문에 컨테이너 런타임은 크게 두가지 수준으로 구분됨
  - 저수준 런타임 : 실제 컨테이너를 실행함
  - 고수준 런타임 : 컨테이너 이미지의 전송, 관리, 이미지 압축 풀기 등을 실행함
- docker에서는 docker-container라는 고수준 런타임 / docker-runc라는 저수준 런타임을 이용함


  
### DockerFile
- 컨테이너에 설치해야하는 패키지, 소스코드, 명령어, 환경변수설정 등을 기록한 하나의 파일
- 이 DockerFile을 빌드하면 자동으로 이미지가 생성됨
- DockerFile을 통해 애플리케이션 빌드 및 배포를 자동화 할 수 있음

- 컨테이너에서 작업 후 이미지로 commit하는 것보다 Dockerfile을 이용해서 빌드 배포하는 것이 더 좋다고 함

```
FROM <이미지>
VOLUME <컨테이너 디렉터리>
ARG <Dockerfile의 변수>
ADD <복사할 파일 경로> <이미지에서 파일이 위치할 경로>
ENV <환경 변수><값>
ENTRYPOINT <명령어>
```

```
FROM java:8
VOLUME /tmp
ARG JAR_FILE=./target/BootSns-0.0.1-SNAPSHOT.jar
ADD ${JAR_FILE} MyDiary.jar
ENTRYPOINT ["java","-jar","/MyDiary.jar"]
```
- 예시

### Docker-compose
- 여러개의 컨테이너로 이루어진 서비스를 구축, 실행하는 순서를 자동으로 하여 관리 간단하게 함
- 여러개 컨테이너 설정 내용을 하나의 yaml 파일에 모앙서 사용함
  - docker-compose.yml
- compose 파일을 준비 -> 커맨드로 yaml 파일 실행
  - 안에서 설정 정보 읽어서 모든 컨테이너(설정에 들어간)를 실행시킴


```
docker-compose up
```
- docker compose 실행

```
docker-compose stop
```
- docker compose 중지


### docker stack

### docker vs docker-compose
- docker
  - Single Container를 관리하는것
  - 커맨드 라인에서 명령어를 실행할 수 있음
  - 컨테이너를 관리하는 런타임

- docker-compose
  - 일종의 툴
  - yaml file 기반으로 multi container 관리할 수 있는 client
  - yaml 파일에 명령어를 적어서 컨테이너를 정의하고 관리

### Dockerfile vs docker-compose.yml
- 역할 비슷한데 분리한 이유는?
- Dockerfile
  - 이미지를 어셈블(모으다, 조립하다)하기 위해 호출할 수 있는 명령이 포함된 간단한 텍스트 파일
  - 단순히 image를 빌드까지만 해주는 선언
  - run에 대한 정의도 가능하지만, 실제 실행을 시키진 않음

- Docker-compose
  - 여러개의 이미지를 빌드, 실행(run)까지 포함해서 관리해주는 툴
    - 빌드시, dockerfile을 사용할 수도 있음
  - 앱이 실행되는 동안 컨테이너를 관리하는 역할
  - 

### Docker in  Docker (도커 인 도커)
- 젠킨스같은 CI/CD 툴을 컨테이너에 설치하고, 젠킨스에서 프로젝트 생성하고, 프로젝트를 컨테이너에서 관리하는 환경 구축시 사용하는 환경
- 컨테이너 = 독립된 환경
- 컨테이너에 설치된 젠킨스가 컨테이너들을 관리하려면 도커 볼륨을 연동해야 함

### Spring Boot / GitLab / Jenkins / docker
- GitLab의 프로젝트에 webhook 생성
- Jenkins 파이프라인 생성
- Dockerfile 작성

## 쿠버네티스 (kubernates)
- 컨테이너 런타임을 통해 컨테이너를 오케스트레이션하는 도구
- 쿠버네티스 사용 -> 컨테이너를 하나씩 관리하지 않아도 됨

### 오케스트레이션(Orchestration)
- 다수의 컨테이너와 이를 사용하는 환경의 설정을 관리하는 것
- "여러 서버"에 걸친 컨테이너 및 사용하는 환경 설정을 관리하는 

[대표적 컨테이너 오케스트레이션 도구들]
- 쿠버네티스
- 도커 스웜
- 아파치 메소스

![image](https://user-images.githubusercontent.com/80720210/181194769-e661123a-0849-43c2-8194-d9d7e0a202cb.png)

### 전통적인 배포 
- 애플리케이션을 물리 서버에서 실행함
- but 여러 app 돌릴시 -> 각각의 리소스의 한계를 정의할 방법 X -> 리소스 할당 문제 발생
- 이를 해결하기 위해선 여러 물리 서버 필요 but 비용 증가

### 가상화된 배포 시대
- CPU에서 여러 VM을 실행하는 방법
- 가상화를 통해 VM간의 app을 분리할 수 있음 + 서로 자유롭게 액세스 불가
- 물리 서버의 리소스 효율적 사용 가능해짐 + 비용 절감(보다 적은 물리 서버 요구됨) + 확장성 뛰어남
- VM = 가상화된 하드웨어상에서 "자체 운영체제"를 포함한 모든 구성요소를 실행하는 하나의 완전한 머신 

### 컨테이너 개발 시대
- 컨테이너 = VM과 유사 but 격리성 저하됨 (OS를 공유함)
  - 자체 파일 시스템, cpu 점유율, 메모리, 프로세스 공간이 있음
  - VM에 비해 몇가지 장점 존재

### 온프레미스 On-premise
- premise : 전제 / 토지 및 부속물이 딸린 가옥 / 전기재산
- 기업의 서버를 클라우드 같은 원격 환경에서 운영하는 방식이 아닌 자체적으로 보유한 전산실 서버에 직접 설치해 운영하는 방식
- 클라우드 컴퓨팅 이전에 기업 인프라 구축의 일반적인 방식이었음


<hr>

### Real User id

### Effective User id

## 볼륨 / 바인드 마운트
![image](https://user-images.githubusercontent.com/80720210/183003842-e7bb020a-2be3-44e0-b2b2-38172f92c533.png)

- 컨테이너의 데이터들 => 컨테이너 삭제하면 전부 삭제됨
  - 즉, 컨테이너의 생명주기에 따라 데이터가 지워짐
  - but 일반적으로 생명주기와 관계없이 영속성을 유지해야 함
- Docker의 데이터들의 영속성을 위해서 두가지 옵션이 제공됨
  - 1) Voulme
  - 2) Bind mount

### Volume
- docker 커맨드로 volume을 생성할 수 있음
```
docker volume create our-vol
docker run -v our-vol:/var/jenkins_home jenkins/jenkins:lts
```
- docker에서 volume을 생성해서 연결해줄 수 있음
  - 컨테이너의 생명주기와 상관없음
  - 기존에 생성했던 volume을 다시 mount 하는 것으로 기존의 데이터를 사용할 수 있음
- 여러개의 컨테이너 => 하나의 볼륨에 접근 가능함
  - 즉, 컨테이너간데이터 공유가 가능함
- v 옵션으로 볼륨을 마운트할 수 있음
  - -v [볼룸명] : [컨테이너 내부 경로]
  - 내부 경로 -> 볼륨에 저장하고 싶은 경로를 지정해주면 됨

```
docker volume rm our-vol
```
- 이런식으로 볼륨의 삭제가 가능함

### Bind Mount
- 호스트 파일 시스템의 특정 경로를 컨테이너로 바로 마운트하는 방법
```
docker run -v /var/jnekins_home:/var/jenkins_home jenkins/jenkins:lts
```
- v 옵션시 콜론(:) 앞의 경로를 마운트명 대신 위와 같이 호스트의 경로를 지정해주면 됨


<br/>

### 참고
- 도커란 무엇인가 : https://aws.amazon.com/ko/docker/
- Container / image : https://velog.io/@ragnarok_code/%EB%8F%84%EC%BB%A4-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88Container%EC%99%80-%EC%9D%B4%EB%AF%B8%EC%A7%80Image%EB%9E%80
- OCI / CRI / 저수준 * 고수준 컨테이너 런타임 : https://s-core.co.kr/insight/view/oci%EC%99%80-cri-%EC%A4%91%EC%8B%AC%EC%9C%BC%EB%A1%9C-%EC%9E%AC%ED%8E%B8%EB%90%98%EB%8A%94-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-%EC%83%9D%ED%83%9C%EA%B3%84-%ED%9D%94%EB%93%A4%EB%A6%AC%EB%8A%94/
- Dockerfile : https://velog.io/@ckstn0777/%EB%8F%84%EC%BB%A4%ED%8C%8C%EC%9D%BCDockerfile
- Docker-Compose란 : https://velog.io/@kjyeon1101/Docker-compose%EB%9E%80
- docker vs docker compose : https://gahui-developer123.tistory.com/99
- dockerfile vs docker compose 1 : https://velog.io/@s2moon98/dockerFile%EA%B3%BC-docker-compose.yml-%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90
- dockerfile vs docker compose 2 : https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=sokki98&logNo=221256778799
- 컨테이너 오케스트레이션 : https://2mukee.tistory.com/221
- 쿠버네티스란 : https://kubernetes.io/ko/docs/concepts/overview/what-is-kubernetes/
- 바인드 마운트 : https://www.daleseo.com/docker-volumes-bind-mounts/
