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
[Apache NiFi docs로 이동](https://nifi.apache.org/docs.html)
## Apache NiFi 소개
apache에서는 'Apache NiFi supports powerful and scalable directed graphs of data routing, transformation, and system mediation logic.'라고 설명하고, 위키백과에서는 '소프트웨어 시스템 간 데이터 흐름을 자동화하도록 설계된 아파치 소프트웨어 재단의 소프트웨어 프로젝트'라고 설명한다.

NSA(National Security Agency)에서 apache에 기증한 데이터플로우 엔진이며, 대용량 데이터를 수집 및 처리할 수 있는 ETL 툴의 일종이며, FBP 개념이 적용되었다고 한다.<br>
`FBP(Flow Based Programming): 데이터플로우 프로세스를 미리 구축하고 이를 유지하면서 데이터를 교환하는 프로그래밍 패러다임`

MiNiFi라는 프로젝트를 함께 활용하여 원격지로부터 데이터를 수집할 수 있다. 다수의 원격지가 존재하더라도 모든 데이터 수집 흐름은 연동된 NIFI가 관리한다.<br>
`ex) 데이터 원격지(MiNiFi) -> NiFi -> hadoop`

## NiFi 주요 컨셉
아래는 FBP에서 사용하는 단어들에 매핑되는 NIFI만의 독자적인 단어들이다.

|NiFi Term|FBP Term|Description|
|------|---|---|
|FlowFile|Information Packet|FlowFile은 NiFi에서 처리되는 기본 단위이다. FlowFile은 시스템을 통해 이동하는 각 객체(데이터)를 나타내며, 각 객체(데이터)를 처리할 때 필요한 key/value 속성값과 0바이트 이상의 콘텐츠로 구성되어 있고 NiFi는 이에 대한 모든 정보를 남긴다(추적한다).|
|FlowFile Processor|Black Box|Processor는 실제로 작업(work)을 수행한다(perform). Processor는 시스템 간의 data routing, transformation 또는 mediation 조합을 수행한다. Processor는 지정된 FlowFile 및 해당 콘텐츠 스트림의 속성에 액세스할 수 있다. Processor는 주어진 작업 단위에서 0개 이상의 FlowFiles에서 작동하고 해당 작업을 commit하거나 rollback할 수 있다.|
|Connection|Bounded Buffer|Connection은 processor 간의 실제 연결을 제공한다. 이들은 queue 역할을 하며 다양한 process들이 서로 다른 rates로 상호 작용할 수 있다. 이러한 queue는 동적으로 우선 순위를 지정할 수 있으며 load할 때 상한(upper bounds)을 가질 수 있으므로 back pressure가 가능하다.|
|Flow Controller|Scheduler|Flow Controller는 NiFi의 scheduler로 process가 연결하는 방법에 대한 지식을 유지하고 모든 process가 사용하는 스레드와 할당을 관리한다. Flow Controller는 process 간의 FlowFile 교환을 용이하게 하는 brocker 역할한다.|
|Process Group|subnet|Process Group은 input ports를 통해 데이터를 수신하고 output ports를 통해 데이터를 보낼 수 있는 특정 process 및 해당 connection의 집합이다. 이러한 방식으로 process group을 사용하면 다른 components를 구성하는 것만으로 완전히 새로운 components를 생성할 수 있다.|

## NiFi Architecture
NiFi는 host OS의 JVM 내에서 실행된다. JVM에서 NiFi의 기본 components는 아래와 같다.

![Architecture](/assets/nifi_image/nifiarchitecture.png)

- Web Server
    - 웹서버의 목적은 NIFI의 http 기반 명령 및 제어 API를 호스팅하는 것이다.
- Flow Controller
    - 작업(operation)의 두뇌 기능을 하며, extension을 실행할 쓰레드를 제공하고 extension이 실행을 위해 resource를 제공 받을 때의 schedule을 관리한다.
- Extensions
    - NiFi가 제공하는 기본 processor 이외에 직접 processor를 개발하여 extension할 수 있으며, 핵심은 Extension은 JVM 내에서 동작하고 실행한다는 것이다.
- FlowFile Repository
    - NiFi가 현재 flow에서 주어진 활성화 FlowFile의 속성과 상태값(내용의 저장위치 등)을 저장하는(keep track) 곳이며, repo의 구현(implementation)은 pluggable하다.
    - Default 접근 방식은 지정된 디스크 파티션에 위치하는 a persistent Write-Ahead Log(영구 미리쓰기 로그)이다.
- Content Repository
    - 주어진 FlowFile의 실제 콘텐츠 바이트(byte)가 저장되는 곳이며, repo의 구현(implementation)은 pluggable하다.
    - Default 접근 방식은 FS(file system) 내에 데이터 블록을 저장하는 매우 간단한 메커니즘이다. Single volume에 대한 경합(contention)을 줄이기 위해 서로 다른 물리적 파티션을 사용하도록 FS(file system) 저장 위치를 지정할 수 있다.
- Provenance Repository
    - 모든 provenance 이벤트 데이터가 저장되는 곳, 다시 말해서 processor가 처리될 때 FlowFile의 이벤트가 저장되는 곳이다.
    - repo 구성은 하나 이상의 물리적 디스크 볼륨을 사용하는 기본 구현(implementation)으로 pluggable하다. 
    - 각 위치 내에서 이벤트 데이터는 인덱싱되고 검색 가능하다.
    - 실제로 NiFi web UI 우측 삼단 메뉴 클릭하여 provenance 들어가서 확인이 가능하다.

## NIFI Cluster
NIFI는 cluster 내에서도 동작할 수 있는데, 하나의 서버에서 처리가 어려운 대량의 데이터를 병렬적으로 처리가 가능하다. NIFI 1.0 릴리스부터 zero-leader clustering이 사용된다. 
- zero-leader clustering
  - ZooKeeper에 의해 NiFi 노드들 중 각각 하나의 노드를 primary 노드와 cluster coordinator로 지정하는 것을 의미한다.
- cluster coordinator
  - 노드들의 정보(가동 여부, 상태 등)을 관리하며, 데이터플로우에 대한 모든 변경 사항(추가, 수정, 삭제 등)을 cluster의 모든 노드에 복제한다.
  - 장애 조치는 주키퍼에 의해 자동으로 처리된다.
  - 모든 노드는 coordinator에게 heartbeat 및 상채 정보를 보고한다.
- Primary 노드
  - 모든 cluster는 하나의 primary 노드를 가지며 ZooKeeper에 의해 선택된다. 여러 노드가 동시에 실행될 이유가 없을 때 해당 노드에서만 실행된다.

![nizero-leader-cluster](/assets/nifi_image/zero-leader-cluster.png)