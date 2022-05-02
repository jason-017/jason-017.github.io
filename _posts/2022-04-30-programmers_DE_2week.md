---
title: "[programmers data engineering] 2week"
excerpt: "[스터디/6기] 실리콘밸리에서 날아온 데이터 엔지니어링 스타터 키트 with Python"

categories:
 - 프로그래머스 데이터 엔지니어링
tags:
 - python
 - airflow
 - data engineer
 - programmers
---

## 프로그래머스 데이터 엔지니어링 2주차

[실리콘밸리에서 날아온 데이터 엔지니어링 스타터 키트 with Python](https://programmers.co.kr/learn/courses/12916)

2주차부터 데이터 엔지니어링이 무엇인지, 어떤 스킬셋이 필요한지, 데이터 인프라는 어떻게 building할 것인지에 대해 배울 수 있었다.

### 0. 데이터 엔지니어링은 뭘까?
당연한 이야기이겠지만, 데이터 엔지니어가 하는 다양한 업무들을 데이터 엔지니어링이라고 한다. 큰 틀에서 보자면 아래 3가지와 같다.
- Managing Data Warehouse(데이터 웨어하우스 관리)
  - 데이터 웨어하우스를 관리하는 것이 데이터 엔지니어의 가장 주요한 업무이다.
- Writing and Managing Data Pipelines(데이터 파이프라인 작성 및 관리)
  - 데이터 파이프라인을 ETL(Extract, Transform, Load) 또는 data job이라 부르기도 한다.
  - 데이터 파이프라인은 상황에 맞게 생성되어야 한다.
    - 배치성 데이터 처리인지, 실시간 데이터 처리인지
    - A/B 테스트 분석
    - summary data 생성
- 유저 행동 데이터 등의 이벤트 데이터 수집

### 1. 데이터 엔지니어에게 필요한 스킬들
데이터 인프라를 다루기 위해서는 아래와 같이 다양한 플랫폼/소프트웨어/툴 등을 알아야 한다. 그렇다고 모든 지식을 알아야 한다는 것은 아니다. 필요에 따라 습득해 나가면 되는 것이다. 애초에 모든 내용을 다 알기에는 너무 많고 새로운 기술들이 나오는 속도는 매우 빠르기 때문에 불가능하다.

|skill|내용|
|-----|---|
|SQL|hive/presto/SparkSQL ... |
|programming language|python/scala/java ...|
|large scale computing platform|spark/YARN ...|
|knowledge|ML/statistics ...|
|cloud computing|AWS: redshift/EMR/S3/SageMaker ...<br>GCP: BigQuery/ML engine ...<br>MicroSoft: AzureML ...<br>snowflake|
|ETL/ELT scheduler|Airflow, NiFi ...|

### 2. Data Infra는 필수적일까?
이미 시장에서 자리 잡은 기업의 경우 데이터 인프라는 필수적일텐데, 스타트업과 같이 이제 막 설립된 기업의 경우 데이터 자체가 없어 데이터 분석이 불가능하므로 데이터 인프라는 필수적이지 않다. 만약 필요하다면 stitch data, fiveTran, segment 등과 같이 저렴한 금액으로 ETL 서비스와 데이터 웨어하우스를 제공하는 플랫폼을 이용하면 된다. 이후 스타트업이 잘 성장해서 데이터가 생성되기 시작할 때 데이터 인프라를 구축하면 된다.

![data infra](/assets/de2/datainfra.png)

### 3. Data Infra 간단하게 구축하기
- 데이터 웨어하우스는 RedShift를 사용할 예정
  - RedShift는 AWS 데이터 웨어하우스이며, 최대 1.6PB의 데이터까지 scalable(확장 가능한) postgreSQL 엔진이다.
  - 용량이 작거나 개인 프로젝트 용도로 RDBMS인 MySQL를 사용해도 좋고, RedShift trial 버전을 사용해도 좋다.
  - 본질은 production DB와 분리된 공간에서 데이터 분석을 위해 편하게 다룰 수 있는 저장소가 필요하다는 것이다. Scalable한 저장소가 필요한 이유도 기업에서 발생하는 데이터가 굉장히 방대하기 때문이라, 개인 프로젝트에서는 사실 scalable할 이유도 딱히 없다.
  - 데이터 웨어하우스의 가장 주요한 목적은 모든 데이터를 하나의 저장소에 모아두는 것이다.
- MySQL로부터 일부 데이터 마이그레이션 예정
  - production DB는 서비스에 필요한 핵심 데이터만 들어가야 한다. 이를 통해 성능을 최대한 끌어올릴 수 있다.
- ETL or ELT or data pipeline이라고 불리는 다양한 데이터 수집 방법들
  - 파일 크기, 수집 빈도, 소스 데이터 접근 등 여러 측면을 고려하여 데이터 수집 방법을 정해야 한다.
  - ETL툴로는 airflow, oozie, azkaban 등이 존재한다.
  - Hive or spark로 배치성 처리를 하기도 한다.
  - 실시간 처리를 위해 kafka, sparkStreaming, cassandra 등을 사용한다.

### 4. 수집된 데이터 활용
- raw data를 가지고 있는 것은 분명히 좋지만, 너무나도 자세한 정보들을 제공한다. 때문에 우리는 <u>summary table</u>을 데이터 웨어하우스 내에 주기적으로 build/update해줘야 한다.
- 여기서 SQL 전문 지식은 매우 중요해지기 시작한다. 왜냐하면 redshift, hive, BigQuery, sparkSQL 등을 사용해야 하기 때문이다. 물론, SQL 말고 spark와 같이 더욱 프로그래밍적인 접근을 할 수도 있다.
- 이후 summary table을 looker, tableau, superset 등을 이용해서 dash board를 제공할 수 있다. 현재는 looker가 가장 핫한 툴로 사용되고 있다. 그리고 summary table을 DS(Data Science)팀에 제공해야 한다.

![data flow](/assets/de2/dataflow.png)

