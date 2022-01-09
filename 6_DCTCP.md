## 6. (Incast) Safe and Effective Fine-grained TCP Retransmissions for Datacenter Communication (DCTCP)

#### 6.1. Incast가 무엇인가? 왜 데이터 센터에서 중요한가?

    incast는 데이터 센터의 다대일 통신 패턴에서 발생하는 TCP 처리량 붕괴를 말한다.
    많은 flow가 짧은 시간 내 스위치의 동일한 인터페이스에 쏠리면 스위치 메모리 용량을 넘어 패킷 손실이 발생할 수 있다.
    요청을 받은 서버가 시간이 초과 될 경우, 다른 서버에서 응답을 받을 수 있지만, 클라이언트는 나머지 response를 받기 위해 최소 200ms(TCP 타임아웃)는 기다려야 한다.

#### 6.2. Queue build up 문제가 무엇인가? 어떻게 이 문제점을 처리하는가?

    short flow가 large flows의 뒤에서 대기하므로 지연 시간이 늘어난다. 이를 위해 작은 버퍼를 써서 큐 딜레이를 줄인다.
