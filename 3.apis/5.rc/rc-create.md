ReplicationController 생성 
-------------

새로운 RC를 생성한다. 생성되어있는 Service의 Replication을 설치한다.  이때 필요한 정보인 manifest 파일은 kubernetes 에서 제공하는 yaml 템플릿으로 사용한다.

####  **Trigger Request**
아래와 같이 명령어 또는 RESTful로 Request를 전송한다.
```
magnum rc-create --manifest ./redis-controller.yaml --bay testbay
```
```
curl -i -X POST -H 'X-Auth-Token: 7b09f07122ac46759a5ae75b18c358a7' -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'User-Agent: python-magnumclient' -d '{"manifest_url": null, "bay_uuid": "5821a2ae-df8c-47cd-8d48-5a3f4c6ad1f2", "manifest": "apiVersion: v1beta3\nkind: ReplicationController\nmetadata:\n  name: redis\nspec:\n  replicas:  2\n  selector:\n    name: redis\n  template:\n    metadata:\n      labels:\n        name: redis\n    spec:\n      containers:\n      - name: redis\n        image: kubernetes/redis:v1\n        ports:\n        - containerPort: 6379\n        resources:\n          limits:\n            cpu: \"1\"\n        volumeMounts:\n        - mountPath: /redis-master-data\n          name: data\n      volumes:\n        - name: data\n          emptyDir: {}\n\n"}' http://10.10.10.100:9511/v1/rcs
```
#### **Controller API**
목록 호출 명령이 실행 되면 아래와 같이 Req가 전달된다.  
```
magnum.api.controllers.v1.replicationcontroller.ReplicationControllersController#post
magnum.conductor.api.API#rc_create
magnum.common.rpc_service.API#_call

oslo_messaging.rpc.client.RPCClient#call
oslo_messaging.rpc.client._CallContext#call
```
아래와 같이 메세지지가 rpc로 전달된다.
```
{'args': {'rc': {'magnum_object.changes': ['user_id', 'name', 'replicas', 'bay_uuid', 'labels', 'manifest', 'images', 'manifest_url', 'project_id'], 'magnum_object.version': '1.0', 'magnum_object.namespace': 'magnum', 'magnum_object.name': 'ReplicationController', 'magnum_object.data': {'user_id': u'ee35a819c05a4a058a14268db1090e59', 'name': u'redis', 'replicas': 2, 'bay_uuid': u'5821a2ae-df8c-47cd-8d48-5a3f4c6ad1f2', 'labels': {u'name': u'redis'}, 'manifest': u'apiVersion: v1beta3\nkind: ReplicationController\nmetadata:\n  name: redis\nspec:\n  replicas:  2\n  selector:\n    name: redis\n  template:\n    metadata:\n      labels:\n        name: redis\n    spec:\n      containers:\n      - name: redis\n        image: kubernetes/redis:v1\n        ports:\n        - containerPort: 6379\n        resources:\n          limits:\n            cpu: "1"\n        volumeMounts:\n        - mountPath: /redis-master-data\n          name: data\n      volumes:\n        - name: data\n          emptyDir: {}\n\n', 'images': [u'kubernetes/redis:v1'], 'manifest_url': None, 'project_id': u'b41b232354f44ec9bfa5ec091443cfa0'}}}, 'method': 'rc_create'}
```

#### <i class="icon-pencil"></i> **Conductor**  
아래와 같은 handler로 진입하고, 그 이후 Kubernetes로 명령이 전달된다. 
```
magnum.conductor.handlers.kube.Handler#rc_create
magnum.common.pythonk8sclient.client.ApivbetaApi.ApivbetaApi#createReplicationController
magnum.common.pythonk8sclient.client.swagger.ApiClient#callAPI
```
전달되는 RESTful정보는 아래와 같다. 
```
resourcePath = '/api/v1beta3/namespaces/{namespaces}/replicationcontrollers'
resourcePath = resourcePath.replace('{format}', 'json')
method = 'POST'
```
전달되는 Payload정보는 아래와 같다.
```
{'kind': 'ReplicationController', 'spec': {'replicas': 2, 'template': {'spec': {'containers': [{'image': 'kubernetes/redis:v1', 'volumeMounts': [{'mountPath': '/redis-master-data', 'name': 'data'}], 'name': 'redis', 'resources': {'limits': {'cpu': '1'}}, 'ports': [{'containerPort': 6379}]}], 'volumes': [{'emptyDir': {}, 'name': 'data'}]}, 'metadata': {'labels': {'name': 'redis'}}}, 'selector': {'name': 'redis'}}, 'apiVersion': 'v1beta3', 'metadata': {'name': 'redis'}}
```
생성 명령이 수행 된 후 SQL Alchemy에 의해서 저장된다.
```
magnum.objects.service.Service#create
```

-------------
copyright@Cha Dong-Hwi
