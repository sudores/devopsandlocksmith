---
title: "Implementing IaC, foreword"
date: 2022-01-03T23:11:04+02:00
tags: [IaC,iac,IaaC,iaac,devops,ansible,terraform,jenkins]
categories: [DevOps,IaC,Implementation]
draft: false
---

# Implementing IaC, foreword 

Now, I'm working at company which has a very weak automation. 
It's really hard for me to understand why. 
This company has 3 system administrators (sysadmin further) including me, 
and automation could save a lot of time, especially infrastructure as a code (iac further). 

## A bit of background 

Company i'm working for is a small product company focused on implementation of on-premise
solutions with tight cisco integration. 
And now we have a lot of local stands for mostly every customer, with transfer to 
customers power in last stages of project. 
That stands have a lot common, starting with software installed, like an app servers, 
ending with app archives (war-s for now) placement. 
And this situation is just perfect for implementing iac with full code development life cycle,
like tests, code review, etc.

## Pros and Cons of implementation
Pros:
 - Lesser error quantity as consequence of implementing software development practices 
   like code review, usage of linters and automated tests presence
 - Possibility of fast rollback to last working state of infra as consequence of git usage
 - Ease of changing project DevOps engineer as consequence of joint code (infra) ownership 
 - Lesser documentation quantity, in cause of code self-docmentation
 - Infra reproduciblity in multiple environments
 - Faster infra creature in cause of code reuse on local and customer stand
 - IP addresses management (no more ip scanning to find free ip)
Cons:
 - Increased time to market in cause of tests running and code review process

## Local implementation proposal
Here is described step by step implementation of iac in our conditions, with our local peculiarities.
But i suppose that it could be helpful for other companies with similar development and
solution implementation process.

**Proposed IaC stack:**
- Ansible for software provisioning
- Terraform for VM provisioning
- vSphere as hypervisor, private cloud provider
- Git as VCS
- Github cloud as git server implementation
- Jenkins as orchestrator

**Used software stack:**
- Apache tomcat 8
- Redis
- RabbitMQ
- Nginx

**Local peculiarities:**
- No IP management with dhcp
- No automated VM images creature 
- Multiple stands with different software modules installed
- Solutions are on-premise
- Limited access to infrastructure on customer's side
- Limited internet access on a customer's side 

## Main rules 


## Github project types
The github projects/repos are divided to module/role project and customer project.

### Customer's project 
Local stands are also should be regarded as customers.
Customer's project should contain next folders with the below content:
```
repo
├── docs
├── tf
├── ansible
├── .gitignore
└── README.md
```
- docs directory, contains technical project specific documentation.  
- tf directory, contains tf module, all hidden terraform dirs should be added to gitigonore.  
- ansible directory, contains ansible playbook to setup similar stand, ansible.cfg in it specifies
  roles to be downloaded by path repo/ansible/roles which added to gitigonore.  

### Terraform module repo
Terraform module must describe only one VM with only one software set. And follow the template below. 
README.md describes the usage of tf module.
```
repo
├── tf
│   └── tffiles... 
├── .gitignore
└── README.md 
```

### Ansible role repo
Ansible role repo should follow the default ansible role folders layout.
README.md describes main vars and usage of ansible role.
```
repo
├── vars
├── defaults
├── files
├── handlers
├── meta
├── tasks
├── templates
├── tests
└── README.md
```

### Documentation repo
The documentation repo contains technical documentation written in markdown.
There are only one documentation repo, which contains the main conventions about code convetions, naming etc.
```
repo
├── files
├── docs
└── README.md
```

## Local staging stands iac pipeline
The local stands infra should be delivered to them with CI/CD pipeline described below.
![CI/CD infra pipeline](/IaCConfManage.svg)  
CI/CD infra pipeline  
The environment goes in accordance with git branch, e.g. if push was done to the dev branch
then configuration is applied to dev environment.  

## Customer's local stands iac
### Deployment
In the customer's local stands it's permissible to avoid pipeline creature if there is no ready template for it,
and it's rare case but it's not recommended. There are several variants of deployment to customer:
- Deployment with VM template at latest stages of project (before preprod exploitation). This variant is preferred.
- Deployment with ansible playbooks to customer's premaid environment. This variat requires more attention from sysadmin and 
  isn't recommended.

### Exploitation changes
This part should be written according to experience got in practice

## Conclusion
It's preface for the articles series about iac implementation for on-premise solutions.
By adding new articles table of context will be updated with new articles.



