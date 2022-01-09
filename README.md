# Cloud Network

클라우드 네트워크 관련 개념 및 key 논문 내용 정리

클라우드 컴퓨팅 : 자신의 컴퓨터가 아닌, 벤더에서 제공하는 컴퓨터 자원(네트워크, 서버, 스토리지, 애플리케이션, 서비스)을 인터넷을 통해 언제, 어디서나 접근할 수 있는 모델. 컴퓨터 자원을 구매하거나 소유할 필요 없이 필요한 만큼 on-demand로 사용하는 만큼 돈을 주고 쓰기 때문에 수요에 따라 유연하게 사용하는 컴퓨터 자원의 크기를 늘리거나 줄일 수 있음. 또한 벤더쪽에서 관리하기 때문에 유지보수를 신경쓰지 않아도 됨. 





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
