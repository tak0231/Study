# Metrics-server
- 각 노드에 설치된 kubelet을 통해 node, pod의 CPU, MEmory 사용량 Metrics(측정 항목)를 수집

# 오류 1 : Readiness probe failed: HTTP probe failed with statuscode: 500

```
spec:
      containers:
      - args:
        - --cert-dir=/tmp
        - --secure-port=443
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
        - --kubelet-use-node-status-port
        - --metric-resolution=15s
        - --kubelet-insecure-tls
```
- Deployment metrics-server의 .spec.container.args[0]에 내용 추가하기
  - 맨 마지막줄에 '--kubelet-insecure-tls'만 추가하면 

# 오류 2 : the server is currently unable to handle the request
- metrics-server -> kube-api-server와 https 통신을 통해 노드/파드의 리소스를 얻음
- but 추측

### 참고자료
- (공식) Metrics-server : https://github.com/kubernetes-sigs/metrics-server
- Metrics-server란 : https://potato-yong.tistory.com/150
- Metrics-server 설치시 오류들 : https://gurumee92.tistory.com/323?category=977986
