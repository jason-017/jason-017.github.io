---
title: "[postgreSQL] postgresql 구동, 정지, 접속"
excerpt: "postgresql 사용해보기"

categories:
 - postgreSQL
tags:
 - postgresql
 - database
---
### PostgreSQL Start
```bash
$ ./home/postgresql/pgsql/bin/pg_ctl -D /home/postgresql/pgsql/data start

waiting for server to start....

server started
```

### PostgreSQL Stop
```bash
$ ./home/postgresql/pgsql/bin/pg_ctl -D /home/postgres/pgsql/data stop

waiting for server to shut down....

server stopped
```

### PostgreSQL Restart
```bash
# restart 후 확인할 수 있듯이 stop 후 start를 하는 것과 동일함.
$ ./home/postgresql/pgsql/bin/pg_ctl -D /home/postgres/pgsql/data restart

waiting for server to shut down....

server stopped

waiting for server to start....

server started
```

### postgresql 접속
```bash
$ psql -U postgresql -p 5432 -d postgres
```

### postgresql 탈출
postgresql 터미널에서 빠져나오는 것이지, 종료하는 옵션이 아니다. 때문에 다시 접속 구문으로 postgresql 터미널로 접속할 수 있다.
```bash
# 'ctrl + d'를 통해 탈출 가능
```
    
