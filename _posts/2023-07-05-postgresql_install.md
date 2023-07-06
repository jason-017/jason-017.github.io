---
title: "[postgreSQL] postgresql 빠르게 설치하기"
excerpt: "centOS 7에서 postgresql을 빠르게 설치해보자!"

categories:
 - postgreSQL
tags:
 - postgresql
 - database
---

### CentOS에 postgreSQL 설치하기
CentOS 7에 postgreSQL을 빠르고 간단하게 설치해보자!
Its's super fast track!
설치 및 외부 통신까지만 다루며 conf 등은 추후에 다룰 예정이다.

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

6. 외부 통신 설정
    - postgresql 포트 설정
      - port 접근 가능하도록 conf 파일 내 port 주석을 해제한다. conf 파일은 일반적으로 postgresql > data 디렉토리 아래에 있다.
    - postgresql 클라이언트 설정
      - postgresql은 pg_hba.conf 파일을 통해 클라이언트 주소, 역할, DB 연결 허용 여부를 설정한다.
    ```bash
    $ vi /home/postgresql/pgsql/data/postgresql.conf
    ...
    port=5432
    ...

    # 모든 클라이언트가 모든 DB에 접근할 수 있도록 허용했다.
    $ vi /home/postgresql/pgsql/data/pg_hba.conf
    # TYPE  DATABASE        USER            ADDRESS                 METHOD
    ...
    host    all             all             0.0.0.0/0               trust
    ...
    ```
    
