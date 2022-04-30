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


## **References**
- [Litmus Chaos](https://litmuschaos.io/)
- [Principles of Chaos Engineering](https://principlesofchaos.org/)

## **Author**
Javier Baltar - [linkedIn](https://www.linkedin.com/in/javierbaltar/) | [github](https://github.com/JavierBaltar)
