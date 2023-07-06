---
title: "[postgreSQL] postgresql 빠르게 설치하기"
excerpt: "centOS 7에서 postgresql을 빠르게 설치해보자!"

categories:
 - postgreSQL
tags:
 - postgresql
 - database
---

## CentOS에 postgreSQL 설치하기
CentOS 7에 postgreSQL을 빠르고 간단하게 설치해보자!<br>
Its's a super fast track!<br>
간단 설치 가이드이기 때문에 설치 및 외부 통신까지만 다루며 conf 등은 추후에 다룰 예정이다.<br>
![super fast](/assets/superfast.png)
### postgresql 압축 파일 다운로드
먼저 postgresql 홈페이지에 접속하여 tar 파일을 다운로드한다. 필자의 경우 v13.3 버전(postgresql-13.3.tar.gz)으로 다운로드했다.<br>
<img src="/assets/postgresql_install.png" width="80%" height="80%"><br>
[posrgresql 설치 파일 다운로드 링크](https://www.postgresql.org/ftp/source)

### 유저 추가
```bash
$ useradd postgresql
$ su - postgresql
```

### 설치 파일 압축 해제
```bash
$ tar -xf postgresql-13.3.tar.gz
$ cd postgresql-13.3
```

### 엔진 설치
```bash
./configure --prefix=/home/postgresql/pgsql
make && make install
```

### DB 클러스터 초기화
```bash
$ cd /home/postgresql/pgsql/bin
$ ./initdb -D /home/postgresql/pgsql/data
```

### bash 초기 설정
```bash
$ vi ~/.bash_profile # 아래 내용 추가
export LD_LIBRARY_PATH=:$HOME/pgsql/lib
export PATH=$PATH:$HOME/pgsql/bin
export PGDATA=$HOME/pgsql/data

$ source ~/.bash_profile # 추가 후 source 작업
```

### 외부 통신 설정
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
    
