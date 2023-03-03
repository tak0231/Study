### docker jenkins 설치 & 컨테이너 띄우기
```
$ docker pull jenkins/jenkins:lts
$ docker run -d -p 8181:8080 -p 9090:9090 -v /var/jnekins_home:/var/jenkins_home --name jm_jenkins -u jenkins jenkins/jenkins:lts
```
- d : detached mode (백그라운드 모드)
- p : 포트 연결 / 포트 포워딩 / 매핑 (외부에서 접속할 포트 : 컨테이너 내부 포트)
- v : 디렉토리 설정 (새로연결할디렉토리:본래설치되는디렉토리)
  - 이 옵션을 설정해주어야 컨테이너를 remove하고 다시 컨테이너를 띄우더라도 데이터가 남아있음 
- name : 컨테이너 이름 설정 (설정 x시 16자리 무작위 hash 코드가 됨)
- u : 사용할 유저
- 뒤에는 이미지

```
$ chown -R 1000:1000 [localhost에 저장할 경로]
= $ chown -R 1000:1000 /var/jenkins_home
```
- 다음과 같은 권한 문제 발생시 설정해주어야함
  - touch: cannot touch '/var/jenkins_home/copy_reference_file.log': Permission denied

### docker gitlab 설치 & 컨테이너 띄우기

```
$ docker pull gitlab/gitlab-ce:latest
$ docker run --detach \
             --hostname 127.0.0.1 \
             --publish 443:43
             --publish 8999:80
             --publish 2222:22
             --name gitlab
             --restart always
             --volume /srv/gitlab/config:/etc/gitlab
             --volume /srv/gitlab/logs:/var/log/gitlab
             --volume /srv/gitlab/data:/var/opt/gitlab            
```
- detach : 백그라운드 실행
- hostname : Git의 호스트 이름 / clone시 이용됨 / = 컨테이너 접속시의 host의 이름
  - root@hostname -> 이부분을 변경해 주는 것이기도 함
- publish : 포트 포워딩 / 매핑
  - gitlab에서 사용되는 포트는 3가지
  - 443 : HTTPS
  - 80 : HTTP
  - 22 : SSH
- name : 컨테이너의 이름 지정
- restart : docker 실행시마다 컨테이너를 같이 띄워주는 옵션
- volume :  데이터가 저장되는 곳을 설정

- 포트 변경하였으므로 config 설정이 필요함
```
$ docker exec -it gitlab /bin/bash
``` 
- gitlab 컨테이너 접속
- exec : docker 안쪽에 명령어를 전달할 때 사용
- /bin/bash bin/bash : 도커 컨테이너 안쪽의 bash 쉘이 실행
-i :  표준입출력 STDIN를 열겠다는 의미
  - 그냥 i만 주면 빈 화면이 출력됨
  - 명령어는 제대로 수행됨
-t : 가상 tty(pesudo tty)를 통해 접속하겠다는 의미
  - 프롬프트의 모양을 바꿔줌
  - root@134adb2ba12:~/$ 와 같이

```

```

<br/>

### 참고
- docker jenkins 설치시 주의점 : https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=jyy3k&logNo=222004827128
- docker gitlab 설치 : https://blog.naver.com/PostView.nhn?blogId=dejavuhyo&logNo=222262637582&parentCategoryNo=&categoryNo=60&viewDate=&isShowPopularPosts=true&from=search
