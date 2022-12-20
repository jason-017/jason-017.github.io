---
title: "[programmers data engineering] airflow 설치 및 서비스 등록(ubuntu)"
excerpt: "[스터디/6기] 실리콘밸리에서 날아온 데이터 엔지니어링 스타터 키트 with Python"

categories:
 - 프로그래머스 데이터 엔지니어링
tags:
 - python
 - airflow
 - data engineer
 - programmers
---

# Airflow 2.0 Installation

우분투에서 Airflow 2.0을 설치하는 방법에 대한 문서로 파이썬 3.0을 사용한다. 앞서 별도로 공유된 ssh 로그인 문서를 참조하여 할당된 EC2 서버로 로그인한다(이 때 ubuntu 계정을 사용함).

## Airflow Python Module Installation

#### 먼저 우분투의 소프트웨어 관리 툴인 apt-get을 업데이트하고 파이썬 3.0 pip을 설치한다.

```
sudo apt-get update
sudo apt-get install -y python3-pip
```

#### 다음으로 Airflow 2.0을 설치하고 필요 기타 모듈을 설치한다.

```
sudo apt-get install -y postgresql-server-dev-all
sudo apt-get install -y libmysqlclient-dev
sudo pip3 install numpy
sudo pip3 install pandas
sudo pip3 install apache-airflow==2.1.4
sudo pip3 install apache-airflow-providers-postgres[amazon]
sudo pip3 install apache-airflow-providers-mysql[amazon]
sudo pip3 install SQLAlchemy==1.3.23
sudo pip3 install typing_extensions
```

## airflow:airflow 계정 생성

airflow 서비스는 airflow 사용자를 기반으로 실행될 예정이므로 airflow 계정을 생성한다. airflow 계정의 홈디렉토리는  /home/airflow로 설정한다.

```
sudo groupadd airflow
sudo useradd -s /bin/bash airflow -g airflow -d /home/airflow -m
```

## Airflow의 정보가 저장될 데이터베이스로 사용될 Postgres 설치

Airflow는 기본으로 SQLite 데이터베이스를 설치하는데 이는 싱글 쓰레드라 다수의 DAG 혹은 다수의 Task들이 동시에 실행되는 것을 지원하지 못한다. 때문에 이를 Postgres로 교체한다. 

#### Postgres 설치

```
sudo apt-get install -y postgresql postgresql-contrib
```

#### 다음으로 Airflow가 Postgres를 접속할 때 사용할 계정을 Postgres 위에서 생성

이를 위해서 postgres 사용자로 재로그인을 하고 postgres shell (psql)을 실행한 다음에 airflow라는 이름의 계정을 생성한다. 마지막에 exit를 실행해서 원래 ubuntu 계정으로 돌아가는 스텝을 잊지 않는다.

```
$ sudo su postgres
$ psql
psql (10.12 (Ubuntu 10.12-0ubuntu0.18.04.1))
Type "help" for help.

postgres=# CREATE USER airflow PASSWORD 'airflow';
CREATE ROLE
postgres=# CREATE DATABASE airflow;
CREATE DATABASE
postgres=# \q
$ exit
```

#### Postgres를 재시작

이 명령은 ubuntu 계정에서만 실행이 가능함을 꼭 기억!

```
sudo service postgresql restart
```


## Airflow 첫 번째 초기화 

#### 앞서 설치된 Airflow를 실행하여 기본환경을 만든다.

이 때 SQLite가 기본 데이터베이스로 설정되며 앞서 설치한 Postgres로 변경해주어야 한다.
```
sudo su airflow
$ cd ~/
$ mkdir dags
$ AIRFLOW_HOME=/home/airflow airflow db init
$ ls /home/airflow
airflow.cfg  airflow.db  dags   logs  unittests.cfg
```

#### Airflow 환경 파일(/home/airflow/airflow.cfg)을 편집하여 다음 3가지를 바꾼다.

 * "executor"를 SequentialExecutor에서 LocalExecutor로 수정한다.
 * DB 연결스트링("sql_alchemy_conn")을 앞서 설치한 Postgres로 바꾼다.
   * 이 경우 ID와 PW와 데이터베이스 이름이 모두 airflow를 사용하고 호스트 이름은 localhost를 사용한다.
 * "load_examples" 설정을 False로 바꾼다.

```
[core]
...
executor = LocalExecutor
...
sql_alchemy_conn = postgresql+psycopg2://airflow:airflow@localhost:5432/airflow
...
load_examples = False
```

#### Airflow를 재설정

```
AIRFLOW_HOME=/home/airflow airflow db init
```


## Airflow 웹서버와 스케줄러 실행

Airflow 웹서버와 스케줄러를 백그라운드 서비스로 사용하려면 다음 명령을 따라하여 두 개를 서비스로 등록한다. 
다음 명령들은 <b>ubuntu</b> 계정에서 실행되어야한다. 만일 "[sudo] password for airflow: " 메시지가 나온다면 지금 airflow 계정을 사용하고 있다는 것으로 이 경우 "exit" 명령을 실행해서 ubuntu 계정으로 돌아온다.


#### 웹서버와 스케줄러를 각기 서비스로 등록
- 웹서버 서비스로 등록
```
$ sudo vi /etc/systemd/system/airflow-webserver.service

[Unit]
Description=Airflow webserver
After=network.target

[Service]
Environment=AIRFLOW_HOME=/home/airflow
User=airflow
Group=airflow
Type=simple
ExecStart=/usr/local/bin/airflow webserver -p 8080
Restart=on-failure
RestartSec=10s

[Install]
WantedBy=multi-user.target
```

- 스케줄러 서비스로 등록
```
$ sudo vi /etc/systemd/system/airflow-scheduler.service

[Unit]
Description=Airflow scheduler
After=network.target

[Service]
Environment=AIRFLOW_HOME=/home/airflow
User=airflow
Group=airflow
Type=simple
ExecStart=/usr/local/bin/airflow scheduler
Restart=on-failure
RestartSec=10s

[Install]
WantedBy=multi-user.target
```

#### 웹서버 및 스케줄러 서비스 활성화

```
sudo systemctl daemon-reload
sudo systemctl enable airflow-webserver
sudo systemctl enable airflow-scheduler
```

서비스 시작

```
sudo systemctl start airflow-webserver
sudo systemctl start airflow-scheduler
```

서비스 상태 확인

```
sudo systemctl status airflow-webserver
sudo systemctl status airflow-scheduler
```

## 마지막으로 Airflow webserver에 로그인 어카운트를 생성

airflow계정에서 실행되어야 한다. password의 값을 적당히 다른 값으로 바꾼다.

```
AIRFLOW_HOME=/home/airflow airflow users  create --role Admin --username admin --email admin --firstname admin --lastname admin --password admin1234
```

만일 실수로 위의 명령을 ubuntu 어카운트에서 실행했다면 admin 계정을 먼저 지워야한다. 지울 때 아래 명령을 사용한다.

```
AIRFLOW_HOME=/home/airflow airflow users delete --username admin
```

그리고나서 본인 서버를 웹브라우저에서 포트번호 8080을 이용해 접근해보면 아래와 같은 로그인 화면이 실행되어야 한다. 예를 들어 본인 EC2 서버의 호스트 이름이 ec2-xxxx.us-west-2.compute.amazonaws.com이라면 https://ec2-xxxx.us-west-2.compute.amazonaws.com:8080/을 웹브라우저에서 방문해본다.


## /home/airflow/dags 폴더에 DAG 작성

작성한 DAG들이 웹서버에 보이기까지 시간이 소요되며, 그 이유는 Airflow가 기본적으로 5분 마다 한번씩 dags 폴더를 뒤져서 새로운 DAG이 있는지 보기 때문이다. 이 변수는 dag_dir_list_interval으로 airflow.cfg에서 확인할 수 있으며 기본값은 300초 (5분)이다. 


## airflow 사용자 로그인하여 AIRFLOW_HOME 환경변수를 bash 파일에 작성

```
$ vi ~/.bashrc
...
export AIRFLOW_HOME=/home/airflow
...
```

## 환경변수 적용 확인
```
$ source ~/.bashrc
$ echo $AIRFLOW_HOME
/home/airflow
```