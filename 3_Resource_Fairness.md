## 3. Dominant Resource Fairness: Fair Allocation of Multiple Resource Types (Resource Allocators and Schedulers)

#### 3.1. Max-min fairness와 Weighted Max-min fairness의 정의 및 두 방법의 차이는?

max-min fairness
    자원을 1/n로 분배하되, task가 평균 이하의 자원을 필요로하는 경우 그 남은 자원을 나머지에 대해 균등하게 분배함. multi-resource에 대해서는 만족시킬 수 없음.

weighted max-min fairness
    그 남은 자원을 각 task의 가중치(dominant resource)에 따라 분배하므로 multi-resource에 대해 fair allocation 가능.

#### 3.2. Sharing guarantee와 Strategy proofness를 설명하시오

Sharing guarantee
    각 유저는 최소한 1/N(평균)의 자원을 받을 수 있으나, 만일 그보다 낮게 요구한다면, 평균보다 낮은 자원을 받는다.

Strategy proofness
    유저는 필요한 자원보다 더 요구해서 성능이 좋아지지 않는다.

*위 두 속성이 있어야 max-min fairness 합리적임.
