Service 생성 
-------------

새로운 Service를 생성한다. 생성되어있는 minion 에 service 를 설치한다.  이때 필요한 정보인 manifest 파일은 kubernetes 에서 제공하는 yaml 템플릿으로 사용한다.

####  **Trigger Request**
아래와 같이 명령어 또는 RESTful로 Request를 전송한다.
```
magnum service-create --manifest ./redis-sentinel-service.yaml --bay testbay
```
```
curl -i -X POST -H 'X-Auth-Token: 1a1da36773ce45f89f4dee5bf2ce7d7a' -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'User-Agent: python-magnumclient' -d '{"manifest_url": null, "bay_uuid": "5821a2ae-df8c-47cd-8d48-5a3f4c6ad1f2", "manifest": "apiVersion: v1beta3\nkind: Service\nmetadata:\n  labels:\n    name: sentinel\n    role: service\n  name: redis-sentinel\nspec:\n  ports:\n    - port: 26379\n      targetPort: 26379\n  selector:\n    redis-sentinel: \"true\"\n"}' http://10.10.10.100:9511/v1/services
```
#### **Controller API**
목록 호출 명령이 실행 되면 아래와 같이 Req가 전달된다.  
```
magnum.api.controllers.v1.service.ServicesController#post
magnum.conductor.api.API#service_create
magnum.common.rpc_service.API#_call

oslo_messaging.rpc.client.RPCClient#call
oslo_messaging.rpc.client._CallContext#call
```
아래와 같이 메세지지가 rpc로 전달된다.
```
{'args': {'service': {'magnum_object.changes': ['selector', 'user_id', 'name', 'bay_uuid', 'labels', 'manifest', 'manifest_url', 'project_id', 'ports'], 'magnum_object.version': '1.0', 'magnum_object.namespace': 'magnum', 'magnum_object.name': 'Service', 'magnum_object.data': {'user_id': u'ee35a819c05a4a058a14268db1090e59', 'name': u'redis-sentinel', 'bay_uuid': u'5821a2ae-df8c-47cd-8d48-5a3f4c6ad1f2', 'labels': {u'role': u'service', u'name': u'sentinel'}, 'selector': {u'redis-sentinel': u'true'}, 'manifest': u'apiVersion: v1beta3\nkind: Service\nmetadata:\n  labels:\n    name: sentinel\n    role: service\n  name: redis-sentinel\nspec:\n  ports:\n    - port: 26379\n      targetPort: 26379\n  selector:\n    redis-sentinel: "true"\n', 'manifest_url': None, 'project_id': u'b41b232354f44ec9bfa5ec091443cfa0', 'ports': [{u'targetPort': 26379, u'port': 26379}]}}}, 'method': 'service_create'}
```

#### <i class="icon-pencil"></i> **Conductor**  
아래와 같은 handler로 진입하고, 그 이후 Kubernetes로 명령이 전달된다. 
```
magnum.conductor.handlers.kube.Handler#service_create
magnum.common.pythonk8sclient.client.ApivbetaApi.ApivbetaApi#createService
magnum.common.pythonk8sclient.client.swagger.ApiClient#callAPI
```
전달되는 RESTful정보는 아래와 같다. 
```
resourcePath = '/api/v1beta3/namespaces/{namespaces}/services'
resourcePath = resourcePath.replace('{format}', 'json')
method = 'POST'
```
전달되는 Payload정보는 아래와 같다.
```
{'kind': 'Service', 'spec': {'ports': [{'targetPort': 26379, 'port': 26379}], 'selector': {'redis-sentinel': 'true'}}, 'apiVersion': 'v1beta3', 'metadata': {'labels': {'role': 'service', 'name': 'sentinel'}, 'name': 'redis-sentinel'}}
```
생성 명령이 수행 된 후 SQL Alchemy에 의해서 저장된다.
```
magnum.objects.service.Service#create
```

-------------
copyright@Cha Dong-Hwi
