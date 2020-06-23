# 멀티캠퍼스 클라우드 기반 MSA 전문가 양성 과정 프로젝트

## 주제
> MSA기반 비식별처리 웹 서비스

## 진행 기간
> 2020년 5월 25일 ~ 6월 20일

## 팀원
> 김신학, 최민영, 최지훈, 이동욱, 노설

## 전체 서비스 시나리오
![services](/img/services.png)

## API 형식 출력 시나리오
+ 1~5번은 시간 부족으로 구현 하지 못함. 추후 보완할 예정

![filetoapi](/img/filetoapi.png)

## 서비스 아키텍처
![Architecture](/img/Architecture.png)

## 개발환경 및 사용 프로그램
![tool](/img/tool.png)

## 서비스
1. 프론트 및 로그인 서비스
    + 개발자: 이동욱, 노설
    + 역할: 프론트, 회원가입&로그인 서비스
    + 코드: lsy0566/Front_DS
    + 도커 이미지: colvet/login-app-service:latest

2. 업로드 서비스(본인 개발)
    + 역할: 업로드 파일 저장, csv 파일 Null값 제거 및 갯수 mongodb 업데이트
    + 개발자: 김신학
    + 코드: Colvet/django-app-service
    + 도커 이미지: https://hub.docker.com/r/colvet/djangotest
    + 개발 환경 및 사용 라이브러리
        + 개발환경: PyCharm
        + 라이브러리: Django, Djnago-restfreamwork, pandas, pymongo
        + Db: mongoDb

3. 비식별 처리 및 검증 서비스
    + 개발자: 최민영, 최최지훈
    + 역할: 비식별 처리
    + 코드: https://github.com/griffinGC/DeIdentifier-sixth-sense
    + 도커 이미지: wlgns0719/dei-service:latest

4. 타서비스(File -> API)와의 연동 서비스(본인 개발)
    + 개발자: 김신학
    + 카프카 서버: kafka&zookeeper/docker-compose.yml -> kafak 1개, zookeeper 1개, topic: file-events
    + Producer: 파일 읽기, 파일 Line당 kafka 메시지 전송
        + 코드: https://github.com/Colvet/kafka4Producer
        + 개발 환경 및 사용 라이브러리
            + 개발환경: Intelij
            + 프레임 워크: Spring boot
            + 라이브러리: Spring Web, Lombok, Spring for Apache Kafka, Jpa, Mysql, opencsv

    + Consumer: kafka 메시지 consume, 메시지들 합하여 Json 형식으로 출력
        + 개발자: 김신학, 윤종필
        + 코드: https://github.com/Colvet/kafka1Consumer
        + 개발 환경 및 라이브러리
            + 개발환경: Intelij
            + 프레임 워크: Spring boot
            + 라이브러리: Spring Web, Lombok, Spring for Apache kafak, json