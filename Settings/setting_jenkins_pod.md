** spec **
- ubuntu : 20.04

# pv 생성시
- 특정 노드(ex. k8s-w1, k8s-w2 등) 사용시
  - local과 nodeAffinity를 이용하여 특정 노드의 스토리지를 사용 가능

## nfs 사용하는 경우
- hostpath 사용시
  - pod가 생성된 host의 path를 사용
  - but 내 경우엔 master node에 jenkins-volume이 존재
  - 때문에 nfs로 접근 시도

- nfs

### 참고자료
- nfs-server : https://blog.dalso.org/article/ubuntu-nfs-install
- nfs-common : 
- nfs-service 재시작 : https://flower0.tistory.com/313
- access denied by server while mounting1 : https://hbesthee.tistory.com/1590
