Pod 생성 
-------------

새로운 Pod를 생성한다. pod는 bay의 minion 에 생성되는 docker container를 의미한다. cluster 구성을 위해서 필요한 정보인 menifest 파일은 kubernetes 에서 제공하는 yaml 템플릿으로 사용한다.

####  **Trigger Request**
아래와 같이 명령어 또는 RESTful로 Request를 전송한다.
```
magnum pod-create --manifest ./redis-master.yaml --bay testbay
```
```
curl -i -X POST -H 'X-Auth-Token: f44d6801b47a43f88dcf7ad2a3611e2c' -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'User-Agent: python-magnumclient' -d '{"manifest_url": null, "bay_uuid": "5821a2ae-df8c-47cd-8d48-5a3f4c6ad1f2", "manifest": "apiVersion: v1beta3\nkind: Pod\nmetadata:\n  labels:\n    name: redis\n    redis-sentinel: \"true\"\n    role: master\n  name: redis-master\nspec:\n  containers:\n    - name: master\n      image: kubernetes/redis:v1\n      env:\n        - name: MASTER\n          value: \"true\"\n      ports:\n        - containerPort: 6379\n      resources:\n        limits:\n          cpu: \"1\"\n      volumeMounts:\n        - mountPath: /redis-master-data\n          name: data\n    - name: sentinel\n      image: kubernetes/redis:v1\n      env:\n        - name: SENTINEL\n          value: \"true\"\n      ports:\n        - containerPort: 26379\n  volumes:\n    - name: data\n      emptyDir: {}\n"}' http://10.10.10.100:9511/v1/pods
```
#### **Controller API**
목록 호출 명령이 실행 되면 아래와 같이 Req가 전달된다.  
```
magnum.api.controllers.v1.pod.PodsController#post
magnum.conductor.api.API#pod_create
magnum.common.rpc_service.API#_call
oslo_messaging.rpc.client.RPCClient#call
oslo_messaging.rpc.client._CallContext#call
```
아래와 같이 메세지지가 rpc로 전달된다.
```
{'args': {'pod': {'magnum_object.changes': ['user_id', 'name', 'bay_uuid', 'labels', 'manifest', 'images', 'manifest_url', 'project_id'], 'magnum_object.version': '1.0', 'magnum_object.namespace': 'magnum', 'magnum_object.name': 'Pod', 'magnum_object.data': {'user_id': u'ee35a819c05a4a058a14268db1090e59', 'name': u'redis-master', 'bay_uuid': u'5821a2ae-df8c-47cd-8d48-5a3f4c6ad1f2', 'labels': {u'redis-sentinel': u'true', u'role': u'master', u'name': u'redis'}, 'manifest': u'apiVersion: v1beta3\nkind: Pod\nmetadata:\n  labels:\n    name: redis\n    redis-sentinel: "true"\n    role: master\n  name: redis-master\nspec:\n  containers:\n    - name: master\n      image: kubernetes/redis:v1\n      env:\n        - name: MASTER\n          value: "true"\n      ports:\n        - containerPort: 6379\n      resources:\n        limits:\n          cpu: "1"\n      volumeMounts:\n        - mountPath: /redis-master-data\n          name: data\n    - name: sentinel\n      image: kubernetes/redis:v1\n      env:\n        - name: SENTINEL\n          value: "true"\n      ports:\n        - containerPort: 26379\n  volumes:\n    - name: data\n      emptyDir: {}\n', 'images': [u'kubernetes/redis:v1', u'kubernetes/redis:v1'], 'manifest_url': None, 'project_id': u'b41b232354f44ec9bfa5ec091443cfa0'}}}, 'method': 'pod_create'}
```

#### <i class="icon-pencil"></i> **Conductor**  
아래와 같은 handler로 진입하고, 그 이후 Kubernetes로 명령이 전달된다. 
```
magnum.conductor.handlers.kube.Handler#pod_create
magnum.common.pythonk8sclient.client.ApivbetaApi.ApivbetaApi#createPod
```
전달되는 RESTful정보는 아래와 같다. 
```
resourcePath = '/api/v1beta3/namespaces/{namespaces}/pods'
resourcePath = resourcePath.replace('{format}', 'json')
method = 'POST'
```
전달되는 Payload정보는 아래와 같다.
```
{'kind': 'Pod', 'spec': {'containers': [{'name': 'master', 'env': [{'name': 'MASTER', 'value': 'true'}], 'image': 'kubernetes/redis:v1', 'volumeMounts': [{'mountPath': '/redis-master-data', 'name': 'data'}], 'ports': [{'containerPort': 6379}], 'resources': {'limits': {'cpu': '1'}}}, {'image': 'kubernetes/redis:v1', 'name': 'sentinel', 'env': [{'name': 'SENTINEL', 'value': 'true'}], 'ports': [{'containerPort': 26379}]}], 'volumes': [{'emptyDir': {}, 'name': 'data'}]}, 'apiVersion': 'v1beta3', 'metadata': {'labels': {'redis-sentinel': 'true', 'role': 'master', 'name': 'redis'}, 'name': 'redis-master'}}
```
생성 명령이 수행 된 후 SQL Alchemy에 의해서 저장된다.
```
magnum.objects.pod.Pod#create
```

-------------
copyright@Cha Dong-Hwi
