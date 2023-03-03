## WebHook 설정
### WebHook
- 웹페이지 or 웹앱에서 발생하는 특정 행동(이벤트)들을 커스텀 Callback으로 변환해주는 방법
- 이러한 행동 정보들을 실시간으로 제공하는데 사용됨
- 서버에서

### callback
- 

### GitLab Plugin
- GitLab Plugin 설치
- Pipeline 구성에서 [Build Triggers] - [Build when a change is pushed to GitLab. GitLab webhook] 체크
  - 이 때, 여기서 나오는 URL이 나중에 사용되므로 기록해두기
- 하단의 [고급] - 아래의 Token [Generate]

### GitLab WebHook
![image](https://user-images.githubusercontent.com/80720210/182780083-5024c29b-c5e4-4575-80cc-cfc6a0593484.png)

- 웹훅을 생성할 프로젝트에서 [settings] - [Integrations]에서 URL과 Token을 입력후 [Add webhook]
  - Integrations가 없다면 관리자계정으로 Admin Area에서
    - [Settings] - [Integrations] - [Third party offers]에서 [Do not display offers from third parties within GitLab]을 체크 해제

- 파이프라인 생성 후 Push Test하면 해당 파이프라인에서 자동으로 Build됨
  - 안보이면 새로고침

<br/>

## SSH 설정
- docker위의 jenkins에서 내 로컬 Linux로, 또는 jenkins에서 다른 target Server로 배포할 때 사용

### SSH
- 

### SSH Key 생성
```
ssh-keygen -t rsa -b 4096 -m PEM
또는
docker exec -it [컨테이너이름] ssh-keygen -t rsa -b 4096 -m PEM
```
- 서버 로컬에 jenkins가 깔려있으면 위의 방법으로 ssh key 생성
- 서버의 docker에 jenkins가 올라가 있으면 아래의 방법으로 ssh 키 생성

```
# Private key
cat ~/.ssh/id_rsa

# Public key
cat ~/.ssh/id_rsa.pub
```
- 로컬일 때의 ~/ 경로
  - = /home/계정명
  - 루트이면 /root 
- docker일 때의 ~/ 경로
  -  = /var/jenkins_home
  -  당연히 도커는 docker exec -it [컨테이너 명] cat /var/jenkins_home/.ss/id_rsa 이렇게 사용해야 함

### Target Server 설정
- Target Server에서 SSH로 접속할 User로 접속하여 홈 디렉토리로 가기
  - .ssh/authorized_keys에 Jenkins Server의 Public key 내용 붙여넣기
    - ~/.ssh/ 디렉토리가 없으면 ``` mkdir .ssh ```로 생성하기
    - authorized_keys가 없으면 생성하면 됨

```
ssh-keygen -t rsa -b 4096 -m PEM
```

### Publish Over SSH 설정
- Jenkins Plugin 중 Publish Over SSH 설치
- [Jenkins 관리] - [시스템 설정] - 아래쪽에 Publish Over SSH 있음

![image](https://user-images.githubusercontent.com/80720210/182775038-f5f152dc-1768-4330-8494-2a2ccd26a64a.png)

- 하단의 SSH Servers에서 [추가]
- 정보 입력
  - name :
  - Hostname : target server의 ip
  - Username : 로그인할 server의 user
    - 즉, Target Server의 유저 중 .ssh/authorized_keys에 jenkins의 Public key가 작성된 유저
  - Remote Directory : 사용할 디렉토리
    - 주의 : Clean등의 명령어시 해당 디렉토리 하위의 모든 파일 삭제됨
    - 주의 : 반드시 다룰 때 문제없는 디렉토리 설정을 할 것
- [고급] - [Usepassword authentication, or use a diffrent key] 체크

![image](https://user-images.githubusercontent.com/80720210/182775255-39e7c650-304e-4186-8f07-78884c182e83.png)

- Key 부분에 Jenkns Server의 Private Key 내용을 전부 붙여넣기
  - ssh-keygen -t rsa -b 4096 -m PEM가 아닌 단순히 ssh_keygen으로 생성하면 오류남
  - Publish Over SSH가 예전 ssh_keygen 형식을 사용하고 있기 때문에 구버전으로 생성해야 함

- [Test Configuration]  Success가 나오면 끝

<br/>

## port fowarding 설정
- docker위에 jenkins가 있을 경우 필요함

### Port fowarding / Port Binding / Port Mapping

### docker container 생성
```
$ docker run -d -p 8181:8080 -p 9090:9090 -p 9092:9092 -v /var/jnekins_home:/var/jenkins_home --name jm_jenkins --user 1000 jenkins/jenkins:lts
```
- docker container 생성
- 외부에서 jenkins로는 8181 접근 가능 / 9090과 9092도 매핑한 상태
- v : 볼륨의 바인드 마운트를 위한 설정


```
$ chown -R 1000:1000 /var/jenkins_home
```
- 생성시 오류 발생한다면 => 위의 명령어로 jenkins 디렉토리에 접근 권한을 주어야 함


```
$ docker run -d -p 8181:9090 -e JENKINS_OPTS="--httpPort=9090" -v /var/jnekins_home:/var/jenkins_home --name jm_jenkins --user 1000 jenkins/jenkins:lts
```
- e : 컨테이너 내에서 사용할 환경 변수를 설정
  - 기본 8080 -> 이걸 9090으로 바꿔줌

<br/>

### 참고
- https://wiki.kicco.com/space/JEN/48267432
- https://uchupura.tistory.com/63
- https://issues.jenkins.io/browse/JENKINS-57495?page=com.atlassian.jira.plugin.system.issuetabpanels%3Aall-tabpanel
