# Ingress
- 외부 -> 클러스터 접근할 때 하는 요청들을 어떻게 처리할지 정해둔 규칙
- 이런 규칙을 모아둔 자원이 "Ingress"
- 제공하는 기능
  - URL (고유 주소 사용)
  - 트래픽 로드밸런싱 (트래피에 대한 L4, L7 로드밸런서)
  - SSL 인증서 처리 (보안 인증서 처리)
  - 도메인 기반 가상 호스팅 기능 등

# Ingress Controller
- 설정해둔 규칙들(Ingress)를 실제로 동작시키는
- 종류
  - Nginx Controller (ingress-nginx)
 
- 클라우드 서비스들은 자체 로드밸런서 서비스를 가짐 (AWS EKS 등)
- 직접 클라우드를 구축한 경우 -> 인그레스 컨트롤러를 직접 인그레스와 연동해야 함



### 참고자료
- 인그레스 개요 : https://kimjingo.tistory.com/m/163
-  (공식) nginx-ingress-controller : https://github.com/kubernetes/ingress-nginx#support-versions-table
-  
