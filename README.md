# K8s ToyProject
<img width="811" height="651" alt="Image" src="https://github.com/user-attachments/assets/f58b2bb3-3f4e-4e20-960b-a000892a039a" />

이 프로젝트는 Kubernetes 기반 온프레미스 인프라를 구축하고 Argocd를 통해 자동 배포까지 완성하는 과정을 설명합니다. 사용된 주요 노드는 다음과 같습니다:

- K8S Cluster: master1 node1, node2, node3
- dnf-dns (NFS + DNS 서버), harbor (프라이빗 이미지 레지스트리)

## Kubernetes 클러스터 구성
- 마스터-워커 구조로 클러스터 구축  
- Pod, Node, Service 등 Kubernetes 리소스를 관리
- Calico를 네트워크 플러그인으로 사용

## LoadBalancer + Ingress 설정
- 외부 트래픽 수신을 위한 LoadBalancer 구성
  - MetalLB, NGINX Ingress Controller 설치 
- Ingress를 통해 경로별 서비스 라우팅:
  - `/shop` → Shop 서비스  
  - `/news` → News 서비스  
  - `/blog` → Blog 서비스  

## Harbor 레지스트리 구축
- 사설 레지스트리 구축으로 컨테이너 이미지 관리  
- 인증 및 접근 제어 가능  
- ArgoCD에서 이미지 Pull 가능  

## 사설 DNS 서버 구축
- 도메인 `www.aws9.pri` 사설 DNS로 관리  
- Ingress 경로(`/shop`, `/news`, `/blog`)와 연동  

## NFS + CSI 기반 동적 볼륨 구성
- /shared 경로를 NFS로 지정
- NFS 서버와 CSI를 이용한 동적 볼륨 프로비저닝
- nfs-csi-driver 설치

## ArgoCD를 통한 자동 배포
- Argocd Namespace에 argocd 설치
- Git 저장소와 연동하여 선언형 배포 수행  
- 서비스별 경로 트래픽에 따라 자동 배포  

## KEDA + HPA 오토스케일링 구성
- 트래픽 상황에 따라 자동 확장  
- KEDA: 이벤트 기반 스케일링
 - Cron을 통해 예약된 시간에 Pod 3개로 늘어나게 정의
- HPA: CPU/메모리 기반 스케일링  
