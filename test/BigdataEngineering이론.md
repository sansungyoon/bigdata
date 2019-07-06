## Hadoop 이해와 활용

* BigData ?
하둡으로 처리할 데이터
* Hadoop ?
데이터를 분산해서 저장하고, 분산된 데이터를 병렬처리 하는 것(플랫폼)
* HDFS 3종 SET
Namenode, Secondary Namenode, DataNode

>- MasterNode : Supervised and manage the work. 관리자역할. 한 클러스터에 두개이상. 자원관리. SinglePoint of Failure 대비
   - Namenode, JobTracker 로 구성되어있음.
   - Namenode : SlaveNode에 일을 시킴. 메타데이터 관리
   - JobTracker : 사용자 Application 관리. 하둡클러스터에 하나만 존재
- Slave/Worker Node : do the actual work. 필요 시 다수의 Worker 확장 가능.
   - DataNode, TaskTracker 로 구성되어있음
   - DataNode : 블록의 백업 저장소. Namenode에 지속적 보고. 하나가 깨져도 사용자는 계속 데이터 읽을 수 있음
- TaskTracker : 각 Slavenode에 할당된 작업 실행담당
- Namenode는 장애복구를 위해
   1) 메타데이터의 지속상태를 보완해주는 파일 백업
   2) Secondary Namenode운영. 주 Namenode 실패 시 사용될 네임스페이스 이미지복제본 유지

* Resource Management
 [cpu, 메모리] -> Container 지원

* Hadoop 1.0 <-> 2.0 차이 : Yarn 유무

* Yarn : HDFS 상단에서 빅데이터용 어플리케이션을 실행하는 대용량 분산 운영체제 역할

- Resource Manager
   - 클러스터당 하나. 클러스터 전체를 관리하는 마스터서버 역할. 프로그램 요청 처리.
     리소스매니저는 클러스터에서 발생한 작업을 관리하는 어플리케이션 매니저 내장. 자원(CPU, Memory) 사용 경쟁 조율
- Node Manager
   - AM과 Container(CPU, Memory)로 구성. 노드당 하나씩 존재. SlaveNode의 자원을 모니터링하고 관리하는 역할
     RM의 지시를 받아 작업요구사항에 따라 Container 생성
- ApplicationMaster
   - 작업당 하나씩 생성. Container를 사용해 모니터링과 실행 관리.
     RM과 작업에 대한 자워 ㄴ요구사항 협상
참고 ) Container : CPU, Memory. 필요한 자원의 요청은 AM이 담당. 승인은 RM이 담당.
- Fault Tolerant : 리소스가 죽어도 다른 노드에서 작업 계속할 수 있도록 함

* Zookeeper
