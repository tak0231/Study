# Project(Github -> Jenkins)

## gitlab

## github
- 토큰 설정해야 함


# Build(Jeknins Agent Pod)
```
podTemplate(yaml, '''
  apiVersion: v1
  kind: Pod
  Label:
    
  spec:
    containers:
    - name: maven
      image: 
    - nanme: jnlp
      imag: 
''')


node(POD_LABEL) {
  stage("Fetch Git") {
    //  
    
  }
  
  stage("Build Maven") {
    container('maven') {
      
    }
  }
  
  stage("Create Image") {
    
  }
  
  stage("Push Image") {
    
  }
  
}

```
- 사용 안함
- 정확한 구성은 나중에 파악


```
pipeline {
  agent {
    // jenkins서 설정한 cluster명
    kubernetes {
      inheritFrom: 'agentPod',
      yaml '''
      spec : 
        containers:
        - name: maven
          image: 
        - name: jnlp
          image: 
      '''
    }
    
    stage("Fetch Git"){
      container () {
        
      }
    }
    
    stage("Build Maven"){
      
    }
    
    stage("Create Image"){
      
    }
    
    stage("Push Image to Harbor"){
      
    }
  }
}
```





## 1) 플러그인 설치 
- kubernetes 플러그인 설치
- jenkins 플러그인에서 검색해서 설치

## 2) cluster setting
- kubernetes 클러스터 정보 설정하기

# Harbor 연동

##

### 참고자료
- 소스관리
  - gitlab 플러그인 : https://pangtrue.tistory.com/356
  - github 토큰 설정 : https://goddaehee.tistory.com/258
- agent 
  - jenkins 설정 : https://www.whatap.io/ko/blog/77/
  - Kubernetes Plugin Doc : https://plugins.jenkins.io/kubernetes/
  - agent pipeline : https://waspro.tistory.com/554

- jenkins & harbor 연동 : https://zunoxi.tistory.com/131
