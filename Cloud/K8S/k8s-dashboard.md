# Role
```
>> kubectl get clusterrole
```
- k8s의 Role 목록 확인

```
>> kubectl describe clustrrole kubernetes-dashboard
```
- 특정 Role에 대한 내용 확인
- 여기서 예시로 든 Role이 dashboard의 Role	

- 관리자 토큰 생성을 위해 yaml을 이용하여 계정을 생성해야 함
	- 대부분 Cluster-admin role을 부여하고, ClusterRoleBinding을 생성함
	
	
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
```
- ServiceAccount 생성

```
apiVersion: v1
kind: Secret
metadata:
  name: admin-user-secert
  namespace: kubernetes-dashboard
  annotations:
    kubernetes.io/service-account.name: "sa-name"
type: kubernetes.io/service-account-token
data:
  # You can include additional key value pairs as you do with Opaque Secrets
  extra: YmFyCg==
```
- secret 생성 및 ServiceAccount에 연결

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```
- ClusterRoleBinding을 생성
- 그리고 역할에 cluster-admin을 부여함

```
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
```
- secret 출력


### 참고자료
- ROle 생성1 : https://may9noy.tistory.com/343
- Role 생성2 : https://jaynamm.tistory.com/entry/9-%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-%EB%8C%80%EC%8B%9C%EB%B3%B4%EB%93%9C-%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0
- rbac로 service account 권한 설정 : https://blog.dudaji.com/kubernetes/2019/05/01/k8s-authorization-of-sa-with-rbac.html

- serviceAccunt Secret 관련 1 : https://stackoverflow.com/questions/72251976/how-to-get-token-from-service-account
- serviceAccount Secret 관련 2 : https://stackoverflow.com/questions/72256006/service-account-secret-is-not-listed-how-to-fix-it
- serviceAccount Secret 관련 3: https://kubernetes.io/docs/concepts/configuration/secret/#service-account-token-secrets
	