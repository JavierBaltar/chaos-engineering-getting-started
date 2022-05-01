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
 - 
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
➜  litmuseks eksctl create cluster -f eks-cluster.yaml
2022-05-01 21:42:44 [ℹ]  eksctl version 0.95.0
2022-05-01 21:42:44 [ℹ]  using region eu-west-1
2022-05-01 21:42:45 [ℹ]  setting availability zones to [eu-west-1b eu-west-1c eu-west-1a]
2022-05-01 21:42:45 [ℹ]  subnets for eu-west-1b - public:192.168.0.0/19 private:192.168.96.0/19
2022-05-01 21:42:45 [ℹ]  subnets for eu-west-1c - public:192.168.32.0/19 private:192.168.128.0/19
2022-05-01 21:42:45 [ℹ]  subnets for eu-west-1a - public:192.168.64.0/19 private:192.168.160.0/19
2022-05-01 21:42:45 [ℹ]  nodegroup "litmus-demo-ng" will use "" [AmazonLinux2/1.21]
2022-05-01 21:42:45 [ℹ]  using Kubernetes version 1.21
2022-05-01 21:42:45 [ℹ]  creating EKS cluster "litmus-demo" in "eu-west-1" region with managed nodes
2022-05-01 21:42:45 [ℹ]  1 nodegroup (litmus-demo-ng) was included (based on the include/exclude rules)
2022-05-01 21:42:45 [ℹ]  will create a CloudFormation stack for cluster itself and 0 nodegroup stack(s)
2022-05-01 21:42:45 [ℹ]  will create a CloudFormation stack for cluster itself and 1 managed nodegroup stack(s)
2022-05-01 21:42:45 [ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=eu-west-1 --cluster=litmus-demo'
2022-05-01 21:42:45 [ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "litmus-demo" in "eu-west-1"
2022-05-01 21:42:45 [ℹ]  CloudWatch logging will not be enabled for cluster "litmus-demo" in "eu-west-1"
2022-05-01 21:42:45 [ℹ]  you can enable it with 'eksctl utils update-cluster-logging --enable-types={SPECIFY-YOUR-LOG-TYPES-HERE (e.g. all)} --region=eu-west-1 --cluster=litmus-demo'
2022-05-01 21:42:45 [ℹ]
2 sequential tasks: { create cluster control plane "litmus-demo",
    2 sequential sub-tasks: {
        wait for control plane to become ready,
        create managed nodegroup "litmus-demo-ng",
    }
}
2022-05-01 21:42:45 [ℹ]  building cluster stack "eksctl-litmus-demo-cluster"
2022-05-01 21:42:45 [ℹ]  deploying stack "eksctl-litmus-demo-cluster"
2022-05-01 21:43:15 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-01 21:43:45 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-01 21:44:46 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-01 21:45:46 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-01 21:46:46 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-01 21:47:46 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-01 21:48:47 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-01 21:49:47 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-01 21:50:47 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-01 21:51:47 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-01 21:52:48 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-01 21:53:48 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-01 21:55:50 [ℹ]  building managed nodegroup stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-01 21:55:51 [ℹ]  deploying stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-01 21:55:51 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-01 21:56:21 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-01 21:57:04 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-01 21:57:47 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-01 21:59:09 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-01 21:59:10 [ℹ]  waiting for the control plane availability...
2022-05-01 21:59:10 [✔]  saved kubeconfig as "/Users/javier/.kube/config"
2022-05-01 21:59:10 [ℹ]  no tasks
2022-05-01 21:59:10 [✔]  all EKS cluster resources for "litmus-demo" have been created
2022-05-01 21:59:10 [ℹ]  nodegroup "litmus-demo-ng" has 4 node(s)
2022-05-01 21:59:10 [ℹ]  node "ip-192-168-54-154.eu-west-1.compute.internal" is ready
2022-05-01 21:59:10 [ℹ]  node "ip-192-168-76-241.eu-west-1.compute.internal" is ready
2022-05-01 21:59:10 [ℹ]  node "ip-192-168-87-91.eu-west-1.compute.internal" is ready
2022-05-01 21:59:10 [ℹ]  node "ip-192-168-9-177.eu-west-1.compute.internal" is ready
2022-05-01 21:59:10 [ℹ]  waiting for at least 2 node(s) to become ready in "litmus-demo-ng"
2022-05-01 21:59:11 [ℹ]  nodegroup "litmus-demo-ng" has 4 node(s)
2022-05-01 21:59:11 [ℹ]  node "ip-192-168-54-154.eu-west-1.compute.internal" is ready
2022-05-01 21:59:11 [ℹ]  node "ip-192-168-76-241.eu-west-1.compute.internal" is ready
2022-05-01 21:59:11 [ℹ]  node "ip-192-168-87-91.eu-west-1.compute.internal" is ready
2022-05-01 21:59:11 [ℹ]  node "ip-192-168-9-177.eu-west-1.compute.internal" is ready
2022-05-01 21:59:12 [ℹ]  kubectl command should work with "/Users/javier/.kube/config", try 'kubectl get nodes'
2022-05-01 21:59:12 [✔]  EKS cluster "litmus-demo" in "eu-west-1" region is ready
➜  litmuseks kubectl get nodes
NAME                                           STATUS   ROLES    AGE   VERSION
ip-192-168-54-154.eu-west-1.compute.internal   Ready    <none>   72s   v1.21.5-eks-9017834
ip-192-168-76-241.eu-west-1.compute.internal   Ready    <none>   68s   v1.21.5-eks-9017834
ip-192-168-87-91.eu-west-1.compute.internal    Ready    <none>   71s   v1.21.5-eks-9017834
ip-192-168-9-177.eu-west-1.compute.internal    Ready    <none>   72s   v1.21.5-eks-9017834


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

List the services. The external endpoint might be displayed as pending for a minute. 

Browse the url in your browser.
After login to the portal using default credentials (admin/litmus), you will be asked to change your password.

![10%](docs/images/chaos-center.png)

![10%](docs/images/chaos-center-home.png)

![10%](docs/images/chaos-agents.png)


Enabling Monitoring


## **Experiments**

### **Scheduling Experiments**
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
