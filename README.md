# Cloud Network

클라우드 네트워크 관련 개념 및 논문 key 내용 정리

클라우드 컴퓨팅 : 자신의 컴퓨터가 아닌, 벤더에서 제공하는 컴퓨터 자원(네트워크, 서버, 스토리지, 애플리케이션, 서비스)을 인터넷을 통해 언제, 어디서나 접근할 수 있는 모델. 컴퓨터 자원을 구매하거나 소유할 필요 없이 필요한 만큼 on-demand로 사용하는 만큼 돈을 주고 쓰기 때문에 수요에 따라 유연하게 사용하는 컴퓨터 자원의 크기를 늘리거나 줄일 수 있음. 또한 벤더쪽에서 관리하기 때문에 유지보수를 신경쓰지 않아도 됨. 


## 1. 프로그래밍 프레임워크 (Programming Framework)

### MapReduce: Simplified Data Processing on Large Clusters

#### Fault Tolerance 설명하고 MapReduce는 어떻게 그것을 해결하는지?

Fault Tolerance system: 시스템에 고장이 발생해도 요구되는 기능을 정상적 혹은 부분적으로 수행해낼 수 있는 시스템. 맵리듀스에서는 마스터가 정기적으로 heartbeat를 보내고, 정해진 시간 동안 응답이 오지 않으면 woker failure로 판정함. 이때. 해당 워커가 작업해놓은 map 작업의 결과는 해당 워커의 local disk에 저장되므로, 해당 작업은 전부 초기 상태로 다시 설정되어 다른 워커가 작업할 수 있게 됨. 반면 reduce의 경우 작업의 결과가 global file system에 저장되므로 진행중이던 reduce 작업만 다시 실행시키면 된다.

마스터의 경우, 정기적으로 checkpoint를 설정하여 task가 실패하는 경우 해당 checkpoint부터 시작할 수 있지만, 마스터가 여러 개가 아니라면 전체 맵리듀스 계산이 중단된다.

#### Skipping Bad Record란? 그리고 어떻게 이것이 MapReduce 성능을 향상시키는지?

Map/Reduce 처리 중 오류로 인해 해당 record에 대한 작업이 완료되지 않은 경우, 작업중인 워커가 마스터로 해당 record에 대한 sequence 번호를 담은 signal(UDP packet)을 보냄. 마스터는 같은 레코드에서 이 signal을 두 번 받은 경우 모든 워커는 해당 record를 skip하게 됨. 대용량 데이터에서 상대적으로 적은 일부의 record 때문에 계속되는 재실행으로 completion time이 늘어나는 것보다, 결과에 미치는 영향이 미미한 record를 skip하는 것이 더 나은 선택임.

## 2. Resource Allocators and Schedulers

### Mesos: A Platform for Fine-Grained Resource Sharing in the Data Center

설계 목적: 클러스터 환경에서 프레임워크에 대한 효율적인 자원 분배를 위해, job에 있는 task 단위로 allocation하는 fine-grained sharing과, 프레임워크에게 사용가능한 자원 정보를 제공하고 어떤 자원을 쓰게 할지 결정하게 하는 resource offers를 사용함.

다른 프레임워크(스파크, 하둡 등)와의 차이: 다른 프레임워크는 static partitioning을 통해 자원을 사용하므로 노드들의 utility를 최대한 활용하지 못하는 반면, 메소스는 dynamic sharing을 통해 클러스터 내부의 노드에서 idle 시간을 최소화함으로써 utility를 끌어올릴 수 있음.

## 3. DCTCP

#### pHost: Distributed Near-optimal Datacenter Transport over Commodity Network Fabric

## 4. DC Load balancing

#### CONGA: Distributed Congestion-Aware Load Balancing for Datacenters


## 5. SDN

#### Openflow: Enabling Innovation in Campus Network
