## GCP Settings

### VM instance 생성

### ssh 설정

- ssh key 생성
```
# CMD
ssh-keygen -t -rsa -f C:\Users\{Windows_user}\.ssh\{KEY_FILENAME} -c {USERNAME} -b 2048
```
ssh-keygen -t rsa -f key파일경로 -C 이메일전체 -b 2048


- KEY_FILENAME
	- my-key 같은 그냥 확장자 없이
- UserName
	- gmail 계정
	- 이 방법으로는 root에 연결할 수 없음
	
- mobaXterm, putty는 아래 참고자료 확인

### root 패스워드 변경
```
sudo passwd
```

### 패키지 업데이트
```
sudo apt update -y && sudo apt upgrade -y
```


### 참고자료
- GCP VM 인스턴스 생성 : https://aoc55.tistory.com/51
- docker 설치 : https://aoc55.tistory.com/53
- kubernetes 설치 : https://aoc55.tistory.com/54
- kubernetes master node init 관련 : https://futurecreator.github.io/2019/02/25/kubernetes-cluster-on-google-compute-engine-for-developers/
- ssh key 생성 : https://cloud.google.com/compute/docs/connect/create-ssh-keys?hl=ko&_ga=2.222143960.-1380832433.1675674391#windows-10-or-later
- putty GCP 접속 : https://gusrb.tistory.com/51
- mobaXterm GCP 접속 : https://gusrb.tistory.com/53
