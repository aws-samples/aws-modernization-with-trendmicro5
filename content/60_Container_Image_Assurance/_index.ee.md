---
title: "Pipeline Phase - Deploy"
chapter: true
weight: 50
pre: "<b>5. </b>"
---

# Deploy Stage

Now we will use a wide variety of tools and methodologies to actually test and secure our application environment:

- [Zaproxy](https://github.com/zaproxy/zaproxy)
- [Cloud One Network Security](https://cloudone.trendmicro.com/)
- [Cloud One Container Security](https://cloudone.trendmicro.com/)


#### The application that we will use in our company will be this one:
- [Pygoat-TM](https://github.com/JustinDPerkins/pygoat-tm)

---

### Deploy the infrastructure that will host our application on AWS EKS (Elastic Kubernetes Service)

**For this, we are going to use eksctl. EKSCTL is a simple CLI tool for creating clusters on AWS Elastic Kubernetes Service (EKS): <a href="https://github.com/weaveworks/eksctl"> eksctl Github repo</a>**

{{% notice info %}}
<p style='text-align: left;'>
eksctl will create two Stacks for us, the first is the EKS Control Plane and the second Stack is for the NodeGroup
</p>
{{% /notice %}}


---

### 1. In the terminal create the configuration file so that we can create our cluster control plane and our node group in EKS using eksctl

- In the terminal, type the command ```touch eks-creation.yaml```
- In the left column where it shows the files, double click on the file **eks-creation.yaml**
- Or type the command ```code eks-creation.yaml``` to open the file

**And paste the following:**

    apiVersion: eksctl.io/v1alpha5
    availabilityZones:
    - us-east-1b
    - us-east-1a
    cloudWatch:
      clusterLogging: {}
    iam:
      vpcResourceControllerPolicy: true
      withOIDC: false
    kind: ClusterConfig
    kubernetesNetworkConfig:
      ipFamily: IPv4
    managedNodeGroups:
    - amiFamily: AmazonLinux2
      desiredCapacity: 2
      disableIMDSv1: false
      disablePodIMDS: false
      iam:
        withAddonPolicies:
          albIngress: false
          appMesh: false
          appMeshPreview: false
          autoScaler: false
          awsLoadBalancerController: false
          certManager: false
          cloudWatch: false
          ebs: false
          efs: false
          externalDNS: false
          fsx: false
          imageBuilder: false
          xRay: false
      instanceSelector: {}
      instanceType: t3.medium
      labels:
        alpha.eksctl.io/cluster-name: EKS-DevSecOps-TrendMicroWorkshop
        alpha.eksctl.io/nodegroup-name: AmazonLinux
      maxSize: 3
      minSize: 2
      name: AmazonLinux
      privateNetworking: false
      releaseVersion: ""
      securityGroups:
        withLocal: null
        withShared: null
      ssh:
        allow: false
        publicKeyPath: ""
      tags:
        alpha.eksctl.io/nodegroup-name: AmazonLinux
        alpha.eksctl.io/nodegroup-type: managed
      volumeIOPS: 3000
      volumeSize: 80
      volumeThroughput: 125
      volumeType: gp3
    metadata:
      name: EKS-DevSecOps-TrendMicroWorkshop
      region: us-east-1
      version: "1.22"
    privateCluster:
      enabled: false
      skipEndpointCreation: false
    vpc:
      autoAllocateIPv6: false
      cidr: 192.168.0.0/16
      clusterEndpoints:
        privateAccess: false
        publicAccess: true
      manageSharedNodeSecurityGroupRules: true
      nat:
        gateway: Disable

- Type the command to create the Cluster and Node Group ```eksctl create cluster --config-file=eks-creation.yaml```

![EKS](/images/eksctl1.PNG)

#### To follow the creation process

- You can follow through the terminal

![EKS](/images/eksctl3.PNG)

![EKS](/images/eksctl6.PNG)

#### Or you can go to the AWS console, in the CloudFormation service 

- Follow up the events during the creation of the stack **EKS-DevSecOps-TrendMicroWorkshop** and **eksctl-EKS-DevSecOps-TrendMicroWorkshop-nodegroup-AmazonLinux**
- It can take some time
- Select the **Events** tab
- Click **Refresh**

![EKS](/images/eksctl2.PNG)

![EKS](/images/eksctl4.PNG)


---

### 1.2. Ensure both stacks status has reached Create_Complete.

- Select the **Stack info** tab

![EKS](/images/eksctl5.PNG)

---

### One of the technologies that we will use to protect our EKS Cluster is Cloud One Container Security. 

{{% notice info %}}
<p style='text-align: left;'>
To use the Continuous Compliance feature we'll need a network plugin with NetworkPolicy support, in this case we will use the <strong><a href=https://projectcalico.docs.tigera.io/about/about-calico>Project Calico</a></strong>.
</p>
{{% /notice %}}


---

### 1.1. Now Let's follow the instructions to [install Calico add-on on our EKS cluster](https://docs.aws.amazon.com/eks/latest/userguide/calico.html)

{{% notice warning %}}
<p style='text-align: left;'>
Amazon EKS doesn't maintain the manifests used in the following procedures. The recommended way to install Calico on Amazon EKS is by using the Calico Operator instead of these manifests. But as this is an ephemeral environment for us to learn we will use these manifests.
</p>
{{% /notice %}}

**To install Calico using manifests:**

    kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.1/manifests/tigera-operator.yaml

    kubectl create -f - <<EOF
    apiVersion: operator.tigera.io/v1
    kind: Installation
    metadata:
      name: default
    spec: {}
    EOF


![EKS](/images/calicoinstallation.png)

![EKS](/images/calicoinstallation2.png)

**View the resources in the calico-system namespace.**

    kubectl get daemonset calico-node --namespace calico-system


![EKS](/images/eksctl12.PNG)

---

### Let's deploy our repository in AWS ECR (Elastic Container Registry)

[![Launch Stack](https://cdn.rawgit.com/buildkite/cloudformation-launch-stack-button-svg/master/launch-stack.svg)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=ECRTrendWorkShopDevSevOps&templateURL=https://workshop-devsecops-c1-cft-templates.s3.amazonaws.com/ecr.registry.template.yaml)

![ECS](/images/ecs1.PNG)

---

### 1. Specifying stack parameters that you need to change

- **Stack Name:**  ```ECRTrendWorkShopDevSevOps```
 
- **RepositoryName** The name of the Private Repository on ECR: ```trendworkshopdevcecops```

- Click on **Next**

![ECS](/images/ecs2.PNG)

---

### 1.2. Configure the stack options

- Leave the fields as default and click **Next**, or optionally define tags to the evironment if desired.

![ECS](/images/ecs3.PNG)

![ECS](/images/ecs4.PNG)

![ECS](/images/ecs5.PNG)

---

### 1.3. Review the template parameters

- Click on **Create Stack**

![ECS](/images/ecs6.PNG)

![ECS](/images/ecs7.PNG)

![ECS](/images/ecs9.PNG)

![ECS](/images/ecs10.PNG)

![ECS](/images/ecs11.PNG)

---

### 1.4. Follow up the events during creation of the stack

- Select the **Events** tab
- Click **Refresh**

![ECS](/images/ecs12.PNG)

---

### 1.5. Ensure the stack status has reached Create_Complete.

- Select the **Stack info** tab

![ECS](/images/ecs15.PNG)

---

### 1.6. On the Outputs tab

- Click on the link to go to your Repository

![ECS](/images/ecs16.PNG)

![ECS](/images/ecs17.PNG)

---

### 1.7. Click on the **View Push Commands** button

- You will need these details below to build and push the container image that you have cloned and updated with your own details:

![ECS](/images/ecs20.PNG)

--- 

### 1.8. In VS Code

- Start copying and pasting the commands in the terminal that appear in the ECR to push your container image
- Be sure that you are in the **pygoat-tm** directory

![ECS](/images/ecs21.PNG)
![ECS](/images/ecs22.PNG)
![ECS](/images/ecs23.PNG)
![ECS](/images/ecs24.PNG)

---

### 1.9. Let's check if our image is already in our repository

- In your repository, refresh the page
- You will see that now we have our image

![ECS](/images/ecs25.PNG)

---

### 2. Go back to VS Code 

- In the terminal, type the command ```touch pygoat-deployment.yaml```
- In the left column where it shows the files, double click on the file **pygoat-deployment.yaml**
- Or type the command ```code pygoat-deployment.yaml``` to open the file

![Pygoat-Application](/images/app1.PNG)

---

### 2.1. In the pygoat-deployment.yaml file

{{% notice info %}}
<p style='text-align: left;'>
Don't forget to put your image URI on the line with the name image: your_container_image_here
</p>
{{% /notice %}}

**Paste the following:**

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: pygoat-deploy
      labels:
        app: pygoat
    spec:
      replicas: 2
      selector:
        matchLabels:
          app: pygoat
      template:
        metadata:
          labels:
            app: pygoat
        spec:
          containers:
          - name: pygoat
            image: your_container_image_here
            ports:
            - containerPort: 8000
            securityContext:
                privileged: true

{{% notice info %}}
<p style='text-align: left;'>
In this pod definition, the <strong>securityContext</strong> is set to <strong>privileged = true</strong>.
</p>
{{% /notice %}}


![Pygoat-Application](/images/app2.PNG)

---

### 2.2. Go to your repository in AWS ECR

- Copy the **Image URI**

![Pygoat-Application](/images/app3.PNG)

- And replace **your_container_image_here** which is in your **pygoat-deployment.yaml** file

![Pygoat-Application](/images/app5.PNG)

- **Save the file**

---

### 2.2. Let's deploy our Pygoat application to EKS

- Type the command ```kubectl apply -f pygoat-deployment.yaml```

![Pygoat-Application](/images/app4.PNG)

---

### 2.3. Confirm that the pods are already running

- Type the command ```kubectl get pods```
- Wait until **running** is showing


![Pygoat-Application](/images/app6.PNG)

![Pygoat-Application](/images/app7.PNG)

----

### 2.4. Let's create a LoadBalancer service to access our application externally

- Type the command ```touch loadbalancer-pygoat.yaml```
- In the left column where it shows the files, double click on the file **loadbalancer-pygoat.yaml**
- Or type the command ```code loadbalancer-pygoat.yaml``` to open the file

![Pygoat-Application](/images/app8.PNG)

![Pygoat-Application](/images/app9.PNG)

---

### 2.5. In the loadbalancer-pygoat file

**Paste the following:**

    apiVersion: v1
    kind: Service
    metadata:
      name: pygoat-loadbalancer
    spec:
      type: LoadBalancer
      selector:
        app: pygoat
      ports:
        - protocol: TCP
          port: 8000
          targetPort: 8000


![Pygoat-Application](/images/app13.PNG)

- **Save the file**
---

### 2.6. To create the LoadBalancer

- Type the command ```kubectl create -f loadbalancer-pygoat.yaml```

![Pygoat-Application](/images/app10.PNG)

---

### 2.7. Confirm that LoadBalancer is already exposed

- Type the command ```kubectl get svc```
- Copy the value that appears in **EXTERNAL-IP**

![Pygoat-Application](/images/app11.PNG)

---

### 2.8. Access the application

- In your browser paste the URL ```http://###:8000/```
- **Replace the ### with the value you got from your EXTERNAL-IP**
- It may take up to 2 to 3 minutes to access the application

![Pygoat-Application](/images/app12.PNG)

--------

### Perfect! We now have our infrastructure ready so we can use our security processes! :star-struck: :robot: :white_check_mark: :cloud: