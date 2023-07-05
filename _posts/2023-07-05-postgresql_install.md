---
title: "[postgreSQL] centOS에 간단하게 postgresql 설치해보기"
excerpt: "centOS7에서 postgresql을 빠르게 설치해보자."

categories:
 - postgreSQL
 - DB
tags:
 - postgresql
 - database
---

## 프로그래머스 데이터 엔지니어링 2주차

[posrgresql 설치 파일 다운로드 링크](https://www.postgresql.org/ftp/source)

![data flow](/assets/postgresql_install.png)

1. 유저 추가
```bash
$ useradd postgresql
$ su - postgresql
```

2. 설치 파일 압축 해제
```bash
$ tar -xf postgresql-13.3.tar.gz
$ cd postgresql-13.3
```

3. 엔진 설치
```bash
./configure --prefix=/home/postgresql/pgsql
make && make install
```

4. DB 클러스터 초기화
```bash
$ cd /home/postgresql/pgsql/bin
$ ./initdb -D /home/postgresql/pgsql/data
```

5. bash 초기 설정
```bash
$ vi ~/.bash_profile # 아래 내용 추가
export LD_LIBRARY_PATH=:$HOME/pgsql/lib
export PATH=$PATH:$HOME/pgsql/bin
export PGDATA=$HOME/pgsql/data

$ source ~/.bash_profile # 추가 후 source 작업
```

**[외부 통신 설정]**
1. postgresql 포트 설정
    - port 접근 가능하도록 conf 파일 내 port 주석을 해제한다. conf 파일은 일반적으로 postgresql > data 디렉토리 아래에 있다.
    
    ```sql
    vi /home/postgresql/pgsql/data/postgresql.conf
    ...
    port=5432
    ...
    ```
2. postgresql 클라이언트 설정
    - postgresql의 경우 클라이언트 설정을 pg_hba.conf 파일에서 설정한다.
    
    ```sql
    vi /home/postgresql/pgsql/data/pg_hba.conf
    ...
    host    all     all             0.0.0.0/0                 trust
    ...
    ```

2주차부터 데이터 엔지니어링이 무엇인지, 어떤 스킬셋이 필요한지, 데이터 인프라는 어떻게 building할 것인지에 대해 배웠다.

### 0. 데이터 엔지니어링은 뭘까?
큰 틀에서 보자면 아래 3가지와 같다.
- Managing Data Warehouse(데이터 웨어하우스 관리)
  - 데이터 웨어하우스를 관리하는 것이 데이터 엔지니어의 가장 주요한 업무임.
- Writing and Managing Data Pipelines(데이터 파이프라인 작성 및 관리)
  - 데이터 파이프라인을 ETL(Extract, Transform, Load) 또는 data job이라 부르기도 함.
  - 데이터 파이프라인은 상황에 맞게 생성되어야 함.
    - 배치성 데이터 처리인지, 실시간 데이터 처리인지
    - A/B 테스트 분석
    - summary data 생성
- 유저 행동 데이터 등의 이벤트 데이터 수집

### 데이터 엔지니어에게 필요한 스킬들
데이터 인프라를 다루기 위해서는 아래와 같이 다양한 플랫폼/소프트웨어/툴 등을 알아야 한다. 모든 지식을 알아야 한다는 것은 아니고, 필요에 따라 습득해 나가면 된다. 모든 내용을 다 알기에는 너무 많고 새로운 기술들이 나오는 속도는 매우 빠르기 때문에 불가능하다.

|skill|내용|
|-----|---|
|SQL|hive/presto/SparkSQL ... |
|programming language|python/scala/java ...|
|large scale computing platform|spark/YARN ...|
|knowledge|ML/statistics ...|
|cloud computing|AWS: redshift/EMR/S3/SageMaker ...<br>GCP: BigQuery/ML engine ...<br>MicroSoft: AzureML ...<br>snowflake|
|ETL/ELT scheduler|Airflow, NiFi ...|

### Data Infra는 필수적일까?
이미 시장에서 자리 잡은 기업의 경우 데이터 인프라는 필수적일텐데, 스타트업과 같이 이제 막 설립된 기업의 경우 데이터 자체가 없어 데이터 분석이 불가능하므로 데이터 인프라는 필수적이지 않다. 만약 필요하다면 stitch data, fiveTran, segment 등과 같이 저렴한 금액으로 ETL 서비스와 데이터 웨어하우스를 제공하는 플랫폼을 이용하면 된다. 이후 스타트업이 잘 성장해서 데이터가 생성되기 시작할 때 데이터 인프라를 구축하면 된다.

![data infra](/assets/de2/datainfra.png)

### Data Infra 관련 내용
- Pandas로 해결할 수 없을 정도로 데이터가 커질 때 spark를 이용해서 적재하면 되기 때문에 초기에 데이터가 크지 않을 때부터 spark를 도입할 필요는 없음.
- production DB로 사용되는 RDBMS는 서비스에서 사용될 중요한 데이터만 담아두는 경우가 일반적임.
-  production DB 뿐만 아니라 수집되는 모든 데이터를 저장하는 DB를 DW라고 하며, 데이터 분석이나 시각화를 위해 활용됨.
- production DB에서 데이터 작업할 경우 서비스 장애 및 속도 저하를 일으킬 수 있기 때문에 DW 구축은 필수임.

### Data Infra 구축 trial
해당 session에서 data warehouse로 RedShift를 사용할 예정이다. RedShift는 AWS 데이터 웨어하우스이며, 최대 1.6PB의 데이터까지 scalable(확장 가능한) postgreSQL 엔진을 사용한다. 용량이 작거나 개인 프로젝트 용도로 RDBMS인 MySQL를 사용해도 좋고, RedShift trial 버전을 사용해도 좋다.
<br>

본질은 production DB와 분리된 공간에서 데이터 분석을 위해 편하게 다룰 수 있는 저장소가 필요하다는 것이다. data warehouse의 가장 주요한 목적은 모든 데이터를 하나의 저장소에 모아두는 것이다. Scalable한 저장소가 필요한 이유도 기업에서 발생하는 데이터가 굉장히 방대하기 때문이라, 개인 프로젝트에서는 사실 scalable할 이유도 딱히 없다.
<br>

- MySQL(production DB)로부터 일부 데이터를 redshift에 마이그레이션할 예정
- data pipeline(ETL or ELT)
  - 파일 크기, 수집 빈도, 소스 데이터 접근 등 여러 측면을 고려하여 데이터 수집 방법을 정해야 함.
  - ETL툴로는 airflow, oozie, azkaban 등이 존재함.
  - Hive or spark로 배치성 처리를 하기도 함.
  - 실시간 처리를 위해 kafka, sparkStreaming, cassandra 등을 사용함.

### 4. 수집된 데이터 활용
- raw data를 가지고 있는 것은 분명히 좋지만, 너무나도 자세한 정보들을 제공함. 때문에 우리는 <u>summary table</u>을 데이터 웨어하우스 내에 주기적으로 build/update해줘야 함.
- 여기서 SQL 전문 지식은 매우 중요해지기 시작함. 왜냐하면 redshift, hive, BigQuery, sparkSQL 등을 사용해야 하기 때문임. 물론, SQL 말고 spark와 같이 더욱 프로그래밍적인 접근을 할 수도 있음.
- 이후 summary table을 looker, tableau, superset 등을 이용해서 dashboard를 제공할 수 있음.

![data flow](/assets/de2/dataflow.png)

