---
title: "Cleanup your Environment"
chapter: false
weight: 140
pre: "<b>9. </b>"
---

Now that we have successfully completed this lab, to avoid charges please follow the documentation steps to remove resources deployed during this workshop.
---

{{% notice info %}}
<p style='text-align: left;'>
<b>If you subscribed to Cloud One in your AWS account, You can use this <a href="https://cloudone.trendmicro.com/docs/billing-and-subscription-management/billing-method-change/#Marketplace">link</a> to Unsubscribe to Cloud One on AWS Marketplace.</b>
</p>
{{% /notice %}}

---

#### 1. Delete the Cloud One Network Security Routes

- On **Route Table** ```DevSec WorkShop Edge association rtb```
- Click on the **Edit routes** button

![cleanup](/images/c1ns_cleanup.PNG)

- **And delete the routes to the Network Security Endpoint**

![cleanup](/images/c1ns_cleanup1.PNG)

- Click on the **Save Changes** button

![cleanup](/images/c1ns_cleanup2.PNG)

- Click on the **Edge Association** tab and on the **Edit edge association** button

![cleanup](/images/c1ns_cleanup3.PNG)

- Unselect the **Internet Gateway** and click on the **Save changes** button

![cleanup](/images/c1ns_cleanup4.PNG)

- On the **Actions** button click on **Delete** and delete this route table

![cleanup](/images/c1ns_cleanup5.PNG)

- On **Route Table** ```eksctl-EKS-DevSecOps-TrendMicroWorkshop-cluster/PublicRouteTable```
- Click on the **Edit routes** button

![cleanup](/images/c1ns_cleanup6.PNG)

- **And delete the routes to the Network Security Endpoint**

![cleanup](/images/c1ns_cleanup7.PNG)

- Click on the **Save Changes** button

![cleanup](/images/c1ns_cleanup8.PNG)

- On **Route Table** ```Public subnet az1b route table```
- Click on the **Edit routes** button

![cleanup](/images/c1ns_cleanup9.PNG)

- **And delete the routes to the Network Security Endpoint**

![cleanup](/images/c1ns_cleanup10.PNG)

- Click on the **Save Changes** button

![cleanup](/images/c1ns_cleanup11.PNG)

---

- Go back to the **Cloud One Network Security console**
- On **Hosted Infrastructure**, expand your VPC
- And for **both Network Security Endpoint** click on the **delete** icon  

![cleanup](/images/c1ns_cleanup12.PNG)

![cleanup](/images/c1ns_cleanup13.PNG)

![cleanup](/images/c1ns_cleanup14.PNG)

- Wait until both are deleted
- You can click on the **Refresh** button

![cleanup](/images/c1ns_cleanup15.PNG)

---

#### 2. Delete the Load Balancers

- On the AWS search bar, search for ```load balancers```

![cleanup](/images/load_balancer_cleanup.PNG)

- Select the two load balancers created during the workshop, click on **Actions** and then **Delete**
- You can be sure which **Load Balancer** is by the **DNS NAME** or the VPC

![cleanup](/images/load_balancer_cleanup1.PNG)

![cleanup](/images/load_balancer_cleanup2.PNG)

---

#### 3. Delete the EKS Clusters

- Select the stack named: **eksctl-EKS-QA-TrendMicroWorkshop-cluster** 
- Click the **Delete** button

![cleanup](/images/eksqa_cleanup3.PNG)

![cleanup](/images/eksqa_cleanup4.PNG)

![cleanup](/images/eksqa_cleanup5.PNG)

- Select the stack named **eksctl-EKS-QA-TrendMicroWorkshop-nodegroup-AmazonLinux** to be deleted.
- Click the **Delete** button

![cleanup](/images/eksqa_cleanup.PNG)

![cleanup](/images/eksqa_cleanup1.PNG)

![cleanup](/images/eksqa_cleanup2.PNG)

---

- Select the stack named: **eksctl-EKS-DevSecOps-TrendMicroWorkshop-cluster**  
- Click the **Delete** button

![cleanup](/images/eks_cleanup3.PNG)

![cleanup](/images/eks_cleanup4.PNG)

![cleanup](/images/eks_cleanup5.PNG)

- Select the stack named: **eksctl-EKS-DevSecOps-TrendMicroWorkshop-nodegroup-AmazonLinux** to be deleted.
- Click the **Delete** button

![cleanup](/images/eks_cleanup.PNG)

![cleanup](/images/eks_cleanup1.PNG)

![cleanup](/images/eks_cleanup2.PNG)

---

#### 4. Delete the ECR template.
- Go to the **trendworkshopdevcecops** repository on AWS ECR, and delete the images there

![cleanup](/images/ecr_stack_delete1.PNG)

![cleanup](/images/ecr_stack_delete2.PNG)

![cleanup](/images/ecr_stack_delete3.PNG)

- Select the stack named: **ECRTrendWorkShopDevSevOps**
- Click the **Delete** button

![cleanup](/images/ecr_stack_delete.PNG)

![cleanup](/images/ecr_stack_delete4.PNG)

![cleanup](/images/ecr_stack_delete5.PNG)

---

#### 5. Delete the Pygoat-TM-Application-Repo-Trend-Workshop template.

- Select the stack named **Pygoat-TM-Application-Repo-Trend-Workshop**
- Click the **Delete** button
- The Stack will be deleted

![cleanup](/images/codecommit_stack_delete.PNG)

![cleanup](/images/codecommit_stack_delete1.PNG)

![cleanup](/images/codecommit_stack_delete2.PNG)

---

#### 6. Delete the DevSecTrendMicroPipeline template.
- Select the stack named: **DevSecTrendMicroPipeline**
- On the **Outputs** tab click on the S3 Bucket link

![cleanup](/images/devsectrendmicropipeline_stack_delete.PNG)

- Delete the file

![cleanup](/images/devsectrendmicropipeline_stack_delete1.PNG)

![cleanup](/images/devsectrendmicropipeline_stack_delete2.PNG)

![cleanup](/images/devsectrendmicropipeline_stack_delete3.PNG)

- Back to the **DevSecTrendMicroPipeline stack**
- Click the **Delete** button

![cleanup](/images/devsectrendmicropipeline_stack_delete4.PNG)

![cleanup](/images/devsectrendmicropipeline_stack_delete5.PNG)

![cleanup](/images/devsectrendmicropipeline_stack_delete6.PNG)

---

#### 7. Delete the devsecops-lab-workshop template.
- Select the stack named: **devsecops-lab-workshop**
- Click the **Delete** button
- The Stack will be deleted

![cleanup](/images/juiceshop_stack_delete.PNG)

![cleanup](/images/juiceshop_stack_delete1.PNG)

![cleanup](/images/juiceshop_stack_delete2.PNG)

---

#### 8. Delete the DevEnvTrendWorkShop and DevEnvTrendWorkShop-PolicyStack template.
- Select the stack named: **DevEnvTrendWorkShop**
- Click the **Delete** button
- Both Stacks will be deleted

![cleanup](/images/devenv_stack_delete.PNG)

![cleanup](/images/devenv_stack_delete1.PNG)

![cleanup](/images/devenv_stack_delete2.PNG)

------

### Thank you for joining our workshop! We hope you enjoyed this and learned new things! :clap: :clap: