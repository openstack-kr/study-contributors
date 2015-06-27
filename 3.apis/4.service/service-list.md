Service 목록 출력 
-------------

기 생성된 service 목록을 출력한다. 

####  **Trigger Request**
아래와 같이 명령어 또는 RESTful로 Request를 전송한다.
```
magnum service-list
```
```
curl -i -X GET -H 'X-Auth-Token: 895e72c1344d4f238e18afba31278ecb' -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'User-Agent: python-magnumclient' http://20.20.20.100:9511/v1/services
```
#### **Controller API**
목록 호출 명령이 실행 되면 아래와 같이 Req가 전달된다.  
```
magnum.api.controllers.v1.service.ServicesController#get_all
magnum.api.controllers.v1.service.ServicesController#_get_services_collection

magnum.conductor.api.API#service_list
magnum.objects.service.Service#list
```
#### <i class="icon-pencil"></i> **Database**  
아래와 같은 database api로 진입하고, 그 이후 쿼리가 실행 된다. Select된 결과는 역순으로 WEB MVC를 통해서 출력된다. 
```
magnum.db.sqlalchemy.api.Connection#get_service_list
```
```
SELECT service.created_at AS service_created_at, 
       service.updated_at AS service_updated_at, 
       service.id         AS service_id, 
       service.uuid       AS service_uuid, 
       service.name       AS service_name, 
       service.bay_uuid   AS service_bay_uuid, 
       service.labels     AS service_labels, 
       service.selector   AS service_selector, 
       service.ip         AS service_ip, 
       service.ports      AS service_ports, 
       service.project_id AS service_project_id, 
       service.user_id    AS service_user_id 
FROM   service 
WHERE  service.project_id = :project_id_1 

```

-------------
copyright@Cha Dong-Hwi
