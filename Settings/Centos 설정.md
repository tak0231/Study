## VM ware 설정
### single~~
- 보다 나은 옵션


## 네트워크 설정
- 초기 설치시 네트워크 연결되어 있지 않음

```
$ ping 8.8.8.8
```
- Network is unreachable 이라 뜨면 연결 안된 것

```
$ nmcli d
```
- 현재 기기의 네트워크 연결 여부를 확인할 수 있음
- nmcli => NetworkManager cli
- networkManager => 네트워크를 제어, 관리하는 데몬

- 내 장치인 ens33가 현재 연결 안되어 있음


```
$ vi /etc/sysconfig/network-scprits/ifcfg-ens33
```
- ~/network-scrpits 경로 하위의 ifcfg 파일을 수정해 주어야 함 (뒤에는 장치 이름)

```
# ifcfg-ens33 수정할 내용
ONBOOT = Yes
```
- 부팅시 Network를 기동할지 여부

```
$ systemctl restart network
```
- OS를 재부팅 시켜주면 네트워크 설정 끝


## 외부 접속을 위한 설정
- 참고 : https://net-gate.tistory.com/73
- 참고 : https://blog.naver.com/PostView.naver?blogId=bongss2&logNo=220830879059&categoryNo=6&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView
- 참고 : https://nirsa.tistory.com/15


### root 접속 허용
```
$ vi /etc/ssh/sshd_config

# sshd_config
PermitRootLogin=yes
```
- PermitRootLogin의 주석을 풀어준뒤 yes로 변경해주면 root로 로그인이 가능해짐

### 포트 포워딩 설정
- pro의 경우 Network Editor가 내장되어 있음
- player의 경우 vmnetcfg 툴을 활용해야 함


- 설명 추가


### openSSH 설치
```
$ yum install openssh*
```
- 설치 완료 후 재부팅

```
$ /etc/init.d/sshd restart
```

```
```


###


<br />

### 참고
-

