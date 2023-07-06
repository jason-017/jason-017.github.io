---
title: "[postgreSQL] postgresql 구동, 정지, 접속"
excerpt: "postgresql 사용해보기"

categories:
 - postgreSQL
tags:
 - postgresql
 - database
---
## PostgreSQL Start
```bash
$ ./home/postgresql/pgsql/bin/pg_ctl -D /home/postgresql/pgsql/data start

waiting for server to start....

server started
```

## PostgreSQL Stop
```bash
$ ./home/postgresql/pgsql/bin/pg_ctl -D /home/postgres/pgsql/data stop

waiting for server to shut down....

server stopped
```

## PostgreSQL Restart
restart 후 출력되는 문구로 확인할 수 있듯이 stop 후 start를 해주는 것일 뿐 직접 하는 것과 다른 점은 없다.<br>
보통 이유 없이 재시작하는 경우는 많지 않은 것 같은데... 경우에 따라 가장 적절한 방법을 선택하자.
```bash
$ ./home/postgresql/pgsql/bin/pg_ctl -D /home/postgres/pgsql/data restart

waiting for server to shut down....

server stopped

waiting for server to start....

server started
```

## postgresql 접속
```bash
$ psql -U postgresql -p 5432 -d postgres
```

## postgresql 터미널에서 나오기
postgresql 터미널에서 빠져나오는 것이지, 종료하는 명령어가 아니다. 때문에 다시 접속 구문으로 postgresql 터미널로 접속할 수 있다.
```bash
# 'ctrl + d'를 통해 탈출 가능
```
    
