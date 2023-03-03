### 인터넷
- WSL2 -> 로컬에서 이더넷 어댑터를 통해 연결되어 있음
	- 즉, 로컬에만 연결, 외부와는 연결되지 않음
	- 별도의 설정 필요
	
- WSL2 서버 동작시 다음과 네트워크 연결은 다음과 같음
	- 로컬 내부 IP (192.168.0.10) -> WSL2의 어댑터 주소 (172.22.48.1)
	
- 로컬 환경을 외부에서 접근하기 위해서는 port foward 작업 필요
	- 외부에서 WSL로 접근하기 위해서도 Port foward 필요
	- 로컬 외부 IP -> 로컬 내부 IP -> WSL2 어댑터 주소


### IP
- WSL => shutdown시, host 리부트시마다 IP 주소 변경됨
- WSL2는 재부팅 과정에서 IP 주소를 변경함
- 때문에 ubuntu 설정으로 정적 ip 설정 X

- 대신, script 파일을 생성하여 부팅시마다 wsl2의 ip를 연결하는 방식으로 고정시킬 수 있음
	- 정확히는 포트포워딩 방식


```
$remoteport = bash.exe -c "ifconfig eth0 | grep 'inet '"
$found = $remoteport -match '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}';

if( $found ){
  $remoteport = $matches[0];
} else{
  echo "The Script Exited, the ip address of WSL 2 cannot be found";
  exit;
}

#[Ports]

#All the ports you want to forward separated by coma
$ports=@(80,443,10000,3000,4000,5000);


#[Static ip]
#You can change the addr to your ip config to listen to a specific address
$addr='0.0.0.0';
$ports_a = $ports -join ",";


#Remove Firewall Exception Rules
iex "Remove-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' ";

#adding Exception Rules for inbound and outbound Rules
iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Outbound -LocalPort $ports_a -Action Allow -Protocol TCP";
iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Inbound -LocalPort $ports_a -Action Allow -Protocol TCP";

for( $i = 0; $i -lt $ports.length; $i++ ){
  $port = $ports[$i];
  iex "netsh interface portproxy delete v4tov4 listenport=$port listenaddress=$addr";
  iex "netsh interface portproxy add v4tov4 listenport=$port listenaddress=$addr connectport=$port connectaddress=$remoteport";
}
```
- 스크립트 내용
	1) WSL 2 시스템의 IP 주소 가져오기
		- 매번 변경되는 WSL 내부의 IP 주소를 가져오기 위함
	2) 이전 포트 전달 규칙 제거 			(이전의 포트포워딩 룰 제거)
	3) 포트 전달 규칙 추가				(포트포워딩 룰 추가)
	4) 이전에 추가한 방화벽 규칙 제거
	5) 새 방화벽 규칙 추가

- 스크립트 실행시 오류 Excution-Policy 관련 오류 발생할 수 있음
	- Powershell 에서 Get-ExcutionPolicy 실행시 Resctricte가 뜨면 그런 경우
	- Set-ExcutionPolicy RemoteSigned로 변경해주면 됨
	
- 그리고서 사용자가 로그인할 때마다 Script 실행하도록 변경해주면 됨
	- 참고자료의 "wsl2 고정 IP 2" 확인
	
- 즉, 매번 변경되는 wsl2의 IP 정보를 가져와서, 사용하고자 하는 port를 포트포워딩 룰에 추가시켜두는 것
	- ex. 
		- 80, 443 등을 열어두면 외부에서 내부로의 접근 가능해짐
		- 내부에 8080으로 설정해둔 톰캣이 있다면, 외부에서 "wsl2의 IP:8080/~~ " 이렇게 접근 가능
		- 즉, docker -p 옵션을 윈도우의 wsl2에 적용시킨거라 이해하면 될듯

## systemctl
- WSL2에서 systemctl 명령 실행시 아래 오류 발생
```
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down
```
- 루트 시스템 프로세스의 차이 때문에 발생하는 오류
	- 리눅스의 경우 systemd를 사용하는 추세
	- but WSL의 경우 init을 사용
	- 때문에 service나 systemctl 사용시 오류가 발생하는 것

- 대신 기존에 service 명령어는 사용 가능
```
>> service ssh restart
```

### init vs systemd



### 참고자료
- wsl -> d 드라이브로 전환 : https://otrodevym.tistory.com/entry/WSL-D%EB%93%9C%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%A1%9C-%EC%84%A4%EC%B9%98%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95

- wsl2 고정 IP 1 : https://blog.dalso.org/linux/wsl2/11430
- wsl2 고정 IP 2 : https://hou27.tistory.com/entry/WSL2-%EC%A0%95%EC%A0%81-ip-%ED%95%A0%EB%8B%B9%ED%95%98%EA%B8%B0
- wsl2 고정 IP 3 : https://gareen.tistory.com/73
- wsl2 외부 네트워크 연결하기 : https://codeac.tistory.com/118
- Excution-Policy 오류 관련 : https://blog.dalso.org/it/11432
- WSL2 systemctl 동작 안함 : https://www.sysnet.pe.kr/2/0/13104?pageno=16
- init vs systemd : https://mamu2830.blogspot.com/2021/07/init-systemd-difference.html
