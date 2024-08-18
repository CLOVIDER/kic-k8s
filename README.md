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
<img width="427" alt="Screenshot 2024-08-18 at 7 39 32 PM" src="https://github.com/user-attachments/assets/2bc706b8-bb12-43c0-89eb-343fc098d156">


## 📝 ERD

![키즈인컴퍼니 ERD](https://github.com/user-attachments/assets/56cefb42-935d-4ce6-b79b-7cd8f2e05a73)

## 🌱 서비스 아키텍처

<img width="1097" alt="image" src="https://github.com/user-attachments/assets/fc729e9e-baf9-4d7d-9a8b-235083346fef">

## 도전한 사항

## 최적화한 사항

* JIB 빌드를 통한 빌드 시간 77% 단축
* JRE 이미지 빌드를 통한 이미지 1.8배 경량화 및 보안성 강화
    * https://github.com/CLOVIDER/kic-backend/pull/120
* 관리자 대시보드 페이지 Redis 캐싱을 통한 쿼리 2회 단축 및 서버 데이터 전송과 처리 성능 2.5배 향상
    * https://github.com/CLOVIDER/kic-backend/issues/186
