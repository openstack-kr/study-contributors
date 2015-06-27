BayModel 생성 
-------------

새로운 bay model을 생성한다. bay model은 bay를 만드는데 사용되는 기본 spec정보이다. nova의 flavor와 유사한 형태. 기본 정보는 bay를 만들 때  사용할 이미지 이름, keypair, network, flavor, 볼륨, COE등이 있다. 이때 COE는  container orchestration engine 라는 의미로, 이 타입에 따라서 사용될 heat template이 결정된다. 

####  **Trigger Request**
아래와 같이 명령어 또는 RESTful로 Request를 전송한다.
```
magnum --debug baymodel-create 
--name testbaymodel2 
--image-id fedora-21-atomic-3                        
--keypair-id testkey                        
--external-network-id $NIC_ID                        
--dns-nameserver 8.8.8.8 
--flavor-id m1.small  
--docker-volume-size 5 
--coe kubernetes
```
```
curl -i -X POST -H 'X-Auth-Token: 72f50e77448540eda0dba4f749a6d56d' -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'User-Agent: python-magnumclient' -d '{"ssh_authorized_key": null, "docker_volume_size": "5", "external_network_id": "2c984f03-f167-48a7-95a8-54b78932871b", "name": "testbaymodel2", "fixed_network": null, "image_id": "fedora-21-atomic-3", "coe": "kubernetes", "keypair_id": "testkey", "master_flavor_id": null, "dns_nameserver": "8.8.8.8", "flavor_id": "m1.small"}' http://10.10.10.100:9511/v1/baymodels
```
#### **Controller API**
목록 호출 명령이 실행 되면 아래와 같이 Req가 전달된다.  
```
magnum.api.controllers.v1.baymodel.BayModelsController#post
magnum.objects.baymodel.BayModel#create

```
#### <i class="icon-pencil"></i> **Database**  
아래와 같은 database api로 진입하고, 그 이후 Insert가 실행 된다. 
```
magnum.db.sqlalchemy.api.Connection#create_baymodel
```
SQL Alchemy에 의해서 저장되고, 이때 사용되는 Object의 데이터를 캡쳐 한것은 아래와 같다. .
```
baymodel = {BayModel} <magnum.db.sqlalchemy.models.BayModel object at 0x7f9b0d7bcfd0>
 _decl_class_registry = {instance} WeakValueDictionary: <WeakValueDictionary at 140303932234928>
 _extra_keys = {list} []
 _sa_class_manager = {ClassManager} <ClassManager of <class 'magnum.db.sqlalchemy.models.BayModel'> at 7f9b0dfd6ab8>
 _sa_instance_state = {InstanceState} <sqlalchemy.orm.state.InstanceState object at 0x7f9b0d774250>
 apiserver_port = {NoneType} None
 cluster_distro = {unicode} u'fedora-atomic'
 coe = {unicode} u'kubernetes'
 created_at = {datetime} 2015-06-26 14:46:12.976138
 dns_nameserver = {unicode} u'8.8.8.8'
 docker_volume_size = {int} 5
 external_network_id = {unicode} u'2c984f03-f167-48a7-95a8-54b78932871b'
 fixed_network = {NoneType} None
 flavor_id = {unicode} u'm1.small'
 id = {long} 5
 image_id = {unicode} u'fedora-21-atomic-3'
 keypair_id = {unicode} u'testkey'
 master_flavor_id = {NoneType} None
 metadata = {MetaData} MetaData(bind=None)
 name = {unicode} u'testbaymodel2'
 project_id = {unicode} u'b41b232354f44ec9bfa5ec091443cfa0'
 ssh_authorized_key = {NoneType} None
 updated_at = {NoneType} None
 user_id = {unicode} u'ee35a819c05a4a058a14268db1090e59'
 uuid = {str} 'a9003f04-c143-4948-a473-00eccd5eaf34' 
```

-------------
copyright@Cha Dong-Hwi
