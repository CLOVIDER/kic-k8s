<div align="center">

# Kids In Company: System ARCHITECTURE

키즈인컴퍼니 서비스의 시스템 아키텍쳐에 대해 소개합니다.
작성자: 정희찬 (anselmo228)

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2FCLOVIDER%2Fkic-backend&count_bg=%23E7E413&title_bg=%231F36A4&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

</div>

![image](https://github.com/user-attachments/assets/5cd1c9ca-3312-412c-9813-dcb69c157190)

## 목차

1. [기술 스택](#기술-스택)
2. [배포방식](#ERD)
3. [쿠버네티스](#서비스-아키텍처)
4. [도전한 사항](#도전한-사항)
5. [최적화한 사항](#최적화한-사항)

## 💡 프로젝트 소개
>디케이테크인 내부의 공정하고 투명한 사내 어린이집 인원 배정을 위해 어린이집 모집과 추첨을 자동화한 서비스입니다.

## 🛠️ 기술 스택
<img width="559" alt="Screenshot 2024-08-18 at 7 45 10 PM" src="https://github.com/user-attachments/assets/f05610e5-703f-4bd6-a138-5929d84f7262">

DNS: 가비아 도메인(http://kidsincompany.shop/)

Subnet: Private/Public Subnet으로 나누어 관리

Kubernetes Engine: 프론트와 백엔드 서비스 및 MySQL, redis와 같은 DB를 파드에서 관리

Load Balancer: 프론트, 백엔드 서비스를 Nginx Ingress와 묶어서 외부로 라우팅

Virtual Machine: Bastion, Nat, 서비스 Runner

Lambda: 추첨 확률 예측을 위한 함수 실행

AWS API GateWay: Lambda와 Spring 서비스를 연결해주는 Gateway

S3: 이미지 및 도큐먼트 저장소

ArgoCD: 서비스 배포 도구 쿠버네티스 파드에서 실행

Prometheus, Grafana: 모니터링 도구

## 📝 배포방식

[CI]

전형적인 Docker image Build 방식을 따르고 있습니다. 빌드된 이미지는 카카오 레지스트리에 tag와 함께 올라가고 있습니다.
FrontEND: npm 방식으로 빌드 및 이미지 업로드
BackEND: JIB 방식으로 빌드 및 이미지 업로드

[CD]

FrontEND, BackEND 동일하게 빌드된 이미지가 레지스트리에 올라가면 ArgoCD에서 Trigger(deployment.yaml)를 받아 
ArgoCD에 replicaset 3개로 배포
<img width="1031" alt="Screenshot 2024-08-18 at 8 48 18 PM" src="https://github.com/user-attachments/assets/09ac76d4-e586-488a-88e7-f3dd0bc7905d">


## 🌱 쿠버네티스


## 도전한 사항

## 최적화한 사항

* JIB 빌드를 통한 빌드 시간 77% 단축
* JRE 이미지 빌드를 통한 이미지 1.8배 경량화 및 보안성 강화
    * https://github.com/CLOVIDER/kic-backend/pull/120
* 관리자 대시보드 페이지 Redis 캐싱을 통한 쿼리 2회 단축 및 서버 데이터 전송과 처리 성능 2.5배 향상
    * https://github.com/CLOVIDER/kic-backend/issues/186
