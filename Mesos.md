## 2. Mesos: A Platform for Fine-Grained Resource Sharing in the Data Center (Resource Allocators and Schedulers)

#### 2.1. Mesos의 설계 목적

    클러스터 환경에서 프레임워크에 대한 효율적인 자원 분배를 위해 설계됨.
    job에 있는 task 단위로 allocation하는 fine-grained sharing,
    프레임워크에게 사용가능한 자원 정보를 제공하고 어떤 자원을 쓰게 할지 결정하게 하는 resource offers를 사용함.

#### 2.2. 다른 프레임워크(스파크, 하둡 등)와의 차이점

    다른 프레임워크는 static partitioning을 통해 자원을 사용하므로 노드들의 utility를 최대한 활용하지 못하는 반면,
    메소스는 dynamic sharing을 통해 클러스터 내부의 노드에서 idle 시간을 최소화함으로써 utility를 끌어올릴 수 있음.

#### 2.3.Resource Offer와 이의 장점

    메소스의 Master 노드가 slave노드의 사용 가능한 자원 정보를 프레임워크에게 제공, 프레임워크가 자신의 job에 맞는 slave를 택할 수 있도록 함.
    이를 통해 메소스를 최대한 simple하게 하고, 프레임워크가 자신에게 더 유리한 선택을 할 수 있게 함.(eg. data locality)
