
## Part 4 App Issue

Go over and create/apply `manifests/app_issue.yml`

Wait some time till it restarts.

Lets try describe:
```
kubectl describe deploy/oops
```
Unfortuntley its not showing us really why its restarting.

lets follow the logs of the pod
```
kubectl logs --follow $(kubectl get po -l=app=oops --output=jsonpath={.items[*].metadata.name})
```

Similar to the first example, we can see clearly that the application is exiting, and the deployment restarts it because it ensures that there is atleast 1 instance available.

Using the log command is helpful to target pods/containers and see whats going on in the app.

Delete the deployment
```
kubectl delete -f oops.yml 
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
