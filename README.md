# chaos-engineering-getting-started
Chaos Engineering for Kubernetes introduction

 - [**Introduction**](#Introduction)
 - [**Chaos Engineering**](#chaos-engineering)
   - [**Principles**](#principles)
   - [**Use Cases**](#use-cases)
   - [**Benefits**](#benefitss)
   - [**Challenges and Pitfalls**](#challenges-and-pitfalls)
   - [**First Steps**](#first-steps)
   - [**Tools**](#tools)
 - [**Litmus Chaos**](#litmus-chaos)
   - [**Architecture**](#litmus-architecture) 
   - [**Components**](#litmus-components)
   - [**Workflows**](#workflows)
   - [**Chaos Center**](#chaos-center)
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

Difference Between Chaos Engineering And Testing : 

When we develop an application, we pass it through various tests that include Unit Tests, Integration Tests, and System Tests. 

With Unit testing, we write a unit test case and check the expected behaviour of a component that is independent of all external components whereas Integration testing checks the interaction of individual and inter-dependant components. But even extensive testing does not provide us with a guaranteed error-free system because this testing examines only pre-defined and single scenarios. The results don't cover new information about the application, system behaviour, performance, and properties. This uncertainty increases with the use of microservice architectures, where the system grows with passing time. 

Whereas in chaos, it generates a wide range and unpredictable outcome for experimenting on a distributed architecture to build confidence in the system’s capability and withstand turbulent conditions in production. Chaos Testing is a deliberate introduction of failure and faulty scenarios into our system to understand how the system will react and what could be its side effects. This type of testing is an effective method to prevent/minimize outages before they impact the system and ultimately the business.  




## **Chaos Engineering Principles**

### **Benefits**

Increases resiliency and reliability. Chaos testing enriches the organization’s intelligence about how software performs under stress and how to make it more resilient.

Accelerates innovation. Intelligence from chaos testing funnels back to developers who can implement design changes that make software more durable and improve production quality.

Advances collaboration. Developers aren’t the only group to see advantages. The insights chaos engineers glean from their experiments elevate the expertise of the technical group, leading to response times and better collaboration.

Speeds incident response. By learning what failure scenarios are possible, these teams can speed troubleshooting, repairs, and incident response.

Improves customer satisfaction. Increased resilience and faster response times mean less down-time. Greater innovation and collaboration from development and SRE teams means better software that meets new customer demands quickly with efficiency and high performance.

Boosts business outcomes. Chaos testing can also extend an organization’s competitive advantage through faster time-to-value, saving time, money, and resources, and producing a better bottom line.

Insights received after running chaos testing can lead to a reduction in production incidents for the future. 
Through Chaos Engineering, the team can verify the system's behaviour on failure so that accordingly it takes action. 
Chaos Engineering helps in the testing response of the team to the incident. Also, helps in testing if the raised alert has been notified to the correct team. 
On a high level, Chaos Engineering provides us an advantage by overall system availability. Chaos Experiments make the system more resilient to failures. 
Production outages can lead to huge losses to companies depending on the usage of the system, therefore chaos engineering helps in the prevention of large losses in revenue. 
It helps in improving the confidence and engagement of team members for carrying out disaster recovery methods and makes applications highly reliable. 

### **Challenges and Pitfalls**
Unnecessary damage. The major concern with chaos testing is the potential for unnecessary damage. Chaos engineering can lead to a real-world loss that exceeds the allowances of justifiable testing. To limit the cost of uncovering application vulnerabilities, organizations should to avoid tests that overrun the designated blast radius. The goal is to control the blast radius so you can pinpoint the cause of failure without unnecessarily introducing new points of failure.

Lack of observability. Establishing that control can be easier said than done. Lack of end-to-end observability and monitoring into all systems a blast radius might affect is a common problem. Without comprehensive observability, it can be difficult to understand critical dependencies vs non-critical dependencies, or to have adequate context to understand the true business impact of a failure or degradation in order to prioritize fixes. Lack of visibility can also make it difficult for teams to determine the exact root cause of an issue, which can complicate remediation plans.

Unclear starting system state. Another issue is having a clear picture of the starting state of the system before the test is run. Without this clarity, teams can have difficulty understanding the true effects of the test. This can diminish the effectiveness of chaos testing, and can put downstream systems at greater risk and make it harder to control the blast radius.

### **First Steps**
As with any scientific experiment, getting started with chaos engineering requires a little preparation, organization, and the ability to monitor and measure results.

Know the starting state of your environment. To plan a well-controlled chaos test, you should understand the applications, microservices, and architectural design of your environment so you can recognize the effects of the test. Having a baseline you can compare to the end state creates a blueprint for monitoring during the testing and analyzing results after.
Ask what could go wrong and establish a hypothesis. With a clear idea of the system’s starting state, ask what could go wrong. With a clear idea of the system’s starting state, ask what could go wrong. Understand service-level indicators and service-level objectives and use them as a basis for establishing an assumption for how the system should work under pressure.
Introduce chaos one variable at a time. To keep control of the blast radius, introduce only a little chaos at a time, so you can appreciate the results. Be prepared to abort the experiment under certain conditions, so no harm comes to production software, and also have a roll-back plan if something goes wrong. During the test, try to disprove the hypothesis to discover areas to focus on to improve system resilience.
Monitor and record the results. Monitor the experiment to record any nuances in application behavior. Analyze the results to see how the application responds and whether testing achieved the teams’ expectations. Use investigation tools to understand the precise root causes of slow-downs and failures.


## **Tools**

| Tool | Open Source | Language | Platforms | Notes | 
| ----------- | ----------- | ----------- | ----------- | ----------- |
| [Chaos Mesh](https://chaos-mesh.org) | :heavy_check_mark: | Go | Kubernetes | |
| [Chaos Toolkit](https://chaostoolkit.org) | :x: | Python | | |
| [Litmus Chaos](https://litmuschaos.io) | :heavy_check_mark: | TypeScript | | |
| [Gremlin](https://www.gremlin.com) | :x: | | | |
| [Chaos Blade](https://chaosblade.io) | :heavy_check_mark: | Go | Docker, Kubernetes, bare-metal, cloud providers | 




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

### **Chaos Center**
ChaosCenter provides a single pane of glass to configure, operate and monitor your experiments:
- Workflow Creation
From Templates, Custom Workflows from Scratch (using ChaosHubs), From pre-created YAMLs
Chaos Experiments Sequence Control (Parallel as well as Sequential steps creation)
Creation of either Singular or Cron Workflows as Schedules
Attaching priority to Chaos Experiments based on your use cases

- Users & Teams
Creation of Users with Role Based Access Control
Creating a Team of multiple Users
Authenticating Users

- Monitoring & Observability
Connect a Data Source (from any Agent) and monitor workflows
Visualize workflow run statistics and aggregated schedules
Compare two or more Workflows
Upload shared/downloadable dashboards available in the community
Edit queries, Tune dashboards to create a custom one from scratch
Monitor effect of chaos in real time with interleaved events and metrics from Prometheus Datasource
- Workflow Management
Rolling out automated changes using GitOps
Allowing image addition from custom image server (both public and private)
Measure and Analyse the Resilience Score of each workflow


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
eksctl create cluster -f infra/eks-cluster.yaml
2022-05-03 13:02:13 [ℹ]  eksctl version 0.95.0
2022-05-03 13:02:13 [ℹ]  using region eu-west-1
2022-05-03 13:02:13 [ℹ]  setting availability zones to [eu-west-1c eu-west-1b eu-west-1a]
2022-05-03 13:02:13 [ℹ]  subnets for eu-west-1c - public:192.168.0.0/19 private:192.168.96.0/19
2022-05-03 13:02:13 [ℹ]  subnets for eu-west-1b - public:192.168.32.0/19 private:192.168.128.0/19
2022-05-03 13:02:13 [ℹ]  subnets for eu-west-1a - public:192.168.64.0/19 private:192.168.160.0/19
2022-05-03 13:02:13 [ℹ]  nodegroup "litmus-demo-ng" will use "" [AmazonLinux2/1.22]
2022-05-03 13:02:13 [ℹ]  using Kubernetes version 1.22
2022-05-03 13:02:13 [ℹ]  creating EKS cluster "litmus-demo" in "eu-west-1" region with managed nodes
2022-05-03 13:02:13 [ℹ]  1 nodegroup (litmus-demo-ng) was included (based on the include/exclude rules)
2022-05-03 13:02:13 [ℹ]  will create a CloudFormation stack for cluster itself and 0 nodegroup stack(s)
2022-05-03 13:02:13 [ℹ]  will create a CloudFormation stack for cluster itself and 1 managed nodegroup stack(s)
2022-05-03 13:02:13 [ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=eu-west-1 --cluster=litmus-demo'
2022-05-03 13:02:13 [ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "litmus-demo" in "eu-west-1"
2022-05-03 13:02:13 [ℹ]  CloudWatch logging will not be enabled for cluster "litmus-demo" in "eu-west-1"
2022-05-03 13:02:13 [ℹ]  you can enable it with 'eksctl utils update-cluster-logging --enable-types={SPECIFY-YOUR-LOG-TYPES-HERE (e.g. all)} --region=eu-west-1 --cluster=litmus-demo'
2022-05-03 13:02:13 [ℹ]
2 sequential tasks: { create cluster control plane "litmus-demo",
    2 sequential sub-tasks: {
        wait for control plane to become ready,
        create managed nodegroup "litmus-demo-ng",
    }
}
2022-05-03 13:02:13 [ℹ]  building cluster stack "eksctl-litmus-demo-cluster"
2022-05-03 13:02:14 [ℹ]  deploying stack "eksctl-litmus-demo-cluster"
2022-05-03 13:02:44 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-03 13:03:14 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-03 13:04:14 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-03 13:05:15 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-03 13:06:15 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-03 13:07:15 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-03 13:08:15 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-03 13:09:15 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-03 13:10:16 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-03 13:11:16 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-03 13:12:16 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-cluster"
2022-05-03 13:14:24 [ℹ]  building managed nodegroup stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-03 13:14:24 [ℹ]  deploying stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-03 13:14:24 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-03 13:14:55 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-03 13:15:42 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-03 13:16:23 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-03 13:17:52 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-03 13:17:52 [ℹ]  waiting for the control plane availability...
2022-05-03 13:17:52 [✔]  saved kubeconfig as "/Users/javier/.kube/config"
2022-05-03 13:17:52 [ℹ]  no tasks
2022-05-03 13:17:52 [✔]  all EKS cluster resources for "litmus-demo" have been created
2022-05-03 13:17:53 [ℹ]  nodegroup "litmus-demo-ng" has 4 node(s)
2022-05-03 13:17:53 [ℹ]  node "ip-192-168-20-123.eu-west-1.compute.internal" is ready
2022-05-03 13:17:53 [ℹ]  node "ip-192-168-49-106.eu-west-1.compute.internal" is ready
2022-05-03 13:17:53 [ℹ]  node "ip-192-168-54-251.eu-west-1.compute.internal" is ready
2022-05-03 13:17:53 [ℹ]  node "ip-192-168-74-122.eu-west-1.compute.internal" is ready
2022-05-03 13:17:53 [ℹ]  waiting for at least 2 node(s) to become ready in "litmus-demo-ng"
2022-05-03 13:17:53 [ℹ]  nodegroup "litmus-demo-ng" has 4 node(s)
2022-05-03 13:17:53 [ℹ]  node "ip-192-168-20-123.eu-west-1.compute.internal" is ready
2022-05-03 13:17:53 [ℹ]  node "ip-192-168-49-106.eu-west-1.compute.internal" is ready
2022-05-03 13:17:53 [ℹ]  node "ip-192-168-54-251.eu-west-1.compute.internal" is ready
2022-05-03 13:17:53 [ℹ]  node "ip-192-168-74-122.eu-west-1.compute.internal" is ready
2022-05-03 13:17:54 [ℹ]  kubectl command should work with "/Users/javier/.kube/config", try 'kubectl get nodes'
2022-05-03 13:17:54 [✔]  EKS cluster "litmus-demo" in "eu-west-1" region is ready

kubectl get nodes
NAME                                           STATUS   ROLES    AGE   VERSION
ip-192-168-20-123.eu-west-1.compute.internal   Ready    <none>   87s   v1.22.6-eks-7d68063
ip-192-168-49-106.eu-west-1.compute.internal   Ready    <none>   85s   v1.22.6-eks-7d68063
ip-192-168-54-251.eu-west-1.compute.internal   Ready    <none>   88s   v1.22.6-eks-7d68063
ip-192-168-74-122.eu-west-1.compute.internal   Ready    <none>   88s   v1.22.6-eks-7d68063

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

Install Litmus.

As a prerequisite, you will need to have Helm installed on your system. 
Next, add the LitmusChaos Helm repository and then issue the helm install command below. 

```bash
helm repo add litmuschaos https://litmuschaos.github.io/litmus-helm/
"litmuschaos" has been added to your repositories

helm install chaos litmuschaos/litmus --namespace=litmus -f override-service-type-litmus.yaml
NAME: chaos
LAST DEPLOYED: Tue May  3 13:20:23 2022
NAMESPACE: litmus
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Thank you for installing litmus 😀

Your release is named chaos and it's installed to namespace: litmus.

Visit https://docs.litmuschaos.io to find more info.

```

List the services. The frontend service external endpoint might be displayed as pending for a minute. 

```bash
kubectl get services -n litmus
NAME                               TYPE           CLUSTER-IP       EXTERNAL-IP                                                               PORT(S)             AGE
chaos-litmus-auth-server-service   ClusterIP      10.100.134.234   <none>                                                                    9003/TCP,3030/TCP   16s
chaos-litmus-frontend-service      LoadBalancer   10.100.138.168   ad6c5181576aa45d6bd2e70c837c29bf-1312593859.eu-west-1.elb.amazonaws.com   9091:30356/TCP      16s
chaos-litmus-headless-mongo        ClusterIP      10.100.34.134    <none>                                                                    27017/TCP           16s
chaos-litmus-mongo                 ClusterIP      10.100.180.113   <none>                                                                    27017/TCP           16s
chaos-litmus-server-service        ClusterIP      10.100.249.254   <none>                                                                    9002/TCP,8000/TCP   16s

```
NOTE: It may take few minutes to complete the rollout. 

Browse the external-ip:9091 in your browser.
After login to the portal using default credentials (username: admin/ password: litmus), you will be asked to change your password.

![10%](docs/images/chaos-center.png)


Once the login is completed, Chaos Operator is deployed in the Litmus namespace. 

```bash
kubectl get pods -n litmus
NAME                                        READY   STATUS    RESTARTS   AGE
chaos-litmus-auth-server-785847bc86-dz8cz   1/1     Running   0          3m49s
chaos-litmus-frontend-847cf9ddbd-hz82b      1/1     Running   0          3m49s
chaos-litmus-mongo-0                        1/1     Running   0          3m49s
chaos-litmus-server-85574f6c69-qp2jv        1/1     Running   0          3m49s

kubectl get pods -n litmus
NAME                                        READY   STATUS    RESTARTS   AGE
chaos-exporter-7d4c89d779-csg2g             1/1     Running   0          48s
chaos-litmus-auth-server-785847bc86-dz8cz   1/1     Running   0          7m26s
chaos-litmus-frontend-847cf9ddbd-hz82b      1/1     Running   0          7m26s
chaos-litmus-mongo-0                        1/1     Running   0          7m26s
chaos-litmus-server-85574f6c69-qp2jv        1/1     Running   0          7m26s
chaos-operator-ce-d698f8478-crpr9           1/1     Running   0          48s
event-tracker-8567f867-6kqqs                1/1     Running   0          47s
subscriber-67b8bfcc89-4w48j                 1/1     Running   0          47s
workflow-controller-856d568f68-cv7kn        1/1     Running   0          49s
```

![10%](docs/images/chaos-center-home.png)

![10%](docs/images/chaos-agents.png)


## Enabling Monitoring
Create a monitoring namespace:
```bash
kubectl create namespace monitoring
```

Create the operator to instantiate all CRDs
kubectl -n monitoring apply -f utils/prometheus/prometheus-operator/

Deploy monitoring components
kubectl -n monitoring apply -f utils/metrics-exporters-with-service-monitors/node-exporter/
kubectl -n monitoring apply -f utils/metrics-exporters-with-service-monitors/kube-state-metrics/


Deploy prometheus instance and all the service monitors for targets
kubectl -n monitoring apply -f utils/prometheus/prometheus-configuration/

Deploy Grafana
kubectl -n monitoring apply -f utils/grafana/

Go back to Chaos Center and browse the Analytics tab.

![10%](docs/images/analytics.png)

![10%](docs/images/analytics-data-source.png)

![10%](docs/images/analytics-prometheus-data-source.png)


![10%](docs/images/analytics-dashboard.png)

![10%](docs/images/grafana-dashboard.png)

## Experiments

### Sock Shop Workflow Template

LitmusChaos comes with three pre-defined workflows, which deploy a bunch of microservices to play with. We will be running a Sock Shop workflow, which installs a sock-shop application, generate loads, inject chaos on sock-shop, uninstall the application and revert the chaos. 

Browse the dashboard and select Schedule a Workflow.

![10%](docs/images/litmus-workflows.png)
In the Workflows dashboard, select the Self-Agent and then click on Next.
![10%](docs/images/select-agent.png)

Select Create a Workflow from Pre-defined Templates and then select sock shop and then click on Next.
![10%](docs/images/pre-defined-workflow-template.png)

The workflow can be modified by editing the experiment manifest and adding or arranging the experiments settings. 
![10%](docs/images/tune-workflow.png)


The weights of the experiments can be tuned in order to set the importance of each experiment according to your requirements to get a meaningful reliability score. 
![10%](docs/images/adjust-weights.png)

On the next tab, you can schedule the workflow if the experiment needs to run at specific intervals.
![10%](docs/images/chaos-schedule.png)

Finally, click on Finish to run the workflow. 

![10%](docs/images/workflow-running.png)

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

After few minutes, the workflow run execution is completed. The workflow details are available using the graph view or the table view as shown below.

![10%](docs/images/graph-view.png)

Click on View Logs & Results to check out the logs and chaos results for the different experiments. 
![10%](docs/images/chaos-results.png)

### Container Kill
Now, let´s pick the container kill experiment to go through the details of running an experiment.
First of all, I will deploy a sample application

```bash
kubectl apply -f src/nginx/nginx-deployment.yaml -n testing
kubectl apply -f src/nginx/nginx-hpa.yaml -n testing
```

kubectl get pods -n testing
NAME                          READY   STATUS    RESTARTS   AGE
app-sample-55b8878cfb-75l2p   0/1     Running   0          14s
app-sample-55b8878cfb-wbcmc   1/1     Running   0          36s


kubectl expose deployment app-sample --type=LoadBalancer --port=80  -n testing
service/app-sample exposed

kubectl get svc -n testing
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP                                                              PORT(S)        AGE
app-sample   LoadBalancer   10.100.221.67   a587b6b418a0a475d9860ed3f85f4b4e-800872529.eu-west-1.elb.amazonaws.com   80:31203/TCP   9s



Install the experiments on the target testing namespace:
kubectl apply -f https://hub.litmuschaos.io/api/chaos/2.8.0\?file\=charts/generic/experiments.yaml -n testing 


kubectl get chaosexperiments -n testing
NAME                      AGE
container-kill            56s
disk-fill                 57s
docker-service-kill       53s
k8-pod-delete             54s
kubelet-service-kill      55s
node-cpu-hog              56s
node-drain                54s
node-io-stress            54s
node-memory-hog           53s
node-poweroff             54s
node-restart              55s
node-taint                54s
pod-autoscaler            56s
pod-cpu-hog               57s
pod-cpu-hog-exec          54s
pod-delete                55s
pod-dns-error             56s
pod-dns-spoof             55s
pod-io-stress             53s
pod-memory-hog            56s
pod-memory-hog-exec       55s
pod-network-corruption    55s
pod-network-duplication   54s
pod-network-latency       57s
pod-network-loss          56s
pod-network-partition     56s


In order to enable the experiment execution against the deployment, I need to add the annotation litmuschaos.io/chaos="true". 


kubectl annotate deployment/app-sample litmuschaos.io/chaos="true" -n testing
kubectl describe deployment/app-sample -n testing


Name:                   app-sample
Namespace:              testing
CreationTimestamp:      Tue, 03 May 2022 15:35:52 +0200
Labels:                 app=app-sample
                        app.kubernetes.io/name=app-sample
Annotations:            deployment.kubernetes.io/revision: 1
                        kubectl.kubernetes.io/last-applied-configuration:
                          {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"labels":{"app":"app-sample","app.kubernetes.io/name":"app-sample...
                        litmuschaos.io/chaos: true
Selector:               app.kubernetes.io/name=app-sample

kubectl apply -f kill-container-sa.yaml -n testing

serviceaccount/container-kill-sa created
role.rbac.authorization.k8s.io/container-kill-sa created
rolebinding.rbac.authorization.k8s.io/container-kill-sa created

kubectl apply -f chaos-engine-kill-container.yaml -n testing
chaosengine.litmuschaos.io/app-sample-chaos created



Experiment details:
```yaml
apiVersion: litmuschaos.io/v1alpha1
kind: ChaosEngine
metadata:
  name: app-sample-chaos
  namespace: testing
spec:
  annotationCheck: "true"
  engineState: "active"
  appinfo:
    appns: "testing"
    applabel: "app.kubernetes.io/name=app-sample"
    appkind: "deployment"
  chaosServiceAccount: container-kill-sa
  monitoring: true
  jobCleanUpPolicy: "delete"
  experiments:
    - name: container-kill
      spec:
        components:
          env:
            # provide the chaos interval
            - name: CHAOS_INTERVAL
              value: "10"

            # provide the total chaos duration
            - name: TOTAL_CHAOS_DURATION
              value: "20"

            - name: CONTAINER_RUNTIME
              value: "docker"

            - name: SOCKET_PATH
              value: "/var/run/docker.sock"
        probe:
          - name: "check-frontend-access-url"
            type: "httpProbe"
            httpProbe/inputs:
              url: "http://a587b6b418a0a475d9860ed3f85f4b4e-800872529.eu-west-1.elb.amazonaws.com:80"
              insecureSkipVerify: false
              method:
                get:
                  criteria: ==
                  responseCode: "200"
            mode: "Continuous"
            runProperties:
              probeTimeout: 1
              interval: 5
              retry: 1
              probePollingInterval: 1
```

kubectl get pods -n testing
NAME                             READY   STATUS    RESTARTS   AGE
app-sample-55b8878cfb-75l2p      1/1     Running   0          21m
app-sample-55b8878cfb-wbcmc      1/1     Running   0          21m
app-sample-chaos-runner          1/1     Running   0          18s
container-kill-cdykvi--1-w6qhv   1/1     Running   0          13s



kubectl get pods -n testing
NAME                             READY   STATUS    RESTARTS     AGE
app-sample-55b8878cfb-75l2p      0/1     Running   1 (8s ago)   21m
app-sample-55b8878cfb-wbcmc      1/1     Running   0            21m
app-sample-chaos-runner          1/1     Running   0            46s
container-kill-cdykvi--1-w6qhv   1/1     Running   0            41s
container-kill-helper-cxfvxr     1/1     Running   0            19s


The result of the experiment
```bash
kubectl describe chaosresult app-sample-chaos-container-kill -n testing

Name:         app-sample-chaos-container-kill
Namespace:    testing
Labels:       app.kubernetes.io/component=experiment-job
              app.kubernetes.io/part-of=litmus
              app.kubernetes.io/runtime-api-usage=true
              app.kubernetes.io/version=2.8.0
              chaosUID=af0b4eb5-9d0d-4899-9021-161beb0a258a
              controller-uid=7ab3c405-995c-493d-b05a-c59595dc8ac3
              job-name=container-kill-rvkc87
              name=container-kill
Annotations:  <none>
API Version:  litmuschaos.io/v1alpha1
Kind:         ChaosResult
Metadata:
  Creation Timestamp:  2022-05-03T16:56:35Z
  Generation:          2
  Managed Fields:
    API Version:  litmuschaos.io/v1alpha1
    Fields Type:  FieldsV1
    fieldsV1:
      f:metadata:
        f:labels:
          .:
          f:app.kubernetes.io/component:
          f:app.kubernetes.io/part-of:
          f:app.kubernetes.io/runtime-api-usage:
          f:app.kubernetes.io/version:
          f:chaosUID:
          f:controller-uid:
          f:job-name:
          f:name:
      f:spec:
        .:
        f:engine:
        f:experiment:
      f:status:
        .:
        f:experimentStatus:
        f:history:
        f:probeStatus:
    Manager:         experiments
    Operation:       Update
    Time:            2022-05-03T16:56:35Z
  Resource Version:  13579
  UID:               c85aa975-97cf-499a-bfaf-937e4cb8ac72
Spec:
  Engine:      app-sample-chaos
  Experiment:  container-kill
Status:
  Experiment Status:
    Fail Step:                 N/A
    Phase:                     Completed
    Probe Success Percentage:  100
    Verdict:                   Pass
  History:
    Failed Runs:   0
    Passed Runs:   1
    Stopped Runs:  0
    Targets:
      Chaos Status:  targeted
      Kind:          pod
      Name:          app-sample-55b8878cfb-pss82
  Probe Status:
    Name:  check-frontend-access-url
    Status:
      Continuous:  Passed 👍
    Type:          httpProbe
Events:
  Type    Reason   Age   From                            Message
  ----    ------   ----  ----                            -------
  Normal  Awaited  84s   container-kill-rvkc87--1-m8n7b  experiment: container-kill, Result: Awaited
  Normal  Pass     3s    container-kill-rvkc87--1-m8n7b  experiment: container-kill, Result: Pass

```


## **Scheduling Experiments**

The ChaosSchedule is a user-facing chaos custom resource with namespaced scope and is used to inject chaos at a specific time or repeat it for a certain period. It schedules multiple instances of chaos by creating the chaosengine CR at the specified times.
Litmus experiments can be launched on a scheduled basis. ChaosSchedule object supports the schedule attribute 

Immediate:
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
Time range:
```yaml

apiVersion: litmuschaos.io/v1alpha1
kind: ChaosSchedule
metadata:
  name: schedule-nginx
spec:
  schedule:
    repeat:
      timeRange:
        #should be modified according to current UTC Time
        startTime: "2020-05-12T05:47:00Z"   
        endTime: "2020-09-13T02:58:00Z"   
      properties:
        #format should be like "10m" or "2h" accordingly for minutes and hours
        minChaosInterval: "2m"   
  engineTemplateSpec:
    engineState: 'active'
    appinfo:
      appns: 'default'
      applabel: 'app=nginx'
      appkind: 'deployment'
    annotationCheck: 'true'
```

Specific work hours:
```yaml

apiVersion: litmuschaos.io/v1alpha1
kind: ChaosSchedule
metadata:
  name: schedule-nginx
spec:
  schedule:
    repeat:
      properties:
        #format should be like "10m" or "2h" accordingly for minutes and hours
        minChaosInterval: "2m"   
      workHours:
        # format should be <starting-hour-number>-<ending-hour-number>(inclusive)
        includedHours: 0-12
  engineTemplateSpec:
    engineState: 'active'
    appinfo:
      appns: 'default'
      applabel: 'app=nginx'
      appkind: 'deployment'
    # It can be true/false
    annotationCheck: 'true'
```

Chaos Schedules can be halted or resumed as per need issuing the steps below:

Edit the ChaosSchedule custom resource:
```bash
kubectl edit chaosschedule schedule-nginx
```


Change the spec.scheduleState to halt
```bash
spec:
  scheduleState: halt
  ...
```

Similarly, edit the chaosschedule for resuming a halted schedule:
```bash
kubectl edit chaosschedule schedule-nginx
```
Change the spec.scheduleState to active

```bash
spec:
  scheduleState: active
  ...
```


Finally, destroy the EKS cluster. 
```bash
eksctl delete cluster --name litmus-demo
2022-05-03 17:28:57 [ℹ]  eksctl version 0.95.0
2022-05-03 17:28:57 [ℹ]  using region eu-west-1
2022-05-03 17:29:02 [ℹ]  deleting EKS cluster "litmus-demo"
2022-05-03 17:29:03 [ℹ]  will drain 0 unmanaged nodegroup(s) in cluster "litmus-demo"
2022-05-03 17:29:03 [ℹ]  deleted 0 Fargate profile(s)
2022-05-03 17:29:04 [✔]  kubeconfig has been updated
2022-05-03 17:29:04 [ℹ]  cleaning up AWS load balancers created by Kubernetes objects of Kind Service or Ingress
2022-05-03 17:32:34 [ℹ]
2 sequential tasks: { delete nodegroup "litmus-demo-ng", delete cluster control plane "litmus-demo" [async]
}
2022-05-03 17:32:34 [ℹ]  will delete stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-03 17:32:34 [ℹ]  waiting for stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng" to get deleted
2022-05-03 17:32:34 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-03 17:42:39 [ℹ]  waiting for CloudFormation stack "eksctl-litmus-demo-nodegroup-litmus-demo-ng"
2022-05-03 17:42:39 [ℹ]  will delete stack "eksctl-litmus-demo-cluster"
2022-05-03 17:42:39 [✔]  all cluster resources were deleted
```


## **References**
- [Litmus Chaos](https://litmuschaos.io/)
- [Principles of Chaos Engineering](https://principlesofchaos.org/)
- [Awesome Chaos Engineering](https://github.com/dastergon/awesome-chaos-engineering)

## **Author**
Javier Baltar - [linkedIn](https://www.linkedin.com/in/javierbaltar/) | [github](https://github.com/JavierBaltar)
