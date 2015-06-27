Bay 생성 
-------------

새로운 bay를 생성한다. bay는 Container Cluster의 Infrastructure가 되는 Compute 를 의미하고, 이때 Volume Storage, Network 도 같이 구축 되는데, 이때 baymodel을 참고해서 만들어진다. node count는 kubernetes의 minion node를 의미하고, master노드는 기본으로 1개가 생성된다. 

####  **Trigger Request**
아래와 같이 명령어 또는 RESTful로 Request를 전송한다.
```
magnum bay-create --name testbay --baymodel testbaymodel --node-count 1
```
```
curl -i -X POST -H 'X-Auth-Token: f1556971da0a491fba8b86234f1be9db' -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'User-Agent: python-magnumclient' -d '{"name": "testbay", "node_count": "1", "discovery_url": null, "baymodel_id": "891cd949-a057-4e7d-9fba-48d939647240", "bay_create_timeout": null}' http://10.10.10.100:9511/v1/bays
```
#### **Controller API**
목록 호출 명령이 실행 되면 아래와 같이 Req가 전달된다.  
```
magnum.api.controllers.v1.bay.BaysController#post
magnum.conductor.api.API#bay_create
magnum.common.rpc_service.API#_call
oslo_messaging.rpc.client.RPCClient#call
oslo_messaging.rpc.client._CallContext#call
```
아래와 같이 메세지지가 rpc로 전달된다.
```
{'args': {'bay_create_timeout': None, 'bay': {'magnum_object.changes': ['user_id', 'name', 'baymodel_id', 'node_count', 'project_id', 'discovery_url'], 'magnum_object.version': '1.0', 'magnum_object.namespace': 'magnum', 'magnum_object.name': 'Bay', 'magnum_object.data': {'user_id': u'ee35a819c05a4a058a14268db1090e59', 'name': u'testbay2', 'baymodel_id': u'891cd949-a057-4e7d-9fba-48d939647240', 'node_count': 1, 'project_id': u'b41b232354f44ec9bfa5ec091443cfa0', 'discovery_url': None}}}, 'method': 'bay_create'}
```

#### <i class="icon-pencil"></i> **Conductor**  
아래와 같은 handler로 진입하고, 그 이후 heat로 명령이 전달된다. 
```
magnum.conductor.handlers.bay_conductor.Handler#bay_create
magnum/conductor/handlers/bay_conductor.py:71
magnum.common.clients.OpenStackClients#heat
```

생성 된 후 SQL Alchemy에 의해서 저장되고, 이때 사용되는 Object의 데이터를 캡쳐 한것은 아래와 같다. 
```
magnum.objects.bay.Bay#create
```
```
bay = {Bay} <magnum.db.sqlalchemy.models.Bay object at 0x7ff36ae21090>
 _decl_class_registry = {instance} WeakValueDictionary: <WeakValueDictionary at 140683458814536>
 _extra_keys = {list} []
 _sa_class_manager = {ClassManager} <ClassManager of <class 'magnum.db.sqlalchemy.models.Bay'> at 7ff36b980868>
 _sa_instance_state = {InstanceState} <sqlalchemy.orm.state.InstanceState object at 0x7ff36ae28450>
 api_address = {NoneType} None
 baymodel_id = {unicode} u'891cd949-a057-4e7d-9fba-48d939647240'
 created_at = {datetime} 2015-06-26 15:19:00.255532
 discovery_url = {NoneType} None
 id = {long} 2
 metadata = {MetaData} MetaData(bind=None)
 name = {unicode} u'testbay'
 node_addresses = {NoneType} None
 node_count = {int} 1
 project_id = {unicode} u'b41b232354f44ec9bfa5ec091443cfa0'
 stack_id = {unicode} u'acf09639-4e6b-4e3b-91bd-82616236bf8f'
 status = {NoneType} None
 status_reason = {NoneType} None
 updated_at = {NoneType} None
 user_id = {unicode} u'ee35a819c05a4a058a14268db1090e59'
 uuid = {str} '016553fa-1e38-4c1f-b569-7df64920ddc2'
```
사용되는 템플릿은 아래에서 확인 할 수 있다.
> https://github.com/openstack/magnum/blob/stable/kilo/magnum/templates/heat-kubernetes/kubecluster.yaml
https://github.com/openstack/magnum/blob/master/magnum/templates/heat-kubernetes/kubeminion.yaml
 
최종적으로 node count 의 1개 더한 갯수 만큼 노드가 생성되고, 거기에 필요한 각종 network, volume등이 오픈스택에 생성된다.  

-------------
copyright@Cha Dong-Hwi
