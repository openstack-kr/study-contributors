Magnum API 분석  
-------------

Magnum에 대해서 깊은 이해를 위해서는 아래와 같은 이해가 필요합니다.

1. 코드의 흐름을 읽고, 
2. magnum-api와 magnum-conductor의 역할이해 

이를 위해서 중요 API에 대한 코드 플로우가 정리 되어있습니다. 대략적인 순서는 아래와 같으며, 개별 API마다 차이가 있습니다. 

* CLI 또는 RESTful 호출
* API Controller 
* Conductor
* Kubernetes API Call
* Database 

Conductor를 거치는 경우는 MQ를 통해서 전달되는 Payload 를 이해하면 Loosely Coupled한 오픈스택의 아키텍쳐를 이해 하기 쉽습니다. 

개별 API는 다른 것에 종속 적이며, 생성 순서는 아래와 같습니다. 이 도큐먼트에서는 Kubernetes에 대한것만 다루고 있습니다. (Container와 Node는 Swarm을 이용 하는 부분으로 제외)

1. Baymodel
2. Bay
3. Pod
4. Service
5. RC

개별 API의 코드 flow는 다음과 같습니다. 
* [baymodel list]
* [baymodel create]
* [bay list]
* [bay create]
* [pod list]
* [pod create]
* [service list]
* [service create]
* [rc list]
* [rc create]

Magnum 의 아키텍쳐는 아래와 같습니다. 
![architecture of magnum](https://github.com/openstack-kr/study-contributors/blob/master/3.apis/files/magnum-architectue.png)

Heat 의 Stack Topology는 아래와 같습니다. 
![stack topology](https://github.com/openstack-kr/study-contributors/blob/master/3.apis/files/heat-stack-diagram.png)

네트워크 아키텍쳐는 아래와 같습니다. 

![network architecture of magnum minions]( https://github.com/openstack-kr/study-contributors/blob/master/3.apis/files/magnum-network.png )

[baymodel list]:https://github.com/openstack-kr/study-contributors/blob/master/3.apis/1.baymodel/baymodel-list.md
[baymodel create]:https://github.com/openstack-kr/study-contributors/blob/master/3.apis/1.baymodel/baymodel-create.md
[bay list]:https://github.com/openstack-kr/study-contributors/blob/master/3.apis/2.bay/bay-list.md
[bay create]:https://github.com/openstack-kr/study-contributors/blob/master/3.apis/2.bay/bay-create.md
[pod list]:https://github.com/openstack-kr/study-contributors/blob/master/3.apis/3.pod/pod-list.md
[pod create]:https://github.com/openstack-kr/study-contributors/blob/master/3.apis/3.pod/pod-create.md
[service list]:https://github.com/openstack-kr/study-contributors/blob/master/3.apis/4.service/service-list.md
[service create]:https://github.com/openstack-kr/study-contributors/blob/master/3.apis/4.service/service-create.md
[rc list]:https://github.com/openstack-kr/study-contributors/blob/master/3.apis/5.rc/rc-list.md
[rc create]:https://github.com/openstack-kr/study-contributors/blob/master/3.apis/5.rc/rc-create.md

