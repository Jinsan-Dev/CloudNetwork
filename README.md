# Cloud Network

클라우드 네트워크 관련 개념 및 key 논문 내용 정리

클라우드 컴퓨팅 : 자신의 컴퓨터가 아닌, 벤더에서 제공하는 컴퓨터 자원(네트워크, 서버, 스토리지, 애플리케이션, 서비스)을 인터넷을 통해 언제, 어디서나 접근할 수 있는 모델. 컴퓨터 자원을 구매하거나 소유할 필요 없이 필요한 만큼 on-demand로 사용하는 만큼 돈을 주고 쓰기 때문에 수요에 따라 유연하게 사용하는 컴퓨터 자원의 크기를 늘리거나 줄일 수 있음. 또한 벤더쪽에서 관리하기 때문에 유지보수를 신경쓰지 않아도 됨. 


## 1. MapReduce: Simplified Data Processing on Large Clusters (Programming Framework)

#### 1.1. Fault Tolerance 설명하고 MapReduce는 어떻게 그것을 해결하는가?

Fault Tolerance system: 시스템에 고장이 발생해도 요구되는 기능을 정상적 혹은 부분적으로 수행해낼 수 있는 시스템. 맵리듀스에서는 마스터가 정기적으로 heartbeat를 보내고, 정해진 시간 동안 응답이 오지 않으면 woker failure로 판정함. 이때. 해당 워커가 작업해놓은 map 작업의 결과는 해당 워커의 local disk에 저장되므로, 해당 작업은 전부 초기 상태로 다시 설정되어 다른 워커가 작업할 수 있게 됨. 반면 reduce의 경우 작업의 결과가 global file system에 저장되므로 진행중이던 reduce 작업만 다시 실행시키면 된다.

마스터의 경우, 정기적으로 checkpoint를 설정하여 task가 실패하는 경우 해당 checkpoint부터 시작할 수 있지만, 마스터가 여러 개가 아니라면 전체 맵리듀스 계산이 중단된다.

#### 1.2. Skipping Bad Record란? 그리고 어떻게 이것이 MapReduce 성능을 향상시키는지?

Map/Reduce 처리 중 오류로 인해 해당 record에 대한 작업이 완료되지 않은 경우, 작업중인 워커가 마스터로 해당 record에 대한 sequence 번호를 담은 signal(UDP packet)을 보냄. 마스터는 같은 레코드에서 이 signal을 두 번 받은 경우 모든 워커는 해당 record를 skip하게 됨. 대용량 데이터에서 상대적으로 적은 일부의 record 때문에 계속되는 재실행으로 completion time이 늘어나는 것보다, 결과에 미치는 영향이 미미한 record를 skip하는 것이 더 나은 선택임.


## 2. Mesos: A Platform for Fine-Grained Resource Sharing in the Data Center (Resource Allocators and Schedulers)

#### 2.1. Mesos의 설계 목적

클러스터 환경에서 프레임워크에 대한 효율적인 자원 분배를 위해, job에 있는 task 단위로 allocation하는 fine-grained sharing과, 프레임워크에게 사용가능한 자원 정보를 제공하고 어떤 자원을 쓰게 할지 결정하게 하는 resource offers를 사용함.

#### 2.2. 다른 프레임워크(스파크, 하둡 등)와의 차이

다른 프레임워크는 static partitioning을 통해 자원을 사용하므로 노드들의 utility를 최대한 활용하지 못하는 반면, 메소스는 dynamic sharing을 통해 클러스터 내부의 노드에서 idle 시간을 최소화함으로써 utility를 끌어올릴 수 있음.

#### 2.3.Resource Offer가 무엇인지? 그리고 이것의 이점은?

메소스의 Master 노드가 slave노드의 사용 가능한 자원 정보를 프레임워크에게 제공, 프레임워크가 자신의 job에 맞는 slave를 택할 수 있도록 함. 이를 통해 메소스를 최대한 simple하게 하고, 프레임워크가 자신에게 더 유리한 선택을 할 수 있게 함.(eg. data locality)


## 3. Dominant Resource Fairness: Fair Allocation of Multiple Resource Types (Resource Allocators and Schedulers)

#### 3.1. Max-min fairness와 Weighted Max-min fairness의 차이는?

max-min fairness : 자원을 1/n로 분배하되, task가 평균 이하의 자원을 필요로하는 경우 그 남은 자원을 나머지에 대해 균등하게 분배함. multi-resource에 대해서는 만족시킬 수 없음.

weighted max-min fairness : 그 남은 자원을 각 task의 가중치(dominant resource)에 따라 분배하므로 multi-resource에 대해 fair allocation 가능.

#### 3.2. Sharing guarantee와 Strategy proofness를 설명하시오

Sharing guarantee : 각 유저는 최소한 1/N(평균)의 자원을 받을 수 있으나, 만일 그보다 낮게 요구한다면, 평균보다 낮은 자원을 받는다.

Strategy proofness : 유저는 필요한 자원보다 더 요구해서 성능이 좋아지지 않는다.

*위 두 속성이 있어야 max-min fairness 합리적임.


## 4. Delay Scheduling: A Simple Technique for Achieving Locality and Fairness in Cluster Scheduling (Resource Allocators and Schedulers)

#### 4.1. Data locality와 Fairness의 트레이드 오프를 설명하시오.

Data locality를 만족시키려면 특정 노드로 load가 몰려 fairness가 깨지게 됨. fairness를 만족시키려다보면 클러스터 내 노드들 간에 load를 균등하게 주기 위해 task가 분산되고, locality를 만족시키지 못하게 됨.

#### 4.2. 스몰 딜레이가 왜 효율이 좋은지?

엄격하게 공평하도록 allocation을 하면 비효율적임. 이를 위해 작업 시작 전 약간의 딜레이를 두어 fairness를 약간 희생해 거의 100%에 가까운 locality 확보.


## 5. CONGA: Distributed Congestion-Aware Load Balancing for Datacenters (DC Load Balancing)

#### 5.1. ECMP, local congestion aware, global congestion aware 중 local congestion aware가 제일 효율이 안좋은 이유는?

local aware의 경우 한단계 상위 계층의 네트워크 상황을 파악하지 못한채 로컬 포트의 속도만 보고 통제를 한다. 따라서 혼잡한 path를 더욱 혼잡하게 만드는 상황을 초래하기도 한다.

#### 5.2 Flowlet이 무엇인가?, Per pkt이나 Per flow를 안쓰고 왜 플로우렛을 쓰는가?

flowlet이란 flow 보다 작으며 packet단위 보다는 큰 granurarity를 말한다. 하나의 flowlet은 tcp의 burst sending packet, 즉 한 window size가 된다.    
*flowlet switching이란 timeout 값보다 긴 시간 동안 하나의 flow가 idle한 상태가 되면 path를 변경하여 전송하는 것을 의미한다.

per 패킷은 패킷 리오더링을 발생시키고, per 플로우는 속도가 느려서 엘리펀트 (큰 플로우)와 마우스(작은 플로우) 밸런싱이 안되기 때문에 플로우렛을 사용한다.


## 6. (Incast) Safe and Effective Fine-grained TCP Retransmissions for Datacenter Communication (DCTCP)

#### 6.1. Incast가 무엇인가? 왜 데이터 센터에서 중요한가?

incast는 데이터 센터의 다대일 통신 패턴에서 발생하는 TCP 처리량 붕괴를 말한다.

많은 flow가 짧은 시간 내 스위치의 동일한 인터페이스에 쏠리면 스위치 메모리 용량을 넘어 패킷 손실이 발생할 수 있다. 요청을 받은 서버가 시간이 초과 될 경우, 다른 서버에서 응답을 받을 수 있지만, 클라이언트는 나머지 response를 받기 위해 최소 200ms(TCP 타임아웃)는 기다려야 한다.

#### 6.2. Queue build up 문제가 무엇인가? 어떻게 이 문제점을 처리하는가?

short flow가 large flows의 뒤에서 대기하므로 지연 시간이 늘어난다. 이를 위해 작은 버퍼를 써서 큐 딜레이를 줄인다.


## 7. Openflow: Enabling Innovation in Campus Network (SDN)

#### 7.1. 왜 실험 트래픽을 일반 트래픽으로부터 격리시킬려고 하는가?

연구원들은 그들의 트래픽을 조절할 수 있음, 이를 통해 새로운 라우팅 프로토콜이나, 보안 등에 대한 독립적인 실험이 가능함.


#### 7.2. Flow Table, Secure Channel, OpenFlow Protocol이 무엇인가?

Flow Table: 각 flow를 스위치에게 어떻게 처리하면 될지 알려주는 테이블 – flow entry와 action을 연결
Secure Channel: 스위치를 컨트롤러에 연결해 명령과 패킷을 보낼 수 있도록 해줌
OpenFlow Protocol: 컨트롤러가 스위치와 통신할 수 있게 해줌 / 플로우 테이블을 외부에서 정의할 수 있게 해줌
