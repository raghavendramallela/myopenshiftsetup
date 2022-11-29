# OpenShift Notes

## Important Resources to run an application in OpenShift `oc api-resources`

- **Pod**: A running instance of application. 

    `oc explain po`: Pod is a collection of containers that can run on a host. (Also check `oc explain pod.spec | less`)
- **ReplicaSet** (or **ReplicationController** in OpenShift): Takes care of running multiple instances of pods.

    `oc explain rs`: ReplicaSet ensures that a specified number of pod replicas are running at any given time.
- **Deployment** (or **DeploymentConfig** in OpenShift): Adds cluster properties; like Update Strategy & Replication, to the running deployment.

    `oc explain deploy`: Deployment enables declarative updates for Pods and ReplicaSets.
- **Service**: An APi-based Load Balancer for ingress traffic b/n different pods.

    `oc explain svc`: Service is a named abstraction of software service (for example, mysql) consisting of local port (for example 3306) that the proxy listens on, and the selector that determines which pods will answer requests sent through the proxy.
- **Route**: Exposes a URL that provides access to services.
- **ConfigMap**: Provides Site specific data; like env vars,configuration params, to the pods. - Helps in *Decoupling*.

    `oc explain cm`: ConfigMap holds configuration data for pods to consume.
- **Secret**: Provides sensitive information; like passwords, licenses, to pods. - uses base64 encryption.

    `oc explain secret`: Secret holds secret data of a certain type.
- **PersistentVolume**: Provides storage for Non-Ephimeral data that needs persistense when pod failures/application updates.

    `oc explain pv`: PersistentVolume (PV) is a storage resource provisioned by an administrator.
- **PersistentVolumeClaim**: A storage quota that binds from *PersistentVolume* given to pods.

    `oc explain pvc`: PersistentVolumeClaim is a user's request for and claim to a persistent volume
- **StorageClass**: Storage types that generates the amount of data that is required by *PersistentVolume*.

    `oc explain sc`: StorageClass describes the parameters for a class of storage for which PersistentVolumes can be dynamically provisioned.


## Run a sample application into OpenShift local

- Login to `developer` account:
    ```
    $ crc console --credentials
    To login as a regular user, run 'oc login -u developer -p developer https://api.crc.testing:6443'.
    To login as an admin, run 'oc login -u kubeadmin -p GHE8B-uSRJn-5u8Sg-T7J7a https://api.crc.testing:6443'
    
    $ oc login -u developer -p developer https://api.crc.testing:6443
    Login successful.

    You don't have any projects. You can try to create a new project, by running

        oc new-project <projectname>
    ```
- Create a sample project `sampleproj`
    ```
    $ oc new-project sampleproj

    Now using project "sampleproj" on server "https://api.crc.testing:6443".

    You can add applications to this project with the 'new-app' command. For example, try:

        oc new-app rails-postgresql-example

    to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

        kubectl create deployment hello-node --image=k8s.gcr.io/e2e-test-images/agnhost:2.33 -- /agnhost serve-hostname
1. Deploy a sample nginx hello app:
    ```
    $ oc create -f https://raw.githubusercontent.com/raghavendramallela/myopenshiftsetup/main/openshift/samplenginx.yaml

    Warning: would violate PodSecurity "restricted:v1.24": allowPrivilegeEscalation != false (container "samplenginx" must set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities (container "samplenginx" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "samplenginx" must set securityContext.runAsNonRoot=true), seccompProfile (pod or container "samplenginx" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")

    deployment.apps/samplenginx created
    service/samplenginx created
    route.route.openshift.io/samplenginx created
    ```
- Check `samplenginx` resources:
    ```
    $ oc get all -o wide

    NAME                               READY   STATUS    RESTARTS   AGE    IP             NODE                 NOMINATED NODE   READINESS GATES
    pod/samplenginx-74bd9f5dd6-jjj7w   1/1     Running   0          13s    10.217.0.208   crc-lgph7-master-0   <none>           <none>
    pod/samplenginx-74bd9f5dd6-r725r   1/1     Running   0          2m6s   10.217.0.207   crc-lgph7-master-0   <none>           <none>

    NAME                  TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)           AGE    SELECTOR
    service/samplenginx   ClusterIP   10.217.5.228   <none>        80/TCP,8080/TCP   2m6s   deployment=samplenginx

    NAME                          READY   UP-TO-DATE   AVAILABLE   AGE    CONTAINERS    IMAGES                             SELECTOR
    deployment.apps/samplenginx   2/2     2            2           2m6s   samplenginx   docker.io/nginxdemos/nginx-hello   deployment=samplenginx

    NAME                                     DESIRED   CURRENT   READY   AGE    CONTAINERS    IMAGES                             SELECTOR
    replicaset.apps/samplenginx-74bd9f5dd6   2         2         2       2m6s   samplenginx   docker.io/nginxdemos/nginx-hello   deployment=samplenginx,pod-template-hash=74bd9f5dd6

    NAME                                   HOST/PORT                                 PATH   SERVICES      PORT       TERMINATION   WILDCARD
    route.route.openshift.io/samplenginx   samplenginx-sampleproj.apps-crc.testing          samplenginx   8080-tcp                 None
    ```
- `lynx` the `samplenginx` in terminal: (or open lynx http://samplenginx-sampleproj.apps-crc.testing/ in a web-browser in the host-machine)
    ```
    $ lynx http://samplenginx-sampleproj.apps-crc.testing/
                                                                                                                                                                                                                Hello World
    NGINX Logo

    Server address: 10.217.0.208:8080

    Server name: samplenginx-74bd9f5dd6-jjj7w

    Date: 29/Nov/2022:05:28:50 +0000

    URI: /

    [ ] Auto Refresh

                                                                                    Request ID: c5ed937e3bb9de3ad49919c85877b5d0
                                                                                                © F5 Networks, Inc. 2020

- Cleaning up `samplenginx` resources:
    ```
    $ oc delete -f https://raw.githubusercontent.com/raghavendramallela/myopenshiftsetup/main/openshift/samplenginx.yaml

    deployment.apps "samplenginx" deleted
    service "samplenginx" deleted
    route.route.openshift.io "samplenginx" deleted
    ```
2. Deploy a sample my sql app:
    ```
    $ oc create -f https://raw.githubusercontent.com/raghavendramallela/myopenshiftsetup/main/openshift/sampleappmysql.yaml

    secret/samplemysqlsecret created

    Warning: would violate PodSecurity "restricted:v1.24": allowPrivilegeEscalation != false (container "samplemysqlapp" must set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities (container "samplemysqlapp" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "samplemysqlapp" must set securityContext.runAsNonRoot=true), seccompProfile (pod or container "samplemysqlapp" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")
    
    deployment.apps/samplemysqlapp created
    service/samplemysqlapp created
    ```
- Check `samplemysqlapp` resources:
    ```
    $ oc get all
    NAME                                READY   STATUS    RESTARTS   AGE
    pod/samplemysqlapp-66b466db-9kpm2   1/1     Running   0          13s

    NAME                     TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
    service/samplemysqlapp   ClusterIP   10.217.4.224   <none>        3306/TCP   13s

    NAME                             READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/samplemysqlapp   1/1     1            1           13s

    NAME                                      DESIRED   CURRENT   READY   AGE
    replicaset.apps/samplemysqlapp-66b466db   1         1         1       13s
    ```
- Log in to the dabase:
    ```
    $ oc exec -it pod/samplemysqlapp-66b466db-9kpm2 -- bash
    
    bash-4.2$ mysql --user=raghu --password=rgpass
    mysql: [Warning] Using a password on the command line interface can be insecure.
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 2
    Server version: 5.7.24 MySQL Community Server (GPL)

    Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.

    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

    mysql> 
    ```
- Cleaning up `samplemysqlapp` resources:
    ```
    $ oc delete -f https://raw.githubusercontent.com/raghavendramallela/myopenshiftsetup/main/openshift/sampleappmysql.yaml

    secret "samplemysqlsecret" deleted
    deployment.apps "samplemysqlapp" deleted
    service "samplemysqlapp" deleted
    ```
