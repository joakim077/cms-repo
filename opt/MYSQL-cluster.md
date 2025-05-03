#### In our Ghost CMS deployment, we used a MySQL database deployed via a StatefulSet to maintain stable network identities and persistent storage.

When using MYSQL statfulSet for high availability, it's important to follow these principles:
- Only one primary pod should act as primary node and it handles all the write operation.
- all other secondary pods can act as secondary node with read-only permission.
- A replication mechanism must be configured to keep the data synchronized between primary and secondary pods. (this will prevent data conflict)
- If primary pod fails, it is required to promote a secondary pod to primary.


#### Intorduction to MYSQL Operator for Kubernetes
MYSQL Operator for Kubernetes manages MYSQL InnoDB Cluster setup inside a Kubernetes cluster. It manages the lifecycle with setup and maintenance including automating updrages and backups.

### 2. Runnig MYSQL Operator for Kubernetes

- Install MySQL Operator for Kubernetes

  - deploy the Custom Resource Definition (CRDs):
    ```bash
      kubectl apply -f https://raw.githubusercontent.com/mysql/mysql-operator/9.1.0-2.2.2/deploy/deploy-crds.yaml
    ```
  - deploy MySQL Operator for Kubernetes:
    ```bash
      kubectl apply -f https://raw.githubusercontent.com/mysql/mysql-operator/9.1.0-2.2.2/deploy/deploy-operator.yaml
    ```
  - Verify the operator is running by checking the deployment inside the mysql-operator namespace:
    ```bash
    kubectl get deployment -n mysql-operator mysql-operator
    ```
- Create secret
  ```bash    
    kubectl create secret generic my-secret \
            --namespace=default \
            --from-literal=rootUser=root \
            --from-literal=rootHost=% \
            --from-literal=rootPassword=superSecret
  ```
- Deploy  MySQL InnoDB Cluster
  ```bash
    cat << EOF | kubectl apply -f -
    apiVersion: mysql.oracle.com/v2
    kind: InnoDBCluster
    metadata:
    name: mycluster
    spec:
    secretName: my-secret
    instances: 3
    router:
        instances: 1
    tlsUseSelfSigned: true
    datadirVolumeClaimTemplate:
        accessModes: [ "ReadWriteOnce" ]
        resources:
        requests:
            storage: 1Gi
        storageClassName: gp2  # for dynamic volume provisioning
    EOF

      # it will create 2 service.
      # 1.clusterIP: named mycluster for primary pods
      # 2.Headless: for direct access to individual MySQL instances (mycluster-0, mycluster-1 ...)
  ```


- check 
  ```bash
    kubectl run --rm -it myshell --image=container-registry.oracle.com/mysql/community-operator -- mysqlsh

    MySQL JS>  \connect root@mycluster     
    Creating a session to 'root@mycluster'
    Please provide the password for 'root@mycluster': ******
    MySQL mycluster JS>
  ```
- Exec into mycluster-0 pod and create a DataBase.
  ```bash
    kubectl exec -it mycluster-0 -- mysqlsh
    \connect root@localhost
    # provide password
    create database DemoDB;
  ```
**Note** enter ctrl + d to exit from mysqlsh



