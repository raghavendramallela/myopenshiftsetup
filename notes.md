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
    ```
