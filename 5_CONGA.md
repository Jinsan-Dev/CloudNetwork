## 5. CONGA: Distributed Congestion-Aware Load Balancing for Datacenters (DC Load Balancing)

#### 5.1. ECMP, local congestion aware, global congestion aware 중 local congestion aware가 제일 효율이 안좋은 이유는?

    local aware의 경우 한단계 상위 계층의 네트워크 상황을 파악하지 못한채 로컬 포트의 속도만 보고 통제를 한다. 따라서 혼잡한 path를 더욱 혼잡하게 만드는 상황을 초래하기도 한다.

#### 5.2 Flowlet이 무엇인가?, Per pkt이나 Per flow를 안쓰고 왜 플로우렛을 쓰는가?

    flowlet이란 flow 보다 작으며 packet단위 보다는 큰 granurarity를 말한다. 하나의 flowlet은 tcp의 burst sending packet, 즉 한 window size가 된다.    
    *flowlet switching이란 timeout 값보다 긴 시간 동안 하나의 flow가 idle한 상태가 되면 path를 변경하여 전송하는 것을 의미한다.
    per 패킷은 패킷 리오더링을 발생시키고, per 플로우는 속도가 느려서 엘리펀트 (큰 플로우)와 마우스(작은 플로우) 밸런싱이 안되기 때문에 플로우렛을 사용한다.
