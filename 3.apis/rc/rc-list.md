ReplicationControllers 목록 출력 
-------------

기 생성된 ReplicationControllers 목록을 출력한다. 

####  **Trigger Request**
아래와 같이 명령어 또는 RESTful로 Request를 전송한다.
```
magnum  rc-list
```
```
curl -i -X GET -H 'X-Auth-Token: 7b859d6a00904cd1831a6b4ecb22e7ec' -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'User-Agent: python-magnumclient' http://20.20.20.100:9511/v1/rcs
```
#### **Controller API**
목록 호출 명령이 실행 되면 아래와 같이 Req가 전달된다.  
```
magnum.api.controllers.v1.replicationcontroller.ReplicationControllersController#get_all
magnum.api.controllers.v1.replicationcontroller.ReplicationControllersController#_get_rcs_collection

magnum.conductor.api.API#rc_list
magnum.objects.replicationcontroller.ReplicationController#list
```
#### <i class="icon-pencil"></i> **Database**  
아래와 같은 database api로 진입하고, 그 이후 쿼리가 실행 된다. Select된 결과는 역순으로 WEB MVC를 통해서 출력된다. 
```
magnum.db.sqlalchemy.api.Connection#get_rc_list
```
```
SELECT replicationcontroller.created_at AS replicationcontroller_created_at, 
       replicationcontroller.updated_at AS replicationcontroller_updated_at, 
       replicationcontroller.id         AS replicationcontroller_id, 
       replicationcontroller.uuid       AS replicationcontroller_uuid, 
       replicationcontroller.name       AS replicationcontroller_name, 
       replicationcontroller.bay_uuid   AS replicationcontroller_bay_uuid, 
       replicationcontroller.images     AS replicationcontroller_images, 
       replicationcontroller.labels     AS replicationcontroller_labels, 
       replicationcontroller.replicas   AS replicationcontroller_replicas, 
       replicationcontroller.project_id AS replicationcontroller_project_id, 
       replicationcontroller.user_id    AS replicationcontroller_user_id 
FROM   replicationcontroller 
```

-------------
copyright@Cha Dong-Hwi
