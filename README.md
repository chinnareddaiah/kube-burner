# kube-burner
# Stress Testing Using Kube-Burner
> Note:-The purpose of this stress testing is to check whether worker nodes in a particular k8s cluster are upscaling/downscaling or not.

Kube burner is a tool for stress testing where we will be deploying k8s object like deployment, secret, configmap, svc etc iteration wise. For example, suppose we want to deploy 300 pods in a k8s cluster, instead of deploying 300 pods at once, we will break it down into 5 iteration where in each iteration, we will deploy 60 pods (6*50=300). So kube burner gives us the flexibility to gradually increase the load on a particular k8s cluster and we can check how the cluster is performing in real time. Kube burner will also clear all objects that had been created for stress testing purpose. K8s360 grafana dashboard will be considered for visualization, to check the behaviour of the cluster. We can also check the status of worker nodes and kube-burner logs from terminal(bastion).

## Step 1:

Clone the kube-burner repository from innersource

## Step 2:

Run the shell script i.e sh setup.sh inside kube-burner folder. This script will create kube-burner specific namespace, serviceaccount, clusterRole and clusterRoleBinding.

![fluxcd demo use case](./images/1.png?raw=true "FluxCD-Demousecase")

## Step 3:

Navigate to test/api-intensive-test folder and apply the configmap and pod yaml files to start stress-testing. By default it will create 300 pods (50 pods in 6 iterations) and six service, configmap and secret objects(based on number of iteration).

#### command:-

cd test/api-intensive-test/

kubectl apply -f . -n  stress-testing

![fluxcd demo use case](./images/2.png?raw=true "FluxCD-Demousecase")

## Step 4:

To check logs for kube-burner pod:-

#### command:-

kubectl logs -f kube-burner -n stress-testing

![fluxcd demo use case](./images/3.png?raw=true "FluxCD-Demousecase")

To check number of nodes from terminal/bastion,

#### command:-

watch kubectl get nodes

![fluxcd demo use case](./images/4.png?raw=true "FluxCD-Demousecase")

## Step 5:

###  **Note: optional step
Pull up K8s360 dashboard(if available) to check how the cluster is responding

![fluxcd demo use case](./images/5.png?raw=true "FluxCD-Demousecase")
![fluxcd demo use case](./images/6.png?raw=true "FluxCD-Demousecase")

## Step 6:

To restart the process, make sure you delete the kube-burner pod from stress-testing namespace

#### command:-

kubectl delete -f . -n stress-testing

kubectl apply -f . -n stress-testing

![fluxcd demo use case](./images/8.png?raw=true "FluxCD-Demousecase")

## Step 7:

Once testing is done,for cleaning up the other dependencies like namespace, serviceaccount, clusterRole and clusterRoleBinding for kube-burner, execute clear.sh script.

### **Note: Once “clear.sh” got executed, to perform the stress testing again, you have to start from “Step 2” again

#### commands:-

cd ~/kube-burner

sh clear.sh

# Customizing Kube-burner configuration

### **Note: optional

By default, kube burner will create 300 pods (50 pods in 6 iterations) and six service, configmap and secret objects(based on number of iteration).If you want to customize this settings, navigate to test/api-intensive-test/ inside kube-burner folder and edit the configmap.

Few examples are like:-

a. To increase the number of iteration, change the value in line 15

![fluxcd demo use case](./images/9.png?raw=true "FluxCD-Demousecase")

b. To increase the number of pods to be deployed in each iterations, change the value in line 82

![fluxcd demo use case](./images/10.png?raw=true "FluxCD-Demousecase")

c. To increase the jobPause time(it refers to how long kube burner will wait before it start deleting the deployed k8s objects like deployments, secret, configmap and service for stress-testing), change the value in line 24.

![fluxcd demo use case](./images/11.png?raw=true "FluxCD-Demousecase")

d. If you want to deploy k8s object in master node, please uncomment the coommented 4 lines from tolerations in configmap

![fluxcd demo use case](./images/12.png?raw=true "FluxCD-Demousecase")
