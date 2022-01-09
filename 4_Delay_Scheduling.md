## 4. Delay Scheduling: A Simple Technique for Achieving Locality and Fairness in Cluster Scheduling (Resource Allocators and Schedulers)

#### 4.1. Data locality와 Fairness의 트레이드 오프를 설명하시오.

    Data locality를 만족시키려면 특정 노드로 load가 몰려 fairness가 깨지게 됨.
    fairness를 만족시키려다보면 클러스터 내 노드들 간에 load를 균등하게 주기 위해 task가 분산되고, locality를 만족시키지 못하게 됨.

#### 4.2. 스몰 딜레이가 왜 효율이 좋은 이유는?

    엄격하게 공평하도록 allocation을 하면 비효율적임. 이를 위해 작업 시작 전 약간의 딜레이를 두어 fairness를 약간 희생해 거의 100%에 가까운 locality 확보.
