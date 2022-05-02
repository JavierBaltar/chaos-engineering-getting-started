# chaos-engineering-getting-started
Chaos Engineering for Kubernetes introduction

 - [**Introduction**](#Introduction)
 - [**Chaos Engineering**](#chaos-engineering)
 - [**Chaos Engineering Principles**](#chaos-engineering-principles)
 - [**Chaos Engineering Tools**](#chaos-engineering-tools)
 - [**Litmus Chaos**](#litmus-chaos)
   - [**Architecture**](#litmus-architecture) 
   - [**Components**](#litmus-components)
   - [**Chaos Portal**](#chaos-portal)
     - [**Components**](#components)
 - [**Demo**](#demo)
   - [**Source Code**](#source-code)
   - [**Environment Setup**](#environment-setup)
   - [**Experiments**](#experiments)
     - [**Sock Shop Workflow Template**](#sock-shop-workflow-template)
     - [**Container Kill**](#container-kill)
 - [**References**](#referencess)
 - [**Author**](#author)

## **Introduction**
The main objective of this demo is discussing on how to bring understanding to a system that is fundamentally not understandable.

## **Chaos Engineering**

Started back in the days in Netflix and amazon moving to datacenter to cloud public provider aws specifically (chaos monkey to simian army)

Chaos Engineering is the discipline of experimenting on a system in order to build confidence in the system’s capability to withstand turbulent conditions in production.

Chaos engineering started in 2010 during Netflix migration to cloud infrastructure. Netflix engineers decided to develop a suite called Chaos Monkey to test various failure conditions and ensure customer experience.

Later on, Simian Army (a Chaos Monkey evolution) included tools to test AWS infrastrucure and services resiliency againsts failures such as disabling an AWS region, dropping an availability zone, simulating network delays and outages, etc. 


## **Chaos Engineering Principles**


## **Chaos Engineering Tools**

| Tool | Open Source | Notes | 
| ----------- | ----------- | ----------- | 
| [Chaos Mesh](https://chaos-mesh.org) | :heavy_check_mark: |
| [Chaos Toolkit](https://chaostoolkit.org) | :x: |
| [Litmus Chaos](https://litmuschaos.io) | :heavy_check_mark: |
| [Gremlin](https://www.gremlin.com) | :x: |
| [Chaos Blade](https://chaosblade.io) | :heavy_check_mark: |

## **Litmus Chaos**
Litmus is a Chaos Engineering Kubernetes native tool which provides for exhaustive experiments support for testing containers, pods, and nodes. It has good documentation for each of its experiment and also provides a GitHub repository of experiments, open for public contribution.

Its main advantages are:
- Components declared as Kubernetes Custom Resource Definitions (CDRs)
- Plenty of out of the box experiments
- Experiments SDK available in Go, Ansible and Python. 
- Chaos Portal GUI 
- Integration with CICD tools
- Experiment metrics can be imported to Prometheus. 

### **Architecture**
### **Components**
- ChaosExperiment defines the experiment itself, required actions, and their schedule

- ChaosEngine connects an application or Kubernetes node to the specific ChaosExperiment

- ChaosResult stores the results of the experiment. Operator exports it as Prometheus metrics.

#### Experiments Workflow

Once a chaosengine object is created, Litmus creates the Chaos runner pod in the target namespace. This runner will orchestrate the experiment in the specified namespace and against the specified targets. 

Target identification is something that makes Litmus different. To zero in on the target, the user has to insert a specific annotation on the deployment (more workloads are supported here: DaemonSet, StatefulSet and DeploymentConfig). Then, the user needs to modify the labels and fields in the chaosengine object (an example is shown below) so that Litmus can then locate all (or some) of the pods of the target deployment. 

Once the Operator verifies that all the above prerequisites are met (correct labelling, annotation, Chaosexperiment object, permissions), it will create a pod of the experiment runner, which is responsible for the execution of the experiment. This workflow allows for limiting the blast radius of an experiment, as well as for concurrent experiment executions.

### **Chaos Portal**


## **Demo**

### **Source Code**

Source code can be cloned from [github](https://github.com/JavierBaltar/chaos-engineering-getting-started).

```bash
git clone https://github.com/JavierBaltar/chaos-engineering-getting-started.git
cd chaos-engineering-getting-started
```

### **Environment Setup**
The first step is installing Litmus Chaos. In this case, Litmus is deployed on an Amazon EKS cluster using the following tools: eksctl, helm and kubectl.

Once your AWS credentials profile is configure, create a new EKS cluster issuing the following eksctl command:
```bash
eksctl create cluster -f infra/eks-cluster.yaml
```
Output:
```bash
eksctl create cluster -f eks-cluster.yaml
2022-05-02 00:33:36 [ℹ]  eksctl version 0.95.0
2022-05-02 00:33:36 [ℹ]  using region eu-west-1
2022-05-02 00:33:37 [ℹ]  setting availability zones to [eu-west-1a eu-west-1c eu-west-1b]
2022-05-02 00:33:37 [ℹ]  subnets for eu-west-1a - public:192.168.0.0/19 private:192.168.96.0/19
2022-05-02 00:33:37 [ℹ]  subnets for eu-west-1c - public:192.168.32.0/19 private:192.168.128.0/19
2022-05-02 00:33:37 [ℹ]  subnets for eu-west-1b - public:192.168.64.0/19 private:192.168.160.0/19
2022-05-02 00:33:37 [ℹ]  nodegroup "litmus-demo-ng" will use "" [AmazonLinux2/1.22]
2022-05-02 00:33:37 [ℹ]  using Kubernetes version 1.22
2022-05-02 00:33:37 [ℹ]  creating EKS cluster "litmus-demo" in "eu-west-1" region with managed nodes
2022-05-02 00:33:37 [ℹ]  1 nodegroup (litmus-demo-ng) was included (based on the include/exclude rules)
2022-05-02 00:33:37 [ℹ]  will create a CloudFormation stack for cluster itself and 0 nodegroup stack(s)
2022-05-02 00:33:37 [ℹ]  will create a CloudFormation stack for cluster itself and 1 managed nodegroup stack(s)
2022-05-02 00:33:37 [ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=eu-west-1 --cluster=litmus-demo'
2022-05-02 00:33:37 [ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "litmus-demo" in "eu-west-1"
2022-05-02 00:33:37 [ℹ]  CloudWatch logging will not be enabled for cluster "litmus-demo" in "eu-west-1"
2022-05-02 00:33:37 [ℹ]  you can enable it with 'eksctl utils update-cluster-logging --enable-types={SPECIFY-YOUR-LOG-TYPES-HERE (e.g. all)} --region=eu-west-1 --cluster=litmus-demo'
2022-05-02 00:33:37 [ℹ]
2 sequential tasks: { create cluster control plane "litmus-demo",
    2 sequential sub-tasks: {
        wait for control plane to become ready,
        create managed nodegroup "litmus-demo-ng",
    }
}
2022-05-02 00:33:37 [ℹ]  building cluster stack "eksctl-litmus-demo-cluster"
2022-05-02 00:33:37 [ℹ]  deploying stack "eksctl-litmus-demo-cluster"
2022-05-02 00:34:07 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-02 00:34:38 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-02 00:35:38 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-02 00:36:38 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-02 00:37:38 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-02 00:38:39 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-02 00:39:39 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-02 00:40:39 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-02 00:41:39 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-02 00:42:40 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-02 00:43:40 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-02 00:44:40 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-02 00:45:40 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-02 00:47:43 [ℹ]  building managed nodegroup stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-02 00:47:43 [ℹ]  deploying stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-02 00:47:43 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-02 00:48:14 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-02 00:48:55 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-02 00:49:39 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-02 00:51:31 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"

2022-05-02 00:52:39 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-02 00:52:39 [ℹ]  waiting for the control plane availability...
2022-05-02 00:52:39 [✔]  saved kubeconfig as "/Users/javier/.kube/config"
2022-05-02 00:52:39 [ℹ]  no tasks
2022-05-02 00:52:39 [✔]  all EKS cluster resources for "litmus-demo" have been created
2022-05-02 00:52:40 [ℹ]  nodegroup "litmus-demo-ng" has 4 node(s)
2022-05-02 00:52:40 [ℹ]  node "ip-192-168-19-54.eu-west-1.compute.internal" is ready
2022-05-02 00:52:40 [ℹ]  node "ip-192-168-42-161.eu-west-1.compute.internal" is ready
2022-05-02 00:52:40 [ℹ]  node "ip-192-168-55-88.eu-west-1.compute.internal" is ready
2022-05-02 00:52:40 [ℹ]  node "ip-192-168-66-101.eu-west-1.compute.internal" is ready
2022-05-02 00:52:40 [ℹ]  waiting for at least 2 node(s) to become ready in "litmus-demo-ng"
2022-05-02 00:52:40 [ℹ]  nodegroup "litmus-demo-ng" has 4 node(s)
2022-05-02 00:52:40 [ℹ]  node "ip-192-168-19-54.eu-west-1.compute.internal" is ready
2022-05-02 00:52:40 [ℹ]  node "ip-192-168-42-161.eu-west-1.compute.internal" is ready
2022-05-02 00:52:40 [ℹ]  node "ip-192-168-55-88.eu-west-1.compute.internal" is ready
2022-05-02 00:52:40 [ℹ]  node "ip-192-168-66-101.eu-west-1.compute.internal" is ready
2022-05-02 00:52:41 [ℹ]  kubectl command should work with "/Users/javier/.kube/config", try 'kubectl get nodes'
2022-05-02 00:52:41 [✔]  EKS cluster "litmus-demo" in "eu-west-1" region is ready

kubectl get nodes
NAME                                           STATUS   ROLES    AGE     VERSION
ip-192-168-19-54.eu-west-1.compute.internal    Ready    <none>   3m51s   v1.22.6-eks-7d68063
ip-192-168-42-161.eu-west-1.compute.internal   Ready    <none>   3m46s   v1.22.6-eks-7d68063
ip-192-168-55-88.eu-west-1.compute.internal    Ready    <none>   3m50s   v1.22.6-eks-7d68063
ip-192-168-66-101.eu-west-1.compute.internal   Ready    <none>   3m52s   v1.22.6-eks-7d68063

```

Before installing Litmus using Helm chart, override the default NodePort service to LoadBalancer. 
```bash
cat <<EOF > override-service-type-litmus.yaml
portal:
  server:
    service:
      type: ClusterIP
  frontend:
    service:
      type: LoadBalancer
EOF

```

Create a Litmus namespace:
```bash
kubectl create namespace litmus
```

Install Litmus:

```bash
helm repo add litmuschaos https://litmuschaos.github.io/litmus-helm/
"litmuschaos" has been added to your repositories

helm install chaos litmuschaos/litmus --namespace=litmus -f override-service-type-litmus.yaml
NAME: chaos
LAST DEPLOYED: Sun May  1 15:16:30 2022
NAMESPACE: litmus
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Thank you for installing litmus 😀

Your release is named chaos and it's installed to namespace: litmus.

```

List the services. The frontend service external endpoint might be displayed as pending for a minute. 

```bash
kubectl get svc -n litmus
NAME                               TYPE           CLUSTER-IP       EXTERNAL-IP                                                               PORT(S)             AGE
chaos-litmus-auth-server-service   ClusterIP      10.100.134.234   <none>                                                                    9003/TCP,3030/TCP   16s
chaos-litmus-frontend-service      LoadBalancer   10.100.138.168   ad6c5181576aa45d6bd2e70c837c29bf-1312593859.eu-west-1.elb.amazonaws.com   9091:30356/TCP      16s
chaos-litmus-headless-mongo        ClusterIP      10.100.34.134    <none>                                                                    27017/TCP           16s
chaos-litmus-mongo                 ClusterIP      10.100.180.113   <none>                                                                    27017/TCP           16s
chaos-litmus-server-service        ClusterIP      10.100.249.254   <none>                                                                    9002/TCP,8000/TCP   16s

```

Browse the external-ip:9091 in your browser.
After login to the portal using default credentials (admin/litmus), you will be asked to change your password.

![10%](docs/images/chaos-center.png)

![10%](docs/images/chaos-center-home.png)

![10%](docs/images/chaos-agents.png)


Enabling Monitoring


## Experiments**

### Sock Shop Workflow Template

```bash
kubectl get pods -n sock-shop
NAME                               READY   STATUS    RESTARTS   AGE
carts-7856799f8-55qtb              0/1     Running   0          3m17s
carts-7856799f8-hlsgw              1/1     Running   0          3m17s
carts-db-0                         1/1     Running   0          3m17s
carts-db-1                         1/1     Running   0          2m27s
catalogue-7c6967c66f-bczg9         1/1     Running   0          3m17s
catalogue-7c6967c66f-d9cqv         1/1     Running   0          3m17s
catalogue-db-0                     1/1     Running   0          3m17s
catalogue-db-1                     1/1     Running   0          2m28s
front-end-5d87c757c4-crbbz         1/1     Running   0          3m17s
front-end-5d87c757c4-fvl8w         1/1     Running   0          3m17s
git-app-checker-5bd479bbcb-48kfd   1/1     Running   0          3m16s
orders-7454778b8c-9jr6r            1/1     Running   0          3m17s
orders-7454778b8c-xwsjv            0/1     Running   0          3m17s
orders-db-0                        1/1     Running   0          3m17s
orders-db-1                        1/1     Running   0          2m31s
payment-6ddc6b495f-9zr87           1/1     Running   0          3m17s
payment-6ddc6b495f-f684t           1/1     Running   0          3m17s
qps-test-5554f4cb8d-lsgpq          1/1     Running   0          3m16s
queue-master-5b6b9988fb-kn44s      0/1     Running   0          3m17s
queue-master-5b6b9988fb-szcw4      0/1     Running   0          3m17s
rabbitmq-667d94879f-5t7xt          1/1     Running   0          3m17s
rabbitmq-667d94879f-hjzxj          1/1     Running   0          3m16s
shipping-6b7bcccb87-qvsbd          1/1     Running   0          3m16s
shipping-6b7bcccb87-sg5w9          0/1     Running   0          3m16s
user-5c855f4c56-fhn5b              0/1     Running   0          3m16s
user-5c855f4c56-xt7dm              0/1     Running   0          3m16s
user-db-0                          1/1     Running   0          3m17s
user-db-1                          1/1     Running   0          2m7s
```



### Container Kill


## **Scheduling Experiments**
Litmus experiments can be launched on a scheduled basis. ChaosSchedule object supports the schedule attribute 


```yaml
apiVersion: litmuschaos.io/v1alpha1
kind: ChaosSchedule
metadata:
  name: schedule-nginx
spec:
  schedule:
    now: true
  engineTemplateSpec:
    engineState: 'active'
    appinfo:
      appns: 'default'
      applabel: 'app=nginx'
      appkind: 'deployment'
    annotationCheck: 'true'

```


Specific timestamp:

```yaml

apiVersion: litmuschaos.io/v1alpha1
kind: ChaosSchedule
metadata:
  name: schedule-nginx
spec:
  schedule:
    once:
      #should be modified according to current UTC Time
      executionTime: "2020-05-12T05:47:00Z" 
  engineTemplateSpec:
      engineState: 'active'
      appinfo:
        appns: 'default'
        applabel: 'app=nginx'
        appkind: 'deployment'
      annotationCheck: 'true'
```


## **References**
- [Litmus Chaos](https://litmuschaos.io/)
- [Principles of Chaos Engineering](https://principlesofchaos.org/)

## **Author**
Javier Baltar - [linkedIn](https://www.linkedin.com/in/javierbaltar/) | [github](https://github.com/JavierBaltar)
