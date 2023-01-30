---
title: "Security, How it works?! - Lab"
chapter: true
weight: 20
pre: "<b>3. </b>"
---

## Open Source Tools - Lab
In previous sessions we were introduced a little bit about DevOps, DevSecops, SDL etc. Now, instead of reading more text, we are going to make a Lab focused on Application Security, and for that, we will use the [OWASP Top 10](https://owasp.org/Top10/) to exploit two vulnerability in an already vulnerable application.

According to [OWASP](https://owasp.org/www-project-top-ten/) "***The OWASP Top 10 is a standard awareness document for developers and web application security. It represents a broad consensus about the most critical security risks to web applications. Using the OWASP Top 10 is perhaps the most effective first step towards changing the software development culture within your organization into one that produces more secure code.***"

#### To warm up, we will explore 2 vulnerabilities in our application. We will also use the project [Juice Shop](https://github.com/juice-shop/juice-shop) as a vulnerable application.

---

## 1. Creating the JuiceShop environment using AWS CloudFormation Template

You can use the CloudFormation template below to create the infrastructure in same AZ with the 2 subnets, Internet Gateway, and the EC2 instance.

[![Launch Stack](https://cdn.rawgit.com/buildkite/cloudformation-launch-stack-button-svg/master/launch-stack.svg)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=devsecops-lab-workshop&templateURL=https://workshop-devsecops-c1-cft-templates.s3.amazonaws.com/CFT_JuiceShop.yml)

![JuiceShop](/images/JuiceShop.png) 

---

### 2. Click on **Next**

![C1WS](/images/create_env_3.PNG) 

---

### 3. Specifying additional stack parameters

{{% notice info %}}
<p style='text-align: left;'>
A Key Pair is required before continuing this CloudFormation deployment. If you need help creating a Key Pair -> <a href=https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair>Create a key pair</a>
</p>
{{% /notice %}}


<details>
  <summary> -> <code>CLICK HERE</code> to see how to create key pair</summary>

**AWS Console -> EC2 -> Key Pairs -> Create key pair**

![C1WS](/images/create_env_9.png) 

</details>
<br>

- **Stack Name:**  ```devsecops-lab-workshop```

- **VPCCIDR:** the CIDR that you want to use for the VPC. You don't need to change it, if you don't want to change the default configuration.
    
- **KeyPair:** Your KeyPair name

- **MyPublicIP:** Your public IP in CIDR. (e.g. 191.162.228.91/ 32 . You can grab it using a few tools or websites (https://ipinfo.io/ip). It will be use to only allow your IP to access the vulnerable application (JuiceShop))

- **ProtectedPublicSubnetCIDR:** the AZ what you want to use to your Public subnet. You don't need to change it, if you don't want to.

---

### 3.1. Stack details example:

![C1WS](/images/create_env_4.PNG) 

---

### 4. Configure stack options

Leave as fields as default and click **Next**, or optionally define tags to the evironment if desired.

![C1WS](/images/create_env_5.png) 

---

### 5. Review the template parameters 
- Mark the checkbox the **"I acknowledge... "**
- Click on **Create Stack**

![C1WS](/images/create_env_6.PNG) 

![C1WS](/images/create_env_7.png) 

![C1WS](/images/create_env_8.png) 

---

### 6. Follow up the events during creation of the stack.
- Select the **Events** tab
- Click **Refresh**

![C1WS](/images/create_env_10.PNG) 

---

### 7. Ensure the stack status has reached **Create_Complete**. 
- Select the **Stack info** tab

![C1WS](/images/create_env_11.PNG) 


### 8. Select the **Outputs** tab. 
- Copy the value of the key **DVWAPublicDNSName**

![SecurityTesting](/images/create_env_12.PNG)

<ul>
<li>In our created Juice Shop, open <strong>Google Chrome</strong> and navigate to the Public DNS Name </li>

</ul>

![SecurityTesting](/images/create_env_13.PNG)


<h3><span style="color:red">It may take 3 minutes for the application to be accessible</span></h3>

#### Awesome! Our application is working! 

---

### The first vulnerability we will test will be about Security Misconfiguration, which in the OWASP Top 2021 is ranked 5th

In the definition that we can find on the [OWASP](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/) A05:2021 – Security Misconfiguration "***The application might be vulnerable if the application is:***

- ***Missing appropriate security hardening across any part of the application stack or improperly configured permissions on cloud services.***
- ***Unnecessary features are enabled or installed (e.g., unnecessary ports, services, pages, accounts, or privileges).***
- ***Default accounts and their passwords are still enabled and unchanged.***
- ***Error handling reveals stack traces or other overly informative error messages to users.***
- ***For upgraded systems, the latest security features are disabled or not configured securely.***
- ***The security settings in the application servers, application frameworks (e.g., Struts, Spring, ASP.NET), libraries, databases, etc., are not set to secure values.***
- ***The server does not send security headers or directives, or they are not set to secure values.***
- ***The software is out of date or vulnerable (see [A06:2021-Vulnerable and Outdated Components)](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/).***"

In this attack we will try to purposely crash this application and see if we can get it to return some leak information about the configuration.

![SecurityMisconfiguration](/images/jc-securitymisconfiguration.PNG) 

### 1. On the website, we will click in the upper right corner on "Account" and then "Login"

![SecurityMisconfiguration](/images/jc-securitymisconfiguration1.PNG) 

### 2. In the email field we can put any name and ' at the end 

- <strong>Example to use in the browser:</strong> ```bruce'```

And in password, any value

Notice that instead of returning an error message that this user and password do not exist or some other more comprehensive message for the user, it returned a message **"[object Object]"**

![SecurityMisconfiguration](/images/jc-securitymisconfiguration2.PNG) 

### 3. Still in the browser, let’s investigate how the application responded to our request.

- If you are in Chrome, press the key **F12** from your keyboard.

Click in **"Network",** click on "Login" again in the application, on "login" in the debug area, and finally expand the message.

![SecurityMisconfiguration](/images/jc-securitymisconfiguration3.PNG) 

#### Okay, here we can see the response that the application returned to us, and we can already see that it gave us more information than a normal client of the application should know. Like what is the database used and even the Query made.

---

#### The next attack will be a simple Improper Input Validation. In this case, in the application code there is a # that was not properly encoded. Therefore, the image does not load.

### 1. In the upper left corner, click on the hamburger icon and select the option "Photo Wall"

![ImproperInputValidation](/images/jc-improperInputValidation0.PNG) 

![ImproperInputValidation](/images/jc-improperInputValidation01.PNG) 

### 2. With the Mouse over the image that has not yet been loaded "#zatschi #whoneedsfourlegs", right click on it and "Inspect"

![ImproperInputValidation](/images/jc-improperInputValidation.PNG) 

### 3. Double click on #zatschi and replace with %23, do the same thing with #whoneedsfourlegs and also replace # with %23
- We can check a table in [TutorialsPoint](https://www.tutorialspoint.com/html/html_url_encoding.htm") to know which characters, decimals etc are equivalent for URL encode.

![ImproperInputValidation](/images/jc-improperInputValidation1.PNG) 

![ImproperInputValidation](/images/jc-improperInputValidation2.PNG) 

### Et voilà, in the next part we'll start to understand the tools and processes of our application and Pipeline! :laptop: :rocket:
