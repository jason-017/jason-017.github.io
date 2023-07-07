---
title: "[NIFI] Apache NIFI란?"
categories:
 - NIFI
excerpt: "Apache NIFI"
tags:
 - apache
 - NiFi
 - dataflow
 - ETL
---
[Apache NIFI docs](https://nifi.apache.org/docs.html)
## Apache NIFI 소개
대용량 데이터를 수집 및 처리할 수 있는 ETL 툴로, NSA(National Security Agency)에서 apache에 기증한 FBP가 반영된 데이터플로우 엔진이다.<br>
`FBP(Flow Based Programming): 데이터플로우 프로세스를 미리 구축하고 이를 유지하면서 데이터를 교환하는 프로그래밍 패러다임`

위키백과에서는 '소프트웨어 시스템 간 데이터 흐름을 자동화하도록 설계된 아파치 재단의 소프트웨어 프로젝트'라고 설명하고 있다.

MiNiFi라는 프로젝트를 함께 활용하여 원격지로부터 데이터를 수집할 수 있다. 다수의 원격지가 존재하더라도 모든 데이터 수집 흐름은 연동된 NIFI가 관리한다.<br>
`ex) 데이터 원격지(MiNiFi) -> NiFi -> hadoop`

## NiFi 주요 컨셉
아래는 FBP에서 사용하는 단어들에 매핑되는 NIFI만의 독자적인 단어들이다.

|NiFi Term|FBP Term|Description|
|------|---|---|
|FlowFile|Information Packet|FlowFile은 NIFI에서 처리되는 기본 단위로, 시스템(소프트웨어 등) 간 주고받는 데이터를 나타낸다. NiFi는 FlowFile에 대한 모든 정보를 남긴다.|
|FlowFile Processor|Black Box|Processor는 실제 작업을 수행하며 시스템 간 data routing, transformation을 활용한다. 0개 이상의 FlowFile을 처리하고 해당 작업을 commit하거나 rollback할 수 있다.|
|Connection|Bounded Buffer|processor 간 실제 연결(connection)을 제공한다. queue 역할을 하며 queue의 우선 순위 방식을 지정할 수 있다.|
|Flow Controller|Scheduler|Flow Controller는 NIFI의 scheduler로 process 연결 방식을 유지하고 모든 process가 사용하는 스레드와 할당을 관리한다.|
|Process Group|subnet|Process Group은 input ports를 통해 데이터를 수신하고 output ports를 통해 데이터를 보낼 수 있는 특정 process 및 connection 집합이다.|

## NIFI Architecture
NIFI는 host OS의 JVM에서 실행된다. JVM에서 NIFI의 기본 components는 아래와 같다.<br>
![Architecture](/assets/nifi_image/nifiarchitecture.png)

- Web Server
    - NIFI의 http API를 호스팅한다.
- Flow Controller
    - 작업 수행 시 resource 사용에 대한 schedule을 관리한다.
- Extensions
    - NIFI가 제공하는 기본 processor 이외에 직접 processor를 개발하여 extension할 수 있다.
- FlowFile Repository
    - 활성화 FlowFile의 속성과 상태값(내용의 저장위치 등)을 저장하는 곳이다.
    - Default 접근 방식은 지정된 디스크 파티션에 위치하는 a persistent Write-Ahead Log(영구 미리쓰기 로그)이다.
- Content Repository
    - FlowFile의 실제 콘텐츠가 저장되는 곳이다.
    - Default 접근 방식은 FS(file system) 내에 데이터 블록을 저장하는 매우 간단한 메커니즘이다. Single volume에 대한 경합을 줄이기 위해 서로 다른 물리적 파티션을 사용하도록 FS(file system) 저장 위치를 지정할 수 있다.
- Provenance Repository
    - 모든 provenance 이벤트 데이터가 저장되는 곳, 다시 말해서 processor가 처리될 때 FlowFile의 이벤트가 저장되는 곳이다.
    - repo 구성은 하나 이상의 물리적 디스크 볼륨을 사용한다.
    - 이벤트 데이터는 인덱싱되고 검색 가능하다.
    - NIFI web UI에서 provenance 확인이 가능하다.

## NIFI Cluster
NIFI는 cluster 구성으로 동작할 수 있는데, 하나의 서버에서 처리하기 어려운 대량의 데이터를 병렬 처리할 수 있다. NIFI 1.0 릴리스부터 cluster(zero-leader clustering)을 사용할 수 있다. 
- zero-leader clustering
  - ZooKeeper가 임의로 NIFI 노드들 중 primary 노드와 cluster coordinator로 지정하는 것을 의미한다.
- cluster coordinator
  - 노드들의 정보(가동 여부, 상태 등)을 관리하며, 데이터플로우에 대한 모든 변경 사항(추가, 수정, 삭제 등)을 cluster의 모든 노드에 복제한다.
  - 장애 조치는 주키퍼에 의해 자동으로 처리된다.
  - 모든 노드는 heartbeat 및 상채 정보를 coordinator한테 리포트한다.
- Primary 노드
  - 모든 cluster는 하나의 primary 노드를 가지며 ZooKeeper에 의해 선택된다.
  - 여러 노드가 동시에 실행될 이유가 없을 때 해당 노드에서만 실행된다.

![nizero-leader-cluster](/assets/nifi_image/zero-leader-cluster.png)
