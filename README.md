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
minikube kubectl -- apply -f /home/mercury10/operator/sveta-operator/deploy/crds/sve  ta.example.com_v1alpha1_wordpress_cr.yaml
minikube kubectl -- apply -f /home/mercury10/operator/sveta-operator/deploy/crds/sve  ta.example.com_wordpresses_crd.yaml

operator-sdk run --local 


Result:

minikube kubectl -- get pod,deployment,service,pvc,secret

NAME                                   READY   STATUS    RESTARTS   AGE



