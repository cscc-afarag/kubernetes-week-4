
## Part 1  - Restarting Pod
- Go over the `manifests/bad.yml` and then apply it with the  `kubectl apply` command

- In the second terminal we are watching we can see it is failing to start. you should see something like

```
NAME                           READY   STATUS             RESTARTS   AGE
pod/bad-pod-58f7fc776b-fzvbj   0/1     CrashLoopBackOff   5          3m28s

```

- Lets find out whats wrong. first lets use describe on the deployment

```
$ kubectl describe deploy/bad-pod
```
Well see information about the deployment. Go through the output.

Lets get some more info using describe on the pod itself.

```
$ BADPOD=$(kubectl get po -l=app_name=myapp --output=jsonpath={.items[*].metadata.name})

$ kubectl describe po/$BADPOD

```
We should see something like this: 

```
...
    State:          Waiting
      Reason:       CrashLoopBackOff
    Last State:     Terminated
      Reason:       Completed
      Exit Code:    0
      Started:      Mon, 03 Aug 2020 21:43:31 -0400
      Finished:     Mon, 03 Aug 2020 21:43:31 -0400
    Ready:          False
    Restart Count:  6
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-6vs7n (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  default-token-6vs7n:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-6vs7n
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason     Age                   From               Message
  ----     ------     ----                  ----               -------
  Normal   Scheduled  8m12s                 default-scheduler  Successfully assigned dev/bad-pod-58f7fc776b-fzvbj to minikube
  Normal   Pulling    8m11s                 kubelet, minikube  Pulling image "centos:7"
  Normal   Pulled     8m5s                  kubelet, minikube  Successfully pulled image "centos:7"
  Normal   Created    6m37s (x5 over 8m5s)  kubelet, minikube  Created container shell
  Normal   Started    6m37s (x5 over 8m5s)  kubelet, minikube  Started container shell
  Normal   Pulled     6m37s (x4 over 8m4s)  kubelet, minikube  Container image "centos:7" already present on machine
  Warning  BackOff    3m7s (x25 over 8m3s)  kubelet, minikube  Back-off restarting failed container


```

Looks like the container is failing to restart. Lets get logs on the pod.

```
$ kubectl logs $BADPOD
$ kubectl exec -it $BADPOD -- sh
```

The output shows the echo statement, but then it is exiting. If we remember, a deployment ensures a number of pods. So kubernetes keeps restarting it, but the container is designed to print and exit!

Fix this by replacing the `echo` statement with  `tail -f /dev/null` 

Now apply the changes without deleting and starting it.

- NOTE - remember that `kubectl apply` will handle the differences if you edit a manifest created with `apply` and update it with `apply`

Once applied our container should be running and not restarting.


delete the deployment before moving on
```
kubectl delete -f bad.yml
```

---

Helpful links:


- [Kubernetes how to debug CrashLoopBackoff](https://stackoverflow.com/questions/44673957/kubernetes-how-to-debug-crashloopbackoff)
- [Kubernetes imagePullSecrets not working; getting “image not found”](https://stackoverflow.com/questions/32510310/kubernetes-imagepullsecrets-not-working-getting-image-not-found)
- [Trying to create a Kubernetes deployment but it shows 0 pods available](https://stackoverflow.com/questions/51139988/trying-to-create-a-kubernetes-deployment-but-it-shows-0-pods-available)
- [Kubernetes: five steps to well-behaved apps](https://medium.com/@betz.mark/kubernetes-five-steps-to-well-behaved-apps-a7cbeb99471a)

- [Troubleshoot Applications](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/)
- A site dedicated to [Kubernetes Troubleshooting](https://kubernetes.feisky.xyz/en/troubleshooting/) 
  
- CrashLoopBackoff, Pending, FailedMount and Friends: Debugging Common Kubernetes Cluster (KubeCon NA 2017): [video](https://www.youtube.com/watch?v=7FOCG5kua1w) and [slide deck](https://afontofuseless.info/debugging-kubernetes-app-deploys-kc2017/)
- 10 Most Common Reasons Kubernetes Deployments Fail: [Part 1](https://kukulinski.com/10-most-common-reasons-kubernetes-deployments-fail-part-1/) and [Part 2](https://kukulinski.com/10-most-common-reasons-kubernetes-deployments-fail-part-1/)

- [Debugging Microservices: How Google SREs Resolve Outages](https://www.infoq.com/presentations/google-debug-microservices)
- [Debugging Microservices: Lessons from Google, Facebook, Lyft](https://thenewstack.io/debugging-microservices-lessons-from-google-facebook-lyft/)
- [Troubleshooting Java applications on OpenShift](https://developers.redhat.com/blog/2017/08/16/troubleshooting-java-applications-on-openshift/)
- Google Kubernetes Engine [Troubleshooting](https://cloud.google.com/kubernetes-engine/docs/troubleshooting) docs

- [How to find out why mounting an emptyDir volume fails in Kubernetes?](https://stackoverflow.com/questions/51206154/how-to-find-out-why-mounting-an-emptydir-volume-fails-in-kubernetes)
- [Kubernetes NFS volume mount fail with exit status 32](https://stackoverflow.com/questions/34113569/kubernetes-nfs-volume-mount-fail-with-exit-status-32)

- [Debugging Kubernetes PVCs](https://itnext.io/debugging-kubernetes-pvcs-a150f5efbe95)
