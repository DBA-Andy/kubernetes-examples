# Working with Kubernetes Part - 2

### **Before you start**
You need minikube up and running before going through the examples.

Start minikube using the command:
```
$ minikube start
```

If you do not have this setup please go to the [pre-lab] and follow those instructions before continuing.

If you are coming from [lab 1] please make sure your minikube is cleaned, by deleting all contexts, resources, configs, pods, and namespaces made.

You can do it using the `kubectl delete <resource> <name>` command or go with the scorched earth method and run ...

```
$ minikube stop
$ minikube delete
$ minikube start
```
---
## create namespace and context
If you switched your context or deleted your namespace/context from the previous part please follow the below instructions

Create a new namespace - 

```
$ kubectcl create namespace dev
```

Lets make a new context within our minikube environment with our dev namespace and use the same minikube user.

```
$ kubectl config set-context kubedev --cluster=minikube --namespace=dev --user minikube
```

switch to this context.

```
$ kubectl config use-context kubedev
```


## Notes
You will be creating manifest files as we go through this walkthrough - 
**These will be turned in and graded**

**While working through this go through the lecture slides, and online resources**

---

# Kubernetes Workloads

In this guide we will go over Workloads, which are  the higher level kubernetes objects that manage pods or other higher level objects.

Make sure you have read the slides [TODO LINK] before going through this so you have a general understanding. 

- make sure to get a copy of `workload_tasks.md` from [TODO](todo) to fill out while you go through the tasks.

---

# ReplicaSet

### refresher
ReplicaSets allow a user to set a desired number of replicas of a given pod(s) that match a selector.

 The ReplicaSet handles the replica pod's lifecycle and ensures the number requested is upheld.

 
## Task 1 (Create a ReplicaSet)

Your first task is to create a `ReplicaSet` of the nginx application we made earlier.

1. Create a manifest file called `replica_1.yml`
2. In it create a resource of kind `ReplicaSet` and call it `nginx-replica`
3. Give it `2` replicas
4. For the template for the replica, use the nginx pod from before:
   - image : `nginx:stable-alpine` 
   - container name : nginx
   - labels: app_name = nginx and env = dev
   - port : 80
5. `apply` or `create` the resource
6. run `kubectl get rs` and view the results
   
7. Now, using [scale] command, scale up the replicas to 3. Save the command used in the `workload_tasks.md` file where appropriate

8. Now create the following pod and `apply` or `create` it:

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app_name: nginx
    env: dev
spec:
  containers:
  - name: nginx
    image: nginx:stable-alpine
    ports:
    - containerPort: 80

```

1. What happened? How many pods with our labels are there? Is there an event in the `ReplicaSet` that tells us? Write your findings in the `workload_tasks.md`
   - hint - `kubectl describe rs <name>` will shows events for the resource   


continue to part 3 [TODO link]



[pre-lab]: https://github.com/cscc-afarag/kubernetes-lab1/blob/master/pre-lab.md
[lab 1]: https://github.com/cscc-afarag/kubernetes-lab1/blob/master/lab1.md
[scale]: https://kubernetes.io/docs/reference/kubectl/cheatsheet/#scaling-resources