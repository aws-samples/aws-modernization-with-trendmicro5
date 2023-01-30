---
title: "Security Concepts"
chapter: false
weight: 6
---

### CIA Triad
CIA stands for Confidentiality, Integrity and Availability, and is basically the core when we talk about Information Security.

- **Confidentiality:** One of the components of privacy implemented to secure your data from unauthorized users. It is the most important attribute for the safety of your data. You have to have control of who can see your data. In other words, Only authorized persons should access certain content, data, etc. authorized to them.

- **Integrity:** Data cannot be modified in an unauthorized or undetected manner. When the data changes without your permission, it could lead to unintended consequences.

- **Availability:** The information must be available always when it is needed. Always aim to remain available at all times, to prevent disruptions.

![TriadCIA](/images/triad.PNG)

---

### SDL - Security Development Lifecycle
Security Development Lifecycle is a process to ensure security when developing software, and help developers build secure software and address security compliance requirements. Also secure the application in order to reduce the number and severity of vulnerabilities in the software and make the software secure enough.

Some interesting points in SDL for our Workshop:

#### Training: 
Devs and Software Engineers must at least have some understanding of security fundamentals. For example, educating on attacks behavior and security terminology.

#### Conformity:
Define the minimum acceptable levels of security quality.

#### Approved tools:
Define and publish a list of approved tools that developers can use.

#### Perform Security testing on third-party library - Software Composition Analysis (SCA):
Nowadays it is very rare for a developer to write code from scratch. It is important to understand the impact that a security vulnerability in these libraries can have on the security of your software.

#### Perform Static Security Test - Static Application Securing Test (SAST):
Analyze the source code. Security code reviews focus on vulnerabilities and security issues. It's typically integrated into the pipeline to identify vulnerabilities in the code at each commit, push or even pull request.

#### Perform Dynamic Security Test - Dynamic Application Securing Test (DAST):
Will look for strange behavior in the application while it is running. It can also be integrated into the Pipeline, it is mostly performed at the end of the process, and is usually done when the application is working at least on a Quality Assurance environment.

--- 

### Threat Modeling
One of the definitions that we can find on [Owasp](https://owasp.org/www-community/Threat_Modeling) "Threat modeling works to identify, communicate, and understand threats and mitigations within the context of protecting something of value. A threat model is a structured representation of all the information that affects the security of an application. In essence, it is a view of the application and its environment through the lens of security." 

#### Why Should we Threat Model?
One of the main advantages of doing Threat Modeling is that you will be able to recognize a problem in your system before you even start to develop, test and even implement it. And with that, mitigate the problem more quickly and not have to wait for it to appear in later phases in the project.

#### Phases
- Define a security scope;
- What can go wrong in the system;
- How can you manage and mitigate the risks encountered; 
- Assess what has been done and identify whether threats have been addressed/resolved;


#### Advantage
Possibility of finding and fixing bugs in the initial stages of system design, continuous refinement throughout the development cycle, knowing what attacks will occur and that response is taken to minimize impact and risks;


--- 

### Secure by Design
It is the ability of the software to have security from its inception. You need to start thinking beyond shift-left in your code and start thinking about security even before that. Security requirements are defined first to guide developers. Security must be considered and built into the software on each layer, in order to ensure the security of the software, it's important to map attacks tactics and patterns in the software development in order to maintain security. Malicious attacks and Security vulnerabilities must be anticipated on the software and should be assumed to occur, and you should prepare to minimize impact. For example, we can see some principles that are taken into account when we think about Secure by Design:

- Weakest Link;

- Economy of Mechanism;

- Leveraging Existing Components;

- Fail Safe;

- Least Privilege;

---

### If you want, Trend Micro has an article that you can learn more about [Security Best Practices](https://www.trendmicro.com/pt_br/devops/22/d/secure-application-development.html)