# 유용한 명령어 및 명령어 설명
- 참고로 옵션 = flag (-f, --record 등...)

## apply -f
- f = File

## watch 사용
```
>>> watch -n 1 kubectl get po XXX
```
- 1초마다 get pod 실행
	- watch command 응용한 것

## -w 옵션
```
kubectl get po -n mytest -w
```
- watch랑 같음

## -o yaml 옵션
```
>> kubectl get po XXX -o yaml > XXXX.yaml
```

- yaml 파일로 출력
- ```> XXXX.yaml``` 라는 리눅스 명령어 통해 파일로출력 가능

## cordon / uncordon
```
>> kubectl cordon node명
>> kubectl uncordon node명
```
- cordon
	- 특정 노드에 스케쥴링 되는 것을 막음
	- cordon 실행된 노드는 SchedulingDisabled 상태가 됨
- uncordon
	- cordon 해지

## drain
```
>> kubectl drain k8s-w3 --ignore-daemonsets
```
- drain이 적용된 node는 SchedulingDisalbed 상태가 됨
- but DaemonSet Controller가 있다면 오류 발생할 수 있음
	- DeamonSet은 모든 노드에 (특정 노드일수 있지만) Pod가 올라가 있는 것을 보장하기 때문
	- 때문에 사전에 DeamonSet을 무시하고 진행시킬 수 있도록 옵션을 주는 것
		- DaemonSet


## custom-columns
```
>> kubectl get po -n kube-system -o custom-columns=Name:.metadata.name,Node:.spec.nodeName,Kind:.metadata.ownerReferences[0].kind

```
- key, value는 key를 타고 들어가면 됨
- map은 index 지정해주고 key 검색하면 됨
- 원하는 항목의 계위를 모르겠으면 '-o wide'로 파악하면 됨


## --record 옵션 (flg)
```
>> kubectl apply -f rollout-nginx.yaml -n mytest --record
```
- 특정 명령어를 수행한 시점을 기억해둠
- 배치를 발행한 명령어를 기억해둠
	- 주로 Pod, Deployment 등의 오브젝트 관련된 내용 실행시 저장하는 용도
	- 리소스 변경, spec 수정 등...
- 어떤 kubectl 명령어를 수행했는지 기록해둠
	- get 등에는 먹히지 않는 flag

- 배포 등을 진행할 때 실수를 방지하기 위해서 --record 옵션을 사용하는 것

## rollout
- 여러개의 Pod를 모두 죽이지 않고 순차적으로 업데이트 하는 방식
- 롤백 등에 사용됨

### rollout status
```
>> kubectl rollout status deployment rollout-nginx -n mytest
```
- 현재 rollout 진행 상태를 보여줌
 
### rollout history
```
>> kubectl rollout history deployment rollout-nginx -n mytest
```
- record로 기록된 히스토리를 보여줌

### rollout undo
```
>> kubectl rollout status deployment rollout-nginx
```

### rollout undo --to-revision
```
>> kubectl rollout undo
```

## edit
```
>> kubectl edit pods rollout-nginx-55fbd876df-9gcfx -n mytest
```
- kubectl edit {object_type} {resource_name}
- 특정 오브젝트의 내용을 (yaml) 편집할 수 있음

## set image
```
>> kubectl set image deployment rollout-nginx nginx=nginx:1.17.23 --record
```
- kubectl set image deployment ${deployment_name} ${prev_image}=${next_image}
- deployment가 보유한 pod의 이미지를 변경함


## logs
```
>> kubectl logs ${pod_name}
```
- pod log 확인 가능


## delete --all
```
>> kubectl delete deploymet, pod, rs, --all
```
- 모든 리소스에 적용


## explain
```
>> kubectl explain Service
>> kubectl explain AppProject
```

- object나 resource 관련 도움말을 볼 수 있음
	- ex. Service Yaml 작성시 apiVersion
- yaml 작성시 필수 입력값(requirements)도 확인 가능

- kubernetes API Server에 등록된 것들이면 모두 가능
	- ex. argocd의 Object인 Application이나 AppProject도 가능


# 추가 개념

## 컨트롤러

### DaemonSet
- 클러스터 전체 노드에 특정 파드 실행할 때 사용하는 컨트롤러
- Deployment와 유사
	- Deployment : RollingUpdate, 배포 일시중지, 재개 등 배포 작업을 좀 더 세분화하여 조작할 수 있음
	- DaemonSet : 특정 노드 or 모든 노드서 실행되야하는 "특정 파드" 관리에 특화됨
- DaemonSet은 별도로 replica를 지정하지 않음
	- 모든 Node에 한개씩, 또는 특정 label을 가진 노드에 각각 한개씩 모두 배포함 (중요한 차이)
	- Deployment는 replica를 지정함

- 주로 로그수집, 모니터링 목적의 Pod에 사용됨
	- 클러스터 전체적으로 감시할 때
	

- 배포전략 (업데이트 정책)
	- OnDelete 
		- DaemonSet의 기본 정책
		- DaemonSet를 Update 후 수동으로 이전의 Pod를 지워야 함
		- 즉, 직접 파드를 지워야만 Update가 됨
	- RollingUpdate
		- k8s v1.6 이상에서는 RollingUpdate도 지원
		- 롤백도 지원 (지원 버전은 확인 필요)

### ReplicaSet
- 지정한 수만큼의 pod를 보장
- 집합 기반의 Selector를 지원
	- Selector는 in, notin, exists 같은 연산을 지원
	- 연산 조건에 따라 필요로하는 레이블을 선택 가능한
- 단, Update시 RollingUpdate 불가
	- Recreate는 가능

### Deployment
- 여러 배포전략(ex. Roluung Update), 롤백 지원
	- 배포중지, 재개 등 가능
	
	
### 참고자료
- Drain,Cordon, UnCordon : https://velog.io/@koo8624/Kubernetes-Drain-Cordon-and-Uncordon
- DaemonSet 1 : https://kimjingo.tistory.com/134
- DaemonSet 2 : https://www.tencentcloud.com/ko/document/product/457/30664
- Deployment와 RelicaSet : https://nearhome.tistory.com/106
- Deployment vs DaemonSet : https://nirsa.tistory.com/138
- Controller 관련 : https://potato-yong.tistory.com/119
- 쿠버네티스 워크로드 리소스: https://seongjin.me/kubernetes-workloads/ 
- 쿠버네티스 명령어 정리 : https://velog.io/@hanblueblue/kubernates-kubectl-%EB%AA%85%EB%A0%B9%EC%96%B4-%EC%A0%95%EB%A6%AC
- explain : https://kimjingo.tistory.com/m/204
