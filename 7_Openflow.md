## 7. Openflow: Enabling Innovation in Campus Network (SDN)

#### 7.1. 왜 실험 트래픽을 일반 트래픽으로부터 격리시킬려고 하는가?

    연구원들은 그들의 트래픽을 조절할 수 있음, 이를 통해 새로운 라우팅 프로토콜이나, 보안 등에 대한 독립적인 실험이 가능함.


#### 7.2. Flow Table, Secure Channel, OpenFlow Protocol은?

    Flow Table: 각 flow를 스위치에게 어떻게 처리하면 될지 알려주는 테이블 – flow entry와 action을 연결
    Secure Channel: 스위치를 컨트롤러에 연결해 명령과 패킷을 보낼 수 있도록 해줌
    OpenFlow Protocol: 컨트롤러가 스위치와 통신할 수 있게 해줌 / 플로우 테이블을 외부에서 정의할 수 있게 해줌
