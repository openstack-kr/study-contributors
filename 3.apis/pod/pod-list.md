Pod 목록 출력 
-------------

기 생성된 Pod 목록을 출력한다. 

####  **Trigger Request**
아래와 같이 명령어 또는 RESTful로 Request를 전송한다.
```
magnum pod-list
```
```
curl -i -X GET -H 'X-Auth-Token: 03dfe209d0694a46bdaaa71a6b8a52df' -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'User-Agent: python-magnumclient' http://20.20.20.100:9511/v1/pods
```
#### **Controller API**
목록 호출 명령이 실행 되면 아래와 같이 Req가 전달된다.  
```
magnum.api.controllers.v1.pod.PodsController#get_all
magnum.api.controllers.v1.pod.PodsController#_get_pods_collection

magnum.conductor.api.API#pod_list
magnum.objects.pod.Pod#list

```
#### <i class="icon-pencil"></i> **Database**  
아래와 같은 database api로 진입하고, 그 이후 쿼리가 실행 된다. Select된 결과는 역순으로 WEB MVC를 통해서 출력된다. 
```
magnum.db.sqlalchemy.api.Connection#get_pod_list
```
```
SELECT pod.created_at AS pod_created_at, 
       pod.updated_at AS pod_updated_at, 
       pod.id         AS pod_id, 
       pod.uuid       AS pod_uuid, 
       pod.name       AS pod_name, 
       pod."desc"     AS pod_desc, 
       pod.bay_uuid   AS pod_bay_uuid, 
       pod.images     AS pod_images, 
       pod.labels     AS pod_labels, 
       pod.status     AS pod_status, 
       pod.project_id AS pod_project_id, 
       pod.user_id    AS pod_user_id 
FROM   pod 
```

-------------
copyright@Cha Dong-Hwi
