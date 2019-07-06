## Hadoop 이해와 활용

* BigData ?
하둡으로 처리할 데이터. Velocity, Variety, Volume
* Hadoop ?
데이터를 분산해서 저장하고, 분산된 데이터를 병렬처리 하는 것(플랫폼)
* HDFS 3종 SET
Namenode, Secondary Namenode, DataNode

- MasterNode : Supervised and manage the work. 관리자역할. 한 클러스터에 두개이상. 자원관리. SinglePoint of Failure 대비
```
   - Namenode, Secondary Namenode, JobTracker 로 구성되어있음.
   - Namenode : SlaveNode에 일을 시킴. 파일 시스템, 메타데이터 관리.
   - JobTracker : 사용자 Application 관리. 하둡클러스터에 하나만 존재
```
```
   - Secondary Namenode : Stand-by 아님. 항상 메타데이터 최신본 유지. 장애발생시 Secondary 정보로 Namenode 복원
   - HA(High Availability)가 되면 Secondary Namenode는 없음. 대신 Stand-by Namenode 가 그 역할을 하게됨
   - Stand-by Namenode가 죽으면 fs image 업데이트 X.
   - 저널노드 : 에딧로그를 자신이 실행되는 서버의 로컬디스크에 저장
```
- Slave/Worker Node : do the actual work. 필요 시 다수의 Worker 확장 가능.
```
   - DataNode, TaskTracker 로 구성되어있음
   - DataNode : 블록의 백업 저장소. 실제 데이터를 가지고있으며 Namenode에 지속적 보고.
                하나가 깨져도 사용자는 계속 데이터 읽을 수 있음. 하나의 파일은 여러 노드에 블록으로 분리되어 보관 (3copy)
```

- TaskTracker : 각 Slavenode에 할당된 작업 실행담당
- Namenode는 장애복구를 위해
```
   1) 메타데이터의 지속상태를 보완해주는 파일 백업
   2) Secondary Namenode운영. 주 Namenode 실패 시 사용될 네임스페이스 이미지복제본 유지
```
```
   - 메타데이터 이미지 : fs image(전체 Block 스냅샨 가지고있음. Namenode가 서비스), edits
```

* Resource Management
 [cpu, 메모리] -> Container 지원

* Hadoop 1.0 <-> 2.0 차이 : Yarn 유무

* Yarn
```
  - HDFS 상단에서 빅데이터용 어플리케이션을 실행하는 대용량 분산 운영체제 역할
  - Resource Manager, JOb Histroy Server, NodeManagers
```
- Resource Manager
```
   - 클러스터당 하나. 클러스터 전체를 관리하는 마스터서버 역할. 프로그램 요청 처리.
     리소스매니저는 클러스터에서 발생한 작업을 관리하는 어플리케이션 매니저 내장. 자원(CPU, Memory) 사용 경쟁 조율
   - Node Manager로부터 Heartbeat를 받음
   - Run a Scheduler
```
- Node Manager
```
   - AM과 Container(CPU, Memory)로 구성. 노드당 하나씩 존재. SlaveNode의 자원을 모니터링하고 관리하는 역할
     RM의 지시를 받아 작업요구사항에 따라 Container 생성
  - 데이터 노드가 있으면 노드 매니저도 있어야함
  - NodeManager가 죽으면 AM이 Falut tolerance
```
- ApplicationMaster
```
   - 작업당 하나씩 생성. Container를 사용해 모니터링과 실행 관리.
     RM과 작업에 대한 자원 요구사항 협상
```
참고 ) Container : CPU, Memory. 필요한 자원의 요청은 AM이 담당. 승인은 RM이 담당.
- Fault Tolerant : 리소스가 죽어도 다른 노드에서 작업 계속할 수 있도록 함

* Zookeeper
```
   - 분산 병렬 서비스 조정자/ 분산시스템 구성 서비스/ 동기화 서비스 / 분산 시스템을 위한 Naming 등록
   - Hadoop Namenode 에 대한 장애 조치
      : Active node가 죽는 경우, Stand-by Node가 자동기능
   - 현재 Active Node가 누구인지 정함
   - HA 상태정보를 저장하는 장소. 어떤 서버가 Active인지 Stand-by인지 저장
```

* Flume(Source, Channel, Sink), Kafka
```
   - 외부 데이터 소스들로부터 데이터 통합. 여러 시스템 로그 수집에 적합.
```
* Sqoop
```
   - RDMS와 HDFS간 데이터 Import, Export . JDBC로 DB연결
```
* Data Analysis
```
  - Hive, Pig, Impala
```
* Workflow Managemnet : oozie
```
   - 여러 처리단계에 대한 워크플로우 수행
   - Job 사이의 의존 관계를 정의
```
* NoSQL

* Spark
```
   - In Memory Data Computing
```

* HDFS
```
   - Hadoop 플랫폼의 저장 시스템. 확장성, 결함허용
   - 로그분석에는 좋지만 update가 안되는 단점이 있음
   - 파일업로드시 64 or 128 MB Block 에 저장.
   - 장애대처를 위해 3대 이상의 서버를 둠
   - 네임노드는 데이터노드 메타데이터 정보 저장
   - 데이터 노드는 네임노드에 하트비트를 주기적으로 보냄
```

* MapReduce
```
   - 분산 병렬 처리 방식
   - Mapper 단계 : HDFS 블록으로 나눠 분산처리
   - Shuffle, Sort : Mapper의 중간결과 통합, 정렬해서 Reducer에 전달
   - Reducer : Mapper 중간 결과로 최종 결과 수행시켜 HDFS에 저장
```
## NoSQL 이해와 활용

## Flume/Kafka

## Hive/oozie
