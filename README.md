# Infrastructure Automation Kubernetes Week 4

## Introduction

Introduction to Debugging kubernetes applications


## Objectives


In this guide we will learn how to debug kubernetes resources in various ways.

Althought the examples were simple, the tools used are very helpful in debugging more complicated applications.


---
## Getting Started:

1 - Copy the starter code from here into a new, private repository in your personal GitHub account. When adding a collaborator, be sure to add me ("cscc-luke-rouker").

2 - Make sure all resources are deleted
3 - Make sure you are using the dev context and namepace
4 - Have two terminals open. 
    - In the second terminal run the following command
        - `watch kubectl get all`

---

## Understanding the Starter Code
The debugging guide has been broken up into 5 parts
1. restart_pod
2. bad_image
3. memory_issue
4. app_issue
5. storage

Within each directory, there is a readme with the excerise. Follow the directions, and update (if required) the provided manifest in each directory under manifests. The general structure of each yml is provided.


---

## Completing the Assignment


### 1 - Go through examples

Go through each of the directories provided, and complete the tasks.

The general manifest structures have been provided for ease of use, you are required to update it according to the task.


### 2 - Cleanup

```
kubectl delete deployment -l app=redis
kubectl delete service -l app=redis
kubectl delete deployment -l app=guestbook
kubectl delete service -l app=guestbook
```
  

---


## Submitting Your Work

1-  Publish your repository as a private repo, and ensure that you have pushed the latest version

2-  Submit the assignment in Blackboard with the link to your repo

[pre-lab]: https://github.com/cscc-afarag/kubernetes-week-1/blob/master/ENV_SETUP.md
