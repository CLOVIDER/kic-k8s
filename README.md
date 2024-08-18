<div align="center">

# Kids In Company: System ARCHITECTURE

키즈인컴퍼니 서비스의 시스템 아키텍쳐에 대해 소개합니다.
작성자: 정희찬 (anselmo228)

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2FCLOVIDER%2Fkic-backend&count_bg=%23E7E413&title_bg=%231F36A4&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

</div>

![image](https://github.com/user-attachments/assets/5cd1c9ca-3312-412c-9813-dcb69c157190)

## 목차

1. [기술 스택]
2. [배포방식과 쿠버네티스]
3. [SSH 접근]
4. [최적화한 사항](

## 💡 프로젝트 소개
>디케이테크인 내부의 공정하고 투명한 사내 어린이집 인원 배정을 위해 어린이집 모집과 추첨을 자동화한 서비스입니다.

## 🛠️ 기술 스택
<img width="559" alt="Screenshot 2024-08-18 at 7 45 10 PM" src="https://github.com/user-attachments/assets/f05610e5-703f-4bd6-a138-5929d84f7262">

DNS: 가비아 도메인(http://kidsincompany.shop/)

Container Registry: 빌드된 도커 이미지를 관리

Subnet: Private/Public Subnet으로 나누어 관리

Kubernetes Engine: 프론트와 백엔드 서비스 및 MySQL, redis와 같은 DB를 파드에서 관리

Load Balancer: 프론트, 백엔드 서비스를 Nginx Ingress와 묶어서 외부로 라우팅

Virtual Machine: Bastion, Nat, 서비스 Runner

Lambda: 추첨 확률 예측을 위한 함수 실행

AWS API GateWay: Lambda와 Spring 서비스를 연결해주는 Gateway

S3: 이미지 및 도큐먼트 저장소

ArgoCD: 서비스 배포 도구 쿠버네티스 파드에서 실행

Prometheus, Grafana: 모니터링 도구

## 📝 배포 방식과 쿠버네티스

### [Continuous Integration (CI)]

전형적인 Docker 이미지 빌드 방식을 따르고 있습니다. 빌드된 이미지는 카카오 레지스트리에 태그와 함께 업로드됩니다.
- **FrontEND**: npm 방식으로 빌드 및 이미지 업로드
- **BackEND**: JIB 방식으로 빌드 및 이미지 업로드

### [Continuous Deployment (CD)]

FrontEND와 BackEND 모두 빌드된 이미지가 레지스트리에 업로드되면, ArgoCD가 deployment.yaml 파일을 트리거하여 배포를 시작합니다.
- **ArgoCD**: 3개의 레플리카셋으로 배포
  ![Deployment](https://github.com/user-attachments/assets/09ac76d4-e586-488a-88e7-f3dd0bc7905d)

### [Git Self Runner]

Private Repository를 사용함으로 Git Actions 사용 시간에 제한이 있습니다. 이를 해결하기 위해 Kubernetes의 Pod에 ARC를 활용하여 Git Actions가 실행될 때 GitHub 내부 서버가 아닌 쿠버네티스의 서버를 사용할 수 있도록 설정했습니다.
![Git Self Runner](https://github.com/user-attachments/assets/b3c37373-3ca8-41b6-be71-8548c1fd1f04)

### [롤링 배포]

- **점진적 배포**: 새로운 버전의 파드를 하나씩 또는 일정 수만큼 기존 버전의 파드와 교체합니다. 한 번에 하나의 파드가 새로운 버전으로 대체되며, 그 파드가 준비 상태가 되면 다음 파드를 교체합니다.
- **서비스 가용성 유지**: 롤링 배포 중에도 항상 일정 수의 파드가 실행되기 때문에 서비스의 중단이 없습니다.
- **자동 복구**: 배포 중 문제가 발생하면 쿠버네티스가 자동으로 이전 안정적인 상태로 되돌릴 수 있습니다.

### [SSH 접근과 Bastion]

쿠버네티스 접근 또는 서비스 내 VM으로 접근할 때 모든 포트를 열어놓지 않고, Bastion 방식을 통해 특정 IP와 포트로만 접근이 가능하도록 설정했습니다.

## 최적화 사항

- **JIB 빌드를 통한 빌드 시간 77% 단축**
- **JRE 이미지 빌드를 통한 이미지 1.8배 경량화 및 보안성 강화**
    - [참고 링크](https://github.com/CLOVIDER/kic-backend/pull/120)
- **Git Self Runner 사용으로 기존 제한 시간이 있던 Git Actions를 제한 없이 사용 가능하게 함**
- **Bastion 사용으로 보안 강화**

