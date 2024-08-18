<div align="center">

# Kids In Company: System ARCHITECTURE

키즈인컴퍼니 서비스의 시스템 아키텍쳐에 대해 소개합니다.
작성자: 정희찬 (anselmo228)

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2FCLOVIDER%2Fkic-backend&count_bg=%23E7E413&title_bg=%231F36A4&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

</div>

![image](https://github.com/user-attachments/assets/5cd1c9ca-3312-412c-9813-dcb69c157190)

## 목차

1. [기술 스택](#기술-스택)
2. [배포방식과 쿠버네티스](#ERD)
4. [최적화한 사항](#최적화한-사항)

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


## 📝 배포방식과 쿠버네티스

[CI]

전형적인 Docker image Build 방식을 따르고 있습니다. 빌드된 이미지는 카카오 레지스트리에 tag와 함께 올라가고 있습니다.
FrontEND: npm 방식으로 빌드 및 이미지 업로드
BackEND: JIB 방식으로 빌드 및 이미지 업로드

[CD]

FrontEND, BackEND 동일하게 빌드된 이미지가 레지스트리에 올라가면 ArgoCD에서 Trigger(deployment.yaml)를 받아 
ArgoCD에 replicaset 3개로 배포
<img width="1031" alt="Screenshot 2024-08-18 at 8 48 18 PM" src="https://github.com/user-attachments/assets/09ac76d4-e586-488a-88e7-f3dd0bc7905d">

[Git Self Runner]

Public Repository가 아닌 Private Repository를 사용함으로써 Git Actions 사용시간에 제한이 걸려있음.
때문에 Kubernetes의 Pod에 ARC를 활용하여 Git Actions가 돌때 깃허브 내부 서버가 아닌 쿠버네티스의 서버를 사용할 수 있도록 함.

![image](https://github.com/user-attachments/assets/b3c37373-3ca8-41b6-be71-8548c1fd1f04)

[롤링 배포]

점진적 배포: 새로운 버전의 파드를 하나씩 또는 일정 수 만큼씩 기존 버전의 파드와 교체합니다. 한 번에 하나의 파드가 새로운 버전으로 대체되면 그 파드가 준비 상태가 되었을 때 다음 파드가 교체됩니다.

서비스 가용성 유지: 롤링배포 중에도 항상 일정 수의 파드가 실행되고 있기 때문에 서비스의 중단이 없습니다.

자동 복구: 만약 배포 중 문제가 발생하면 쿠버네티스가 자동으로 이전 안정적인 상태로 되돌릴 수 있습니다.

## 최적화한 사항

* JIB 빌드를 통한 빌드 시간 77% 단축
* JRE 이미지 빌드를 통한 이미지 1.8배 경량화 및 보안성 강화
    * https://github.com/CLOVIDER/kic-backend/pull/120

