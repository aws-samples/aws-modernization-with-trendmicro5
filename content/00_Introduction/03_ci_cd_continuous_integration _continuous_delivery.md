---
title: "CI/CD - Continuous Integration && Continuous Delivery" 
chapter: false
weight: 7
---

### What is Continuous Integration CI and Continuous Delivery CD?
CI/CD makes it possible to automate security and quality tests, accelerate development and make more frequent deliveries of our application to the users. The main advantages can be running automated tests, remove manual errors, deploying to a staging or production environment and so on.

#### Difference between the two
Continuous integration, which is an automation process for testing, is the ideal solution to avoid conflicts between new updates when many releases of the application are developed and must be tested to search for bugs. Continuous Delivery or Continuous Deployment are related concepts but will be used to specify in separate cases to demonstrate the level of automation present when deploying the application, whether it will be in a test environment or in a production environment.

#### Continuous Integration
As mentioned before, in the CI phase, several tests are automated and made to ensure that the changes do not create any bugs in the application or in any new feature that is to be implemented. In case of a security or quality issue is actually detected or a merging error that will cause an compatibility issue of the new code in the code that already exists, in this phase, it is facilitated the correction of these bugs quickly.

The main goals of continuous integration is to find and investigate where in the infrastructure build, templates and scripts, as well as the application code, that was created and validated have issues and give quick feedbacks to the teams involved on the process.

---

The main difference between Continuous Delivery and Continuous Deployment is that in Continuous Delivery the code is tested first before it is deployed in a Dev or Quality Assurance (QA) environment. While in Continuous Deployment, the application after tested is directly deployed into the production environment automatically. 

- **Continuous Delivery**
In Continuous Delivery, code that has already passed the tests (CI phase) will be sent to a test or QA environment to analyze and observe the behavior and if the application is stable after the new changes made to the code, and finally before the final step to production, a person on the team can decide if the final push should happen or not.

- **Continuous Deployment**
It is the complement of Continuous Delivery that will automate the release of the build with the changes made to the code for the production environment automatically.

![SecurityTesting](/images/sectesting.PNG)

Image from [Snyk's State of Cloud Native Application Security report](https://snyk.io/state-of-cloud-native-application-security/?utm_campaign=CNAS-SC-2021&utm_medium=Social&utm_source=Twitter-Organic&utm_content=CNAS-2021-Report) on some companies where security testing takes place.

--- 

### Now, we'll finally go to our first Lab and put some of the knowledge we've seen here into practice!! :laptop: :rocket: