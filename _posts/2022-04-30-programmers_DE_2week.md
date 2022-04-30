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

2주차부터 데이터 엔지니어링이 무엇인지, 어떤 스킬셋이 사용되는지, 데이터 인프라는 어떻게 building할 것인지에 대해 배울 수 있었다.

### 0. 데이터 엔지니어링은 뭘까?
당연한 이야기이겠지만, 데이터 엔지니어가 하는 다양한 업무들을 데이터 엔지니어링이라고 한다. 큰 틀에서 보자면 아래 3가지와 같다.
- Managing Data Warehouse(데이터 웨어하우스 관리)
  - 1주차에 배웠던 데이터 웨어하우스를 관리하는 것이 어떻게 보면 데이터 엔지니어의 가장 주요한 업무라고 할 수 있겠다.
- Writing and Managing Data Pipelines(데이터 파이프라인 작성 및 관리)
  - 데이터 파이프라인을 ETL(Extract, Transform, Load) 또는 data job으로 부르기도 한다.
  - 데이터 파이프라인은 상황에 맞게 생성되어야 한다.
    - 배치성 또는 실시간 프로세싱
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

### 2. Data Infra 구축
이미 시장에서 자리 잡은 기업의 경우 데이터 인프라는 필수적일텐데, 스타트업과 같이 이제 막 설립된 기업의 경우 데이터 자체가 없어 데이터 분석이 불가능하므로 데이터 인프라는 필수적이지 않다. 만약 필요하다면, stitch data/fiveTran/segment 등과 같이 저렴한 금액으로 ETL 서비스와 데이터 웨어하우스를 제공하는 플랫폼을 이용하면 된다. 이후 스타트업이 잘 성장해서 데이터가 생성되기 시작하고 이를 분석에 활용하기 위해서는 데이터 인프라를 구축이 필요하다.
- 그래서 data infra가 뭘까?
  - 여기서는 다양한 data source로부터 다양한 형태의 데이터를 원하는 형태로 변환하여 저장되는 데이터 웨어하우스와 같은 저장소를 의미한다.
  - 우리는 데이터 분석을 위해 scalable하면서 분리된 저장소가 필요한데, 데이터 웨어하우스가 필요하다는 이야기와 같다.

![data lake](/assets/de2/datainfra.png)

### 3. Data Infra를 간단하게 구축해보자!
- 데이터 웨어하우스는 redshift로 사용할 예정
  - 용량이 작거나 개인 프로젝트 용도로 RDBMS인 MySQL를 사용하는 것도 좋다.
  - 본질은 productionDB와 분리된 공간에서 데이터 분석을 위해 편하게 다룰 수 있는 저장소가 필요한 것이다. Scalable한 저장소가 필요한 이유도 기업에서 발생하는 데이터가 굉장히 많은 것이기 때문에 개인 프로젝트에서는 scalable할 이유가 딱히 없다.