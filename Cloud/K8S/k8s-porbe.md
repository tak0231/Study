# Probe
- kubelet에 의해 주기적으로 실행되는 진단
  - 이를 통해 문제가 있는 컨테이너를 자동으로 재시작 or 서비스에서 제외시킬 수 있음

- kubelet -> 상태를 진단함
  - 진단하기 위해 핸들러를 호출함
  - 핸들러 종류는 크게 3가지 (수행하는 작업에 따라 분류)
    - ExecAction
    - TCPAction
    - HttpGetAction
   
- 진단 결과는 3가지 중 하나
  - Success (진단 통과)
  - Failure (진단 실패)
  - Unknown (진단 실패, 동작 수행 X)


## Handler
- 컨테이너 상태 진단 위해 어떻게 진단할지를 명시한 것

### ExecAction
- 컨테이너에서 지정된 명령어를 실행함
- 실행했을 때 exit code로 성공, 실패를 분류함
  - code 0 => 성공
  - 나머지 => 실패
```
exec
  command:
  - cat
  - /etc/nginx/nigx.conf
```

### TCPAction
- 지정된 포트로 TCP 소켓 연결을 시도함

```
tcpSocket:
  port: 8080
  initialDelaySeconds: 15
  periodSeconds
```

### HttpGetAction
- 지정된 포트로 url로 HTTP Get 요청을 전송함
  - 응답 : 200~400 => 성공
  - 그외 : 실패
```
httpGet:
  path: healthz
  port: liveness-port
```

## Probe 종류
- kubelet은 실행중인 컨테이너에 대해 3가지 프로브를 진행할 수 있음

### Liveness Probe
- 컨테이너가 제대로 동작중인지를 검사하는 Probe
-  

### Readiness Probe
- 컨테이너가 요청 처리 준비가 되었는지 확인하는 Probe

### Startup Probe
- 컨테이너 내 애플리케이션이 시작되었는지에 대한 Probe


### 참고자료
- (반드시 참고) Probe 1 : https://velog.io/@hoonki/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-Probe
- Probe 2 : https://zunoxi.tistory.com/92
