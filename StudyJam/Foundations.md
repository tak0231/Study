# Cloud Computing and Google Cloud
- 클라우드 컴퓨터에는 근본적인 5가지 속성이 존재함

<br/>

1) on-demand self-service
- **on-demand** 
  - 지금 바로 수요구(수요에) 응하는 것 / 주문형
  - 요구사항에 따라 즉시 제공, 공급하는 방식
- 주문형 자가 서비스 -> Customer가 컴퓨팅 기능을 provisioning 할 수 있음
- 서비스 제공자의 개입 없이 컴퓨팅 기능을 provisioning 할 수 있음
- **프로비저닝**
  - 사용자의 요구에 맞게 시스템 자체를 제공하는 서비스
  - 어던 지식, 자원 등을 미리 준비하고 -> 요청이 들어오면 요청에 맞게 공급하는 것

<br/>

2) Broad network access
- 폭 넓은 네트워크 액세스
- 어디서든 리소스에 접근 가능함
- 여러 종류의 think, thick 클라이언트 플랫폼에서 사용을 촉진하는 표준 메커니즘을 통해 액세스할 수 있음
  - **Thin Client** : 주로 서버와 통신하도록 설계된 소프트웨어 (크롬, 사파리 웹 브라우저)
  - **Thick Client** : 자체 기능을 구현하는 소프트웨어 (ex.게임) / 서버에 연결도 가능, 끊어져도 기능 수행 가능함

<br/>

3) Resource pooling
- **리소스 풀링**
  - 서버나 스토리지 등의 자원을 미리 확보하고 -> 사용자 요청에 따라 제공한다는 개념
  - 혹은 이 확보해 놓은 가상적 공간 자체를 의미
- Provider가 Customer에게 리소스를 공유할 수 있음
  - Customer들은 정확하게 물리적 공간에서 무슨 일이 일어났는지 모름 (알 필요도 없고)
- **Multi-tenant 모델**을 사용
  - 단일 소프트웨어 인스턴스
  - 서로 다른 여러 사용자 그룹에 서비스 제공할 수 있는 소프트웨어 아키텍쳐를 의미
  - Saas 제품이 멀티테넌트 아키텍쳐의 예시

<br/>

4) Rapid elasticity
- 빠른 탄력
- 기능이 탄력적으로 프로지버닝 & 릴리즈 될 수 있음
- 경우에 따라 수요에 맞춰 신혹한 확장 or 축소 될 수 있음
- 즉, 수요에 따라 Customer는 리소스를 탄력적으로 공급받을 수 있음

<br/>

5) Measured Service
- 측정되는 서비스
- Customer는 사용량 만큼만 비용을 제공하면 됨
- 리소스 사용을 자동으로 제어가능하며, 리소스 사용량의 모니터링, 제어, 보고 등이 가능함
- Provider 와 Customer 모두에게 투명하게 공개됨


<br/>

## Google Cloud 에서 제공하는 서비스

### Compute Engine
- 구글 클라우드의 IaaS (Infrastructure as a service)
- 구글 인프라에서 가상 머신을 만들고 실행할 수 있는 맞춤 설정 가능한 컴퓨팅 서비스
- 요구사항에 맞춰 VM을 제공함

### GKE (Google Kubernetes Engine)
- 클라우드 환경에서 컨테이너화된 애플리케이션을 돌릴 수 있도록 해줌
- Compute Engine 위에 올려져 있음

### App Engine
- 구글 클라우드 플랫폼에서 완전히 관리되는 PaaS
- GCP에서 완전히 관리된다 = 인프라에 대한 걱정 없이 코드를 클라우드 상에서 돌릴 수 있음을 의미
- 코드에만 집중할 수 있게 됨
  - 구글에서 프로비저닝과 리소스 관리를 맡음

### Cloud Function
- 서버가 필요없는 실행 환경 or 서비스로서의 기능
- 어떤 이벤트에 대한 응답으로 사용자의 코드를 실행시킴


## Database 관련
- DB를 Compute Engine 위에 설치 할 수 있음
- or GKE에 설치할 수도 있음
- or 완전히 구글이 DB와 스토리지를 관리하게 맡길 수도 있음

- GCP는 관계형 / 비관계형 DB를 모두 제공함
- 빅데이터 학습 서비스도 제공함

<hr/>

### 참고
- 클라우드 컴퓨팅 관련 개념 : https://psyhm.tistory.com/50?category=654719
