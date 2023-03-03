## 명령어
```
$ docker -v
```
- 도커 엔진의 버전 확인

```
$ docker pull [이미지이름]:[이미지버전]
```
- 도커 허브에서 이미지를 내려받을 수 있음

```
$ docker images
```
- 현재 내려받은 이미지들을 확인할 수 있음

```
$ docker create -i -t --name [컨테이너 이름] [이미지 이름]:[이미지버전]
```
- 도커 이미지로 컨테이너를 생성할 수 있음
- i : 상호 입출력 옵션
- t : tty 활성화 옵션 -> bash셸을 사용하도록 container에 설정함
- name : 컨테이너 이름을 명시해주는 옵션 / 없을경우 무작위 16진수 해쉬값이 지정됨 (너무길어짐)
- 이 둘이 있어야 셸 사용이 가능함(하나라도 없으면 X)

```
$ docker inspect
```
- 도커 컨테이너의 ID를 확인할 수 있음

```
$ docker start
```
- 생성된 도커 컨테이너를 실행 (단순히 실행만 함)

```
$ docker attach[컨테이너 이름]
```
- 생성된 컨테이널르 실행 + 접속

```
$ docker run -i -t [이미지이름]:[이미지버전]
```
- 컨테이너 생성 + 실행 + (가능하면) 접속 (create + start+atach) 

### exit 또는 ctrl+D
- 컨테이너 빠져나오기 (나오면서 컨테이너 정지됨)

### ctrl+ P,Q
- 컨테이너 빠져나오기(컨테이너 정지 안됨)

```
$ docker ps
```
- 실행중인 컨테이너 

<br/>

### 참고
- docker 명령어  : https://velog.io/@hanif/%EB%8F%84%EC%BB%A4-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-%EB%8B%A4%EB%A3%A8%EA%B8%B0
