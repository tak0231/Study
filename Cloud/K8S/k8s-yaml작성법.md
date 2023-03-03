# kind: Deployment
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

## labels
- .metadata.labels
- 나중에 다른 리소스에서 이 Deployment를 참조하려할 때 사용하기 위해 지정하는 값
	- 예를들면 service의 .spec.selector에서 서비스와 연결하 Deployment 지정할 때 여기서 정의한 "app: nginx"를 적으면 됨


## selector
- Deployment가 관리하 Pod를 관리할 방법을 정의함
	- Deployment가 관리할 Pod를 지정할 때 사용하느 듯 보임
- 여기서 .spec.selector.matchLabels의 app
	- .spec.template.metadata.labels의 app을 참조하는 것

## template
- Pod Template
- .template.metadata.labels를 통해서 pod에 label(레이블)을 붙음 
- .template.spec을 통해 Pod에 올릴 컨테이너 정의





### 참고자료
- deployment.yaml 작성법 : https://kingofbackend.tistory.com/165