Node 목록 출력 
-------------

기 생성된 Node 목록을 출력한다. 

####  **Trigger Request**
아래와 같이 명령어 또는 RESTful로 Request를 전송한다.
```
magnum container-list
```
```
curl -i -X GET -H 'X-Auth-Token: 638ee37e5b34489489cdd63c61a8d450' -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'User-Agent: python-magnumclient' http://20.20.20.100:9511/v1/nodes
```
#### **Controller API**
목록 호출 명령이 실행 되면 아래와 같이 Req가 전달된다.  
```
magnum.api.controllers.v1.node.NodesController#get_all
magnum.api.controllers.v1.node.NodesController#_get_nodes_collection

magnum.objects.node.Node#list
```
#### <i class="icon-pencil"></i> **Database**  
아래와 같은 database api로 진입하고, 그 이후 쿼리가 실행 된다. Select된 결과는 역순으로 WEB MVC를 통해서 출력된다. 
```
magnum.db.sqlalchemy.api.Connection#get_node_list
```
```
SELECT node.created_at     AS node_created_at, 
       node.updated_at     AS node_updated_at, 
       node.id             AS node_id, 
       node.uuid           AS node_uuid, 
       node.type           AS node_type, 
       node.project_id     AS node_project_id, 
       node.user_id        AS node_user_id, 
       node.image_id       AS node_image_id, 
       node.ironic_node_id AS node_ironic_node_id 
FROM   node 
```

-------------
copyright@Cha Dong-Hwi
