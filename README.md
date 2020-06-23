# 멀티캠퍼스 클라우드 기반 MSA 전문가 양성 과정 프로젝트

## 주제
> MSA기반 비식별처리 웹 서비스

## 진행 기간
> 2020년 5월 25일 ~ 6월 20일

## 팀원
> 김신학, 최민영, 최지훈, 이동욱, 노설

## 전체 서비스 시나리오
![services](https://github.com/Colvet/deIdentifiedProdjcet/blob/master/Img/services.png)



## API 형식 출력 시나리오
+ 1~5번은 시간 부족으로 구현 하지 못함. 추후 보완할 예정

![filetoapi](https://github.com/Colvet/deIdentifiedProdjcet/blob/master/Img/filetoapi.png)

## 서비스 아키텍처
![Architecture](https://github.com/Colvet/deIdentifiedProdjcet/blob/master/Img/Architecture.png)

## 개발환경 및 사용 프로그램
![tool](https://github.com/Colvet/deIdentifiedProdjcet/blob/master/Img/tool.png)

## 서비스
1. 프론트 및 로그인 서비스(port:8087)
    + 개발자: 이동욱, 노설
    + 역할: 프론트, 회원가입&로그인 서비스
    + 코드: lsy0566/Front_DS
    + 도커 이미지: colvet/login-app-service:latest

2. 업로드 서비스(본인 개발)(port:8083)
    + 역할: 업로드 파일 저장, csv 파일 Null값 제거 및 갯수 mongodb 업데이트
    + 개발자: 김신학
    + 코드: Colvet/django-app-service
    + 도커 이미지: https://hub.docker.com/r/colvet/djangotest
    + 개발 환경 및 사용 라이브러리
        + 개발환경: PyCharm
        + 라이브러리: Django, Djnago-restfreamwork, pandas, pymongo
        + Db: mongoDb

3. 비식별 처리 및 검증 서비스(port:8085)
    + 개발자: 최민영, 최지훈
    + 역할: 비식별 처리
    + 코드: https://github.com/griffinGC/DeIdentifier-sixth-sense
    + 도커 이미지: wlgns0719/dei-service:latest

4. 타서비스(File -> API)와의 연동 서비스(본인 개발)
    + 개발자: 김신학
    + 카프카 서버: kafka&zookeeper/docker-compose.yml -> kafak(port:9092) 1개, zookeeper(port:2181) 1개, topic: file-events
    + Kafka Producer(port:8099)
        + 역할: 파일 읽기, 파일 Line당 kafka 메시지 전송(port:8099)
        + 코드: https://github.com/Colvet/kafka4Producer
        + 개발 환경 및 사용 라이브러리
            + 개발환경: Intelij
            + 프레임 워크: Spring boot
            + 라이브러리: Spring Web, Lombok, Spring for Apache Kafka, Jpa, Mysql, opencsv

    + Kafka Consumer(port:8098)
        + 역할: kafka 메시지 consume, 메시지들 합하여 Json 형식으로 출력()
        + 개발자: 김신학, 윤종필
        + 코드: https://github.com/Colvet/kafka1Consumer
        + 개발 환경 및 라이브러리
            + 개발환경: Intelij
            + 프레임 워크: Spring boot
            + 라이브러리: Spring Web, Lombok, Spring for Apache kafak, json

## Db
+ Mysql(port: 3306)
    + Table: users, result_log
+ MongoDb(port: 27017)
    + Table: result_log

# Docker-compose 파일 실행
> 특이사항: 회원 가입 서비스의 mybatis 사용으로 users 테이블 생성 후 실행, 파일 저장 위치 설정 후 실행, kafka 서비스는 추후 도커 이미지 빌드 예정

1. docker-compose up
2. docker exec -it mysql sh
3. mysql -uroot -p (비밀번호: root)
4. use k8sdb
5. CREATE TABLE users ( id INT(255) NOT NULL AUTO_INCREMENT, email VARCHAR(255) NOT NULL, username VARCHAR(255) NOT NULL, password VARCHAR(255) NOT NULL, phoneNumber VARCHAR(255) NOT NULL, isMember INT(3) NOT NULL DEFAULT '1', admin INT(3) UNSIGNED NOT NULL DEFAULT '0', PRIMARY KEY (id), UNIQUE INDEX username (username) );
