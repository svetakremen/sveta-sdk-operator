# sveta-operator
 
Create a Kuberentes Operator using the operator-sdk project that deploys wordpress using on sql via a custom resource.

References : 
    https://developers.redhat.com/blog/2020/12/16/create-a-kubernetes-operator-in-golang-to-automatically-manage-a-simple-stateful-application/               
    
  https://opensource.com/article/20/3/kubernetes-operator-sdk
    


Steps:
1. Install the latest version of Operator SDK

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo yum -y install podman

sudo install minikube-linux-amd64 /usr/local/bin/minikube

2. start minikube:

minikube start

kubectl get po -A

minikube kubectl -- get po -A

wget https://github.com/operator-framework/operator-sdk/releases/download/v0.15.2/operator-sdk-v0.15.2-x86_64-linux-gnu

sudo mv operator-sdk-v0.15.2-x86_64-linux-gnu /usr/local/bin/operator-sdk
 
export PATH=$PATH:/home/<user>/Go/go/bin
 
sudo chmod +x /usr/local/bin/operator-sdk
 
export GOROOT=$(go env GOROOT)

3. Create operator:

operator-sdk add api --kind .....

4. Create custom resource definitions: Add Spec to sveta-operator/pkg/apis/sveta/v1alpha1/wordpress_types.go (passsword)

operator-sdk generate crds

operator-sdk generate k8s

5. Create controller:

operator-sdk add controller --kind 

6. Add code to controller to create subsequent watches for child resources, such as pod, deployment, service, PVC for mysql and wordpress
7. 

minikube kubectl -- apply -f /home/mercury10/operator/sveta-operator/deploy/crds/sveta.example.com_v1alpha1_wordpress_cr.yaml

minikube kubectl -- apply -f /home/mercury10/operator/sveta-operator/deploy/crds/sveta.example.com_wordpresses_crd.yaml

operator-sdk run --local 


Result:

minikube kubectl -- get pod,deployment,service,pvc,secret

NAME                                       READY   STATUS    RESTARTS   AGE


pod/wordpress-7c99b8b494-9sx2f             1/1     Running   0          22h

pod/wordpress-mysql-56d9f4dbd7-lwp9z       1/1     Running   0          22h

NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE

deployment.apps/wordpress         1/1     1            1           22h

deployment.apps/wordpress-mysql   1/1     1            1           22h

NAME                                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE

service/kubernetes                        ClusterIP   10.96.0.1        <none>        443/TCP        4d23h
 
service/wordpress                         NodePort    10.103.242.148   <none>        80:31973/TCP   22h
 
service/wordpress-mysql                   ClusterIP   None             <none>        3306/TCP       22h
 

NAME                                      STATUS      VOLUME                                  CAPACITY    ACCESS MODES     STORAGECLASS     AGE

persistentvolumeclaim/mysql-pv-claim      Bound    pvc-fed14189-7455-4731-9ba5-6e2bbc937fb5   10Gi       RWO            standard       22h

persistentvolumeclaim/wp-pv-claim         Bound    pvc-c943e984-eba3-4bbb-ab9b-b0b74bb4b913   10Gi       RWO            standard       22h

NAME                                                   TYPE                                  DATA   AGE

secret/default-token-tgp5r                             kubernetes.io/service-account-token   3      4d23h

secret/sveta-operator-token-dfsd4                      kubernetes.io/service-account-token   3      24h



