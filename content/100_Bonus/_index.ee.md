---
title: "Bonus - How to protect the Developer Environment"
chapter: true
weight: 70
pre: "<b>7. </b>"
---

# Bonus - How to protect the Developer Environment

### In the previous exercises we leveraged the Cloud One Platform to protect our applications and Application environment; but what about the Developers environment, the machines, and the Cloud Environment as a whole?

### For that we will leverage two other Trend Micro Cloud One Technologies

---

### Trend Micro Conformity 

In this workshop, we learned how to scan our IaC templates with Cloud One Conformity, now let’s integrate our AWS account with Conformity to have a more holistic view of the possible drifts and misconfigurations within our 80+ different AWS services within a single multi-cloud dashboard. By integrating our AWS account with Cloud One Conformity we will be able to continuously analyze our risk status, remediate violations with step-by-step guides to improve our security and compliance posture.

For more information on Workload Security, please visit [Trend Micro Cloud One™ – Conformity](https://www.trendmicro.com/en_ae/business/products/hybrid-cloud/cloud-one-conformity.html)

---

### 1. Log in to your Cloud One account and click on the Conformity box

![C1C](/images/c1cIntegration1.PNG)

---

### 1.1. Select AWS Account to add a AWS account

- Click **Next**

![C1C](/images/c1cIntegration2.PNG)

---

### 1.3. Give it an Account name and Environment name for the AWS account inside Conformity

- **Account name**: ```TrendDevSecWorkshopAWSAccount```

- **Environment Name**: ```DevSecWorkshop```

- Click **Next**

![C1C](/images/c1cIntegration3.PNG)

---

### 1.4. Select **Automated Setup** to use Cloud Formation Template and automatically integrate your AWS Account with Conformity

- Click **Next**

![C1C](/images/c1cIntegration4.PNG)

---

### 1.5. Lauch the CloudFormation Template in your AWS Account

- In the same browser we are Logged on your AWS Account, click on the **Launch Stack** Button

![C1C](/images/c1cIntegration5.PNG)

- It will open a new tab, in your AWS Account

![C1C](/images/c1cIntegration6.PNG)

--- 

### 1.6. Ensure the stack status has reached Create_Complete

- Select the **Stack info** tab

![C1C](/images/c1cIntegration7.PNG)

---

### 1.7. On the Outputs tab

- Copy the **CloudConformityRoleArn** Value

![C1C](/images/c1cIntegration8.PNG)

- And paste the value on the **ARN field** in Conformity

![C1C](/images/c1cIntegration9.PNG)

- Then click **Next**

- The Account was successfully set up

![C1C](/images/c1cIntegration10.PNG)

- Then click **Next**

---

### Wait until the first Scan is finished in the AWS Account

![C1C](/images/c1cIntegration11.PNG)

- Now in the Conformity Dashboard we could see areas that would require attention so that we could start applying best practices and remediations to improve our infrastructure.

---

### If you want to explore more about Cloud One Conformity, we have a workshop about Securing AWS Infrastructure with Trend Micro - Cloud One Conformity - **[HERE](https://trendmicro.awsworkshop.io/)** -

---

### Trend Micro Workload Security

Get started with Workload Security and embrace the robust server protection offered, so you can protect all your servers where you need it, without disrupting your business applications or processes.

For more information on Workload Security, please visit [Trend Micro Cloud One™ – Workload Security](https://www.trendmicro.com/en_ae/business/products/hybrid-cloud/cloud-one-workload-security.html)

---

### 1. Log in to your Cloud One account and click on the Endpoint & Workload Security box

![C1WS](/images/c1wsIntegration1.PNG)

---

### 1.2. Click on the **Suport** button 

- And **Deployment Script**

![C1WS](/images/c1wsIntegration2.PNG)

---

### 1.3. Chose the platform of your machine and a policy from the list

- **Save to file** or **Copy to Clipboard** the commands to download and install the agent in your machine 

![C1WS](/images/c1wsIntegration3.PNG)

---

### 1.4. Let's install the Workload Security agent on your Developer Machine

- In the terminal, create the file with the command ```touch DeploymentScript.sh```
- Open the file ```code DeploymentScript.sh```
- Paste the comands from the Workload Security Console

![C1WS](/images/c1wsIntegration4.PNG)

---

### 1.5. Give execution permission to the file

- Type the command ```chmod +x DeploymentScript.sh```
- Execute the file ```sudo ./DeploymentScript.sh```

![C1WS](/images/c1wsIntegration5.PNG)

---

### 1.6. Wait until the agent is downloaded and instaled

![C1WS](/images/c1wsIntegration6.PNG)

---

### 1.7. Go back to the console

- On the **Computers** tab
- Search for you Computer name

![C1WS](/images/c1wsIntegration7.PNG)

---

### If you want to explore more about Cloud One Endpoint & Workload Security, we have a workshop about how to automate security and protect your server fleet - **[HERE](https://cloud-one-security.awsworkshop.io/)** -

---

#### Want to learn about the other valuable Trend Micro Cloud One solutions? please visit [Trend Micro Cloud One™](https://www.trendmicro.com/cloudone)

-------
