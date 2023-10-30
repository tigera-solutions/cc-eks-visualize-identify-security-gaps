# Workshop: Amazon EKS Security Bootcamp: </br> Visualize cluster traffic and identify security gaps

## Welcome

In this EKS-focused workshop, you will work with Amazon AWS and Calico Cloud to learn how to employ Calico Cloud in order to visualize cluster traffic and pinpoint security vulnerabilities within your Kubernetes EKS cluster.

In today's highly interconnected and digital landscape, ensuring the security of your EKS Kubernetes clusters is an absolute necessity. This workshop provides you with the essential knowledge and skills to strengthen your cluster's defenses thoroughly, guaranteeing the safeguarding of vital workloads and sensitive information. It enables you to tailor security measures to suit your organization's specific needs and keeps you at the forefront of cybersecurity in a swiftly evolving environment.

The field of cybersecurity is currently experiencing a strong demand, and acquiring the skill set to effectively secure Kubernetes environments is highly valuable. Regardless of whether you're an IT administrator, developer, or a security professional, the knowledge gained from this workshop will significantly improve your expertise, making you an indispensable asset to your organization. Furthermore, by proactively implementing robust security measures, you can protect your organization from potential financial losses and reputational harm that may arise from security breaches.

Make sure not to overlook this chance to enhance your security expertise, safeguard your infrastructure, and propel your professional development forward.

Upon completing this workshop, you will gain insights into how professionals in your industry secure and monitor cloud-native applications in Amazon AWS. You'll also acquire valuable best practices that you can apply within your organization.

### Time Requirements

The estimated time to complete this workshop is 60-90 minutes.

### Target Audience

- Cloud Professionals
- DevSecOps Professional
- Site Reliability Engineers (SRE)
- Solutions Architects
- Anyone interested in Calico Cloud :)

### Learning Objectives

- Learn how to **analyze service-to-service communication** to evaluate the security risk posed by network-based threats.
- Visualize **notifications** when **malware is executed** within your workloads.
- **Detect and prevent** anomalous behaviors such as attempts to **access restricted URLs**.
- Discover how to **quarantine workloads** to prevent the **lateral movement** of the threat.
- Learn how to build and enforce **security policy** to stop **command and control attack**.

## Workshop Environment Preparation

> :warning: **For this workshop, you are expected to have access to a previously created AKS cluster.**

- Please, follow the instructions on the repository below if you don't have it ready: 

  [Calico Cloud on EKS - Workshop Environment Preparation](https://github.com/tigera-solutions/eks-workshop-prep)

- We will run this workshop from the Azure Cloud Shell, as described in that repository.

- To start your cluster, reload the environment variables create in your Azure Cloud Shell first and then start the cluster. Use the following command:

  ```bash
  source ~/workshopvars.env
  az aks start --resource-group $RESOURCE_GROUP --name $CLUSTERNAME
  ```

## Modules

This workshop is organized in sequential modules. One module will build up on top of the previous module, so please, follow the order as proposed below.

Module 1 - [Connect the EKS cluster to Calico Cloud](/mod/module-1-connect-calicocloud.md)  
Module 2 - [Analyze service-to-service communication](/mod/module-2-analyze-communications.md)  
Module 3 - [Malware protection, access control and quarantine with Thread Defence](/mod/module-3-threat-defense.md)  
Module 4 - [Enforcing security policy to stop C&C attacks](/mod/module-4-security-policies.md)  
Module 5 - [Clean up](/mod/module-5-clean-up.md)  

--- 

### Useful links

- [Project Calico](https://www.tigera.io/project-calico/)
- [Calico Academy - Get Calico Certified!](https://academy.tigera.io/)
- [O’REILLY EBOOK: Kubernetes security and observability](https://www.tigera.io/lp/kubernetes-security-and-observability-ebook)
- [Calico Users - Slack](https://slack.projectcalico.org/)

**Follow us on social media**

- [LinkedIn](https://www.linkedin.com/company/tigera/)
- [Twitter](https://twitter.com/tigeraio)
- [YouTube](https://www.youtube.com/channel/UC8uN3yhpeBeerGNwDiQbcgw/)
- [Slack](https://calicousers.slack.com/)
- [Github](https://github.com/tigera-solutions/)
- [Discuss](https://discuss.projectcalico.tigera.io/)

> **Note**: The workshop provides examples and sample code as instructional content for you to consume. These examples will help you understand how to configure Calico Cloud and build a functional solution. Please note that these examples are not suitable for use in production environments.