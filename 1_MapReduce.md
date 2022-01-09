## 1. MapReduce: Simplified Data Processing on Large Clusters (Programming Framework)

#### 1.1. Fault Tolerance란?

    Fault Tolerance system: 시스템에 고장이 발생해도 요구되는 기능을 정상적 혹은 부분적으로 수행해낼 수 있는 시스템. 

#### 1.2. MapReduce의 해결방법

    맵리듀스에서는 마스터가 정기적으로 heartbeat를 보내고, 정해진 시간 동안 응답이 오지 않으면 woker failure로 판정함. 
    이때. 해당 워커가 작업해놓은 map 작업의 결과는 해당 워커의 local disk에 저장되므로, 해당 작업은 전부 초기 상태로 다시 설정되어 다른 워커가 작업할 수 있게 됨.
    반면 reduce의 경우 작업의 결과가 global file system에 저장되므로 진행중이던 reduce 작업만 다시 실행시키면 된다.
    마스터의 경우, 정기적으로 checkpoint를 설정하여 task가 실패하는 경우 해당 checkpoint부터 시작할 수 있지만, 마스터가 여러 개가 아니라면 전체 맵리듀스 계산이 중단된다.

#### 1.3. Skipping Bad Record란? 그리고 어떻게 이것이 MapReduce 성능을 향상시키는지?

    Map/Reduce 처리 중 오류로 인해 해당 record에 대한 작업이 완료되지 않은 경우, 작업중인 워커가 마스터로 해당 record에 대한 sequence 번호를 담은 signal(UDP packet)을 보냄.
    마스터는 같은 레코드에서 이 signal을 두 번 받은 경우 모든 워커는 해당 record를 skip하게 됨.
    대용량 데이터에서 상대적으로 적은 일부의 record 때문에 계속되는 재실행으로 completion time이 늘어나는 것보다, 결과에 미치는 영향이 미미한 record를 skip하는 것이 더 나은 선택임.
