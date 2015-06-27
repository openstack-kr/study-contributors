Baymodel 목록 출력 
-------------

기 생성된 Baymodel 목록을 출력한다. 

####  **Trigger Request**
아래와 같이 명령어 또는 RESTful로 Request를 전송한다.
```
baymodel-list
```
```
curl -i -X GET -H 'X-Auth-Token: 7edc91a3e040473592eaacc957899bac' -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'User-Agent: python-magnumclient' http://20.20.20.100:9511/v1/baymodels
```
#### **Controller API**
목록 호출 명령이 실행 되면 아래와 같이 Req가 전달된다. WEB MVC를 타고 진입 한다. 
```
magnum.api.controllers.v1.baymodel.BayModelsController#get_all
magnum.api.controllers.v1.baymodel.BayModelsController#_get_baymodels_collection
```
#### <i class="icon-pencil"></i> **Database**  
아래와 같은 database api로 진입하고, 그 이후 쿼리가 실행 된다. Select된 결과는 역순으로 WEB MVC를 통해서 출력된다. 
```
magnum.db.sqlalchemy.api.Connection#get_baymodel_list
```
```
SELECT baymodel.created_at          AS baymodel_created_at, 
       baymodel.updated_at          AS baymodel_updated_at, 
       baymodel.id                  AS baymodel_id, 
       baymodel.uuid                AS baymodel_uuid, 
       baymodel.project_id          AS baymodel_project_id, 
       baymodel.user_id             AS baymodel_user_id, 
       baymodel.name                AS baymodel_name, 
       baymodel.image_id            AS baymodel_image_id, 
       baymodel.flavor_id           AS baymodel_flavor_id, 
       baymodel.master_flavor_id    AS baymodel_master_flavor_id, 
       baymodel.keypair_id          AS baymodel_keypair_id, 
       baymodel.external_network_id AS baymodel_external_network_id, 
       baymodel.fixed_network       AS baymodel_fixed_network, 
       baymodel.dns_nameserver      AS baymodel_dns_nameserver, 
       baymodel.apiserver_port      AS baymodel_apiserver_port, 
       baymodel.docker_volume_size  AS baymodel_docker_volume_size, 
       baymodel.ssh_authorized_key  AS baymodel_ssh_authorized_key, 
       baymodel.cluster_distro      AS baymodel_cluster_distro, 
       baymodel.coe                 AS baymodel_coe 
FROM   baymodel 
```

-------------
copyright@Cha Dong-Hwi
