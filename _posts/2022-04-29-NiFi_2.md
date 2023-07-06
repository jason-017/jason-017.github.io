---
title: "[NIFI] Apache NiFi 설치"
categories:
 - NiFi
excerpt: "centOS 7에 Apache NiFi 설치하기"
tags:
 - apache
 - NiFi
 - dataflow
 - ETL
---
### centOS 7에 Apache NIFI 설치
Apache NiFi tar.gz 파일을 다운로드한다. 설치한 tar.gz 파일을 centOS로 옮겨서 원하는 위치에 압축을 풀어주고, nifi-env.sh 파일에서 JAVA_HOME 주석을 해제해준다.<br>
참고로 자바는 1.8 이상의 버전을 지원한다고 명시되어 있다.<br>
![nifi download](/assets/nifi_image/nifidownload.PNG)<br>
[NiFi 다운로드 페이지로 이동](https://nifi.apache.org/download.html)<br>

```bash
bash> echo $JAVA_HOME
/usr/local/java/jdk1.8.0_311

bash> vi /nifi-1.16.0/bin/nifi-env.sh
...
# The java implementation to use.
export JAVA_HOME=/usr/local/java/jdk1.8.0_311/
...
```

### NIFI 설정
nifi.properties 파일에 http 또는 https 설정를 설정한다. http와 https를 동시에 실행할 수 없으므로 한가지만 설정하고 나머지 하나는 공백으로 남겨둬야 한다.
- default는 https이며 접속할 때 로그인을 해야 한다. 로그인 내용은 로그를 통해 확인이 가능하다.
- http를 사용하고 싶으면 추가적으로 nifi.remote.input.secure를 false로 설정해줘야 한다.
- nifi.web.http.host(s): http(s)로 접근을 허용할 host 이름이며, 모든 네트워크 접근을 허용하려면 0.0.0.0으로 설정하면 된다.
    
```bash
bash> vi nifi-1.16.0/conf/nifi.properties
...
# Site to Site properties
nifi.remote.input.host=
nifi.remote.input.secure=false
nifi.remote.input.socket.port=
nifi.remote.input.http.enabled=true
nifi.remote.input.http.transaction.ttl=30 sec
nifi.remote.contents.cache.expiration=30 secs

# web properties #
#############################################

# For security, NiFi will present the UI on 127.0.0.1 and only be accessible through this loopback interface.
# Be aware that changing these properties may affect how your instance can be accessed without any restriction.
# We recommend configuring HTTPS instead. The administrators guide provides instructions on how to do this.

nifi.web.http.host=0.0.0.0
nifi.web.http.port=09091
nifi.web.http.network.interface.default=

#############################################

nifi.web.https.host=
nifi.web.https.port=
...
```

### NiFi 실행하기
nifi.sh로 직접 실행시키거나 systemctl로 실행시킬 수 있다. 정상 구동을 확인하기 위해 log를 함께 봐야 한다.
    
```bash
$ ./bin/nifi.sh start
또는
$ systemctl start nifi

Java home: /usr/local/java/jdk1.8.0_311/
NiFi home: /nifi-1.16.0

Bootstrap Config File: /nifi-1.16.0/conf/bootstrap.conf

$ tail -f /nifi-1.16.0/logs/nifi-app.log
...
2022-04-29 19:52:08,302 INFO [main] org.apache.nifi.web.server.JettyServer http://0.0.0.0:1633/nifi
2022-04-29 19:52:08,303 INFO [main] org.apache.nifi.BootstrapListener Successfully initiated communication with Bootstrap
2022-04-29 19:52:08,303 INFO [main] org.apache.nifi.NiFi Started Application Controller in 9.825 seconds (9825205493 ns)
...
```

### NiFi 중지하기
nifi.sh로 직접 stop 시키거나 systemctl로 stop 시킬 수 있다.

```bash
$ ./nifi.sh stop
또는
$ systemctl stop nifi

$ tail -f /nifi-1.16.0/logs/nifi-app.log
...
2022-04-29 19:52:36,355 INFO [Thread-1] org.apache.nifi.NiFi Application Server shutdown started
2022-04-29 19:52:36,355 INFO [Thread-1] org.apache.nifi.nar.NarAutoLoader NAR Auto-Loader stopped
2022-04-29 19:52:36,355 INFO [Thread-1] org.apache.nifi.NiFi Application Server shutdown completed
...
```

### NiFi 상태 보기
다른 시스템과 마찬가지로 systemctl status nifi로 현재 활성화 여부를 볼 수 있다.<br>
![nifi download](/assets/nifi_image/status.PNG)
