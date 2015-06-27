Container 목록 출력 
-------------

기 생성된 Container 목록을 출력한다. 

####  **Trigger Request**
아래와 같이 명령어 또는 RESTful로 Request를 전송한다.
```
magnum container-list
```
```
curl -i -X GET -H 'X-Auth-Token: e61ea3b4a1054f54bcc10e1bd11c8c99' -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'User-Agent: python-magnumclient' http://20.20.20.100:9511/v1/containers
```
#### **Controller API**
목록 호출 명령이 실행 되면 아래와 같이 Req가 전달된다.  
```
magnum.api.controllers.v1.container.ContainersController#get_all
magnum.api.controllers.v1.container.ContainersController#_get_containers_collection

magnum.objects.container.Container#list
```
#### <i class="icon-pencil"></i> **Database**  
아래와 같은 database api로 진입하고, 그 이후 쿼리가 실행 된다. Select된 결과는 역순으로 WEB MVC를 통해서 출력된다. 
```
magnum.db.sqlalchemy.api.Connection#get_container_list
```
```
SELECT container.created_at AS container_created_at, 
       container.updated_at AS container_updated_at, 
       container.id         AS container_id, 
       container.project_id AS container_project_id, 
       container.user_id    AS container_user_id, 
       container.uuid       AS container_uuid, 
       container.name       AS container_name, 
       container.image_id   AS container_image_id, 
       container.command    AS container_command, 
       container.bay_uuid   AS container_bay_uuid, 
       container.status     AS container_status 
FROM   container 
```

-------------
copyright@Cha Dong-Hwi
