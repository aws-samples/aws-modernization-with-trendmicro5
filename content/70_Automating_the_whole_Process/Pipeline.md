---
title: "Pipeline"
chapter: false
weight: 61
pre: "<b>6.1 </b>"
---

---

Finally let's deploy everything we've done so far automatically so that we have our environment safe, and have a single source of truth for our entire environment, with our security processes included.

![CodePipeline](/images/DevSecOpsWorkshopPipelineDiagram.png)

---

### 1. First let's create our Git Repository in AWS CodeCommit

[![Launch Stack](https://cdn.rawgit.com/buildkite/cloudformation-launch-stack-button-svg/master/launch-stack.svg)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=Pygoat-TM-Application-Repo-Trend-Workshop&templateURL=https://workshop-devsecops-c1-cft-templates.s3.amazonaws.com/codecommit.repository.template.yaml)

![CodeCommit](/images/CodeCommit1.PNG)

---

### 1.2. Stack Parameters

- Leave the parameters as they are, you don't need to change anything.

![CodeCommit](/images/CodeCommit2.PNG)

---

### 1.3. Configure the stack options

- Leave the fields as default and click **Next**, or optionally define tags to the evironment if desired.

![CodeCommit](/images/CodeCommit3.PNG)

![CodeCommit](/images/CodeCommit4.PNG)

---

### 1.4. Review the template parameters

- Click on **Create Stack** or **Submit**

![CodeCommit](/images/CodeCommit5.PNG)

![CodeCommit](/images/CodeCommit6.PNG)

![CodeCommit](/images/CodeCommit7.PNG)

---

### 1.5. Follow up the events during creation of the stack

- Select the **Events** tab
- Click **Refresh**

![CodeCommit](/images/CodeCommit8.PNG)

---

### 1.6. Ensure the stack status has reached Create_Complete

- Select the **Stack info** tab

![CodeCommit](/images/CodeCommit9.PNG)

---

### 1.7. On the Outputs tab

- Click on the link to go to your Repository

![CodeCommit](/images/CodeCommit10.PNG)

![CodeCommit](/images/CodeCommit11.PNG)

---

### 1. Deploy the Pipeline

[![Launch Stack](https://cdn.rawgit.com/buildkite/cloudformation-launch-stack-button-svg/master/launch-stack.svg)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=DevSecTrendMicroPipeline&templateURL=https://workshop-devsecops-c1-cft-templates.s3.amazonaws.com/pipe.yaml)

![Pipeline](/images/CodePipe1.PNG)

---

### 1.1. Specifying stack parameters that you need to change

{{% notice info %}}
<p style='text-align: left;'>
<b> If you still don't have a Cloud One API Key with Full Access created yet. Go check how to create </b> <a href="http://localhost:1313/40_finding_risks_in_open_source_dependencies/01_security_tools.html#1-lets-log-in-to-trend-micro-cloud-onehttpscloudonetrendmicrocom-1">HERE</a>!
</p>
{{% /notice %}}

- **CloudOneApiKey**: **Full Access API Key** generated under your Cloud One dashboard. You can use the same one created before during this Workshop. **Remember that this API Key must have Full Access**.
- **CloudOneRegion**: Your Cloud One Region
- Click on **Next**

![Pipeline](/images/CodePipe2.PNG)

---

### 1.2. Configure the stack options

- Leave the fields as default and click **Next**, or optionally define tags to the evironment if desired.

![CodeCommit](/images/CodePipe11.PNG)

![CodeCommit](/images/CodeCommit4.PNG)

---

### 1.3. Review the template parameters

- Click on **Create Stack** or **Submit**

![Pipeline](/images/CodePipe3.PNG)

![Pipeline](/images/CodePipe4.PNG)

![Pipeline](/images/CodePipe5.PNG)

![Pipeline](/images/CodePipe6.PNG)

---

### 1.4. Follow up the events during creation of the stack

- Select the **Events** tab
- Click **Refresh**

![Pipeline](/images/CodePipe7.PNG)

---

### 1.5. Ensure the stack status has reached Create_Complete

- Select the **Stack info** tab

![Pipeline](/images/CodePipe8.PNG)

---

### 1.6. On the Outputs tab

- Click on the link to go to your Pipeline

![Pipeline](/images/CodePipe9.PNG)

- You will be redirect to the Pipeline

---

### 2. Follow the execution of the Pipeline

![Pipeline](/images/CodePipe10.PNG)

---

### 2.1. Second Action - Scan-The-Code-With-Horusec

{{% notice warning %}}
<p style='text-align: left;'>
Sometimes this action could fail due to the maximum number of requests to Docker Hub has been reached. You can click on the <b>Retry button</b> that appears when an action fails.
</p>
{{% /notice %}}

![Pipeline](/images/CodePipeError.PNG)

![Pipeline](/images/CodePipeError2.PNG)

---

#### **On this action the code of the application will be scanned to find vulnerabilities.**

![Pipeline](/images/CodePipe13.PNG)

- Click on the **Details** button to see the log for this action and the Horusec findings.

![Pipeline](/images/CodePipe35.PNG)

![Pipeline](/images/CodePipe14.PNG)

---

### 2.2. Third Action - Create-EKS-Cluster

#### **On this action a AWS Elastic Kubernetes Service Cluster will be created.**

![Pipeline](/images/CodePipe15.PNG)

![Pipeline](/images/CodePipe15.1.PNG)

![Pipeline](/images/CodePipe15.2.PNG)

![Pipeline](/images/CodePipe16.PNG)

---

### 2.3. Fourth Action - Scan-The-Infra-With-Conformity

#### **On this action Cloud One Conformity will scan the EKS Cloud Formation Template deployed to identify security and governance risks.**

![Pipeline](/images/CodePipe17.PNG)

- Click on the **Details** button to see the log for this action and the Cloud One Conformity findings.

![Pipeline](/images/CodePipe34.PNG)

![Pipeline](/images/CodePipe18.PNG)

---

### 2.4. Fifith Action - Scan-for-Secrets

#### **On this action the repository will be scanned by Detect Secrets to find any secrets in the repo.**

![Pipeline](/images/CodePipe33.PNG)

- Click on the **Details** button to see the log for this action and the Detect Secrets findings.

![Pipeline](/images/CodePipe38.PNG)

![Pipeline](/images/CodePipe20.PNG)

---

### 2.5. Sixth Action - ManualApprove

#### **On this action you decide if the vulnerabilities found in the code and infrastructure are acceptable or not. And aprove or reject the changes. If you reject the changes the pipeline will stop the execution, accepting the changes and the pipeline will continue.**

- Click on the **Review** button to approve or reject.  

![Pipeline](/images/CodePipe21.PNG)

- Click on the **Approve** button to aprove the changes and move to the next Action.

![Pipeline](/images/CodePipe22.PNG)

![Pipeline](/images/CodePipe23.PNG)

---

### 2.6. Seventh Action - Deploy-the-App-to-EKS

{{% notice warning %}}
<p style='text-align: left;'>
Sometimes this action could fail due to the maximum number of requests to Docker Hub has been reached. You can click on the <b>Retry button</b> that appears when an action fails.
</p>
{{% /notice %}}

![Pipeline](/images/CodePipeError.PNG)

![Pipeline](/images/CodePipeError3.PNG)

---

#### **On this action it will build the docker image, push to the AWS ECR trendworkshopdevcecops repository we created previous on the workshop, deploy the container with the app to EKS and create the Load Balancer.**

![Pipeline](/images/CodePipe24.PNG)

- Click on the **Details** button to see the logs.

![Pipeline](/images/CodePipe36.PNG)

![Pipeline](/images/CodePipe25.PNG)

![Pipeline](/images/CodePipe32.PNG)

---

### 2.7. Eighth Action - CloudOne-ContainerSecurity-RunTime-Deploy 

#### **On this action will deploy the Cloud One Container Security Runtime to the EKS Cluster, create a Cluster on Container Security console and associate it with the policy TrendMicroDevSecOpsWorkshopPolicy that we created before.**

![Pipeline](/images/CodePipe26.PNG)

- Click on the **Details** button to see the logs.

![Pipeline](/images/CodePipe37.PNG)

![Pipeline](/images/CodePipe27.PNG)

---

### 3. On the Cloud One Container Security console

![Pipeline](/images/ekssc17.PNG)

- You will see that now you already have a Cluster created called **DevSec_EKS_QA_TrendMicroWorkshop** with the policy we created earlier in the step of ***Deploy Security Tools - Container Security*** on the Workshop.

![Pipeline](/images/CodePipe28.PNG)

- If you go to the **Events** tab there will have some events about our newly deployement through the Pipeline. Our deployment has a Privileged Container, because of this the pod has been isolated.

![Pipeline](/images/CodePipe29.PNG)

---

### 3.1. Let's try access the application

- On the **Deploy-the-App-to-EKS** Action click on **Details**

![Pipeline](/images/CodePipeDetails.PNG)

- On the bottom of the page will have the **Application URL** 

![Pipeline](/images/CodePipe30.PNG)

- Copy and paste on the browser ```http://###:8000```

![Pipeline](/images/CodePipe31.PNG)

- You will not be able to access because the pods were isolated, because it was attempt to deploy a **Privileged Container**.

-----

#### Congrats now your environment is more secure automatically!! :grin: :sunny: