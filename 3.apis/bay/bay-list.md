Bay 목록 출력 
-------------

기 생성된 Bay 목록을 출력한다. 

####  **Trigger Request**
아래와 같이 명령어 또는 RESTful로 Request를 전송한다.
```
magnum bay-list
```
```
curl -i -X GET -H 'X-Auth-Token: 6f999389f1ca4c1e9d640e4ad93c465e' -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'User-Agent: python-magnumclient' http://20.20.20.100:9511/v1/bays
```
#### **Controller API**
목록 호출 명령이 실행 되면 아래와 같이 Req가 전달된다. WEB MVC를 타고 진입 한 후 conductor api 클래스로 진입한다. 
>- magnum.api.controllers.v1.bay.BaysController#get_all 
 - magnum.api.controllers.v1.bay.BaysController#_get_bays_collection 
> - magnum.conductor.api.API#bay_list 
 - magnum.objects.bay.Bay#list

#### <i class="icon-pencil"></i> **Database**  
아래와 같은 database api로 진입하고, 그 이후 쿼리가 실행 된다. Select된 결과는 역순으로 WEB MVC를 통해서 출력된다. 
>magnum.db.api.Connection#get_bay_list
magnum.db.sqlalchemy.api.Connection#get_bay_list

```
SELECT bay.created_at     AS bay_created_at, 
       bay.updated_at     AS bay_updated_at, 
       bay.id             AS bay_id, 
       bay.project_id     AS bay_project_id, 
       bay.user_id        AS bay_user_id, 
       bay.uuid           AS bay_uuid, 
       bay.name           AS bay_name, 
       bay.baymodel_id    AS bay_baymodel_id, 
       bay.stack_id       AS bay_stack_id, 
       bay.api_address    AS bay_api_address, 
       bay.node_addresses AS bay_node_addresses, 
       bay.node_count     AS bay_node_count, 
       bay.status         AS bay_status, 
       bay.status_reason  AS bay_status_reason, 
       bay.discovery_url  AS bay_discovery_url 
FROM   bay 
WHERE  bay.project_id = :project_id_1 
```

-------------
copyright@Cha Dong-Hwi
