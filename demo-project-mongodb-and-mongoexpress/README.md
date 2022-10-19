# Overview

![screenshot](https://user-images.githubusercontent.com/69618840/188682066-52b7d4ca-128c-45da-815a-bd81a7a88ef2.png)

# How to set up demo project

1. Create and start kubernetes cluster by Minikube

```
$ minikube start --driver=docker
😄  minikube v1.26.1 on Darwin 12.5.1 (arm64)
✨  Using the docker driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
🔄  Restarting existing docker container for "minikube" ...
🐳  Preparing Kubernetes v1.24.3 on Docker 20.10.17 ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```
2. Check the status of nodes

```
$ kubectl get nodes
NAME       STATUS   ROLES           AGE    VERSION
minikube   Ready    control-plane   2d6h   v1.24.3
```

3. Create resources

```
$ kubectl apply -f mongo-secret.yaml
$ kubectl apply -f mongo.yaml
$ kubectl apply -f mongo-configmap.yaml
$ kubectl apply -f mongo-express.yaml
```

4. Check if mongo-express-service and mongodb-service are created

NOTE: EXTERNAL-IP part for mongo-express-service stays pending because in minikube it works a little bit differently in a regular kubernetes setup

```
$ kubectl get service
NAME                    TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
kubernetes              ClusterIP      10.96.0.1        <none>        443/TCP          13m
mongo-express-service   LoadBalancer   10.109.61.104    <pending>     8081:30000/TCP   62s
mongodb-service         ClusterIP      10.100.164.251   <none>        27017/TCP        2m25s
```

5. Assign the EXTERNAL-IP and open browser by `minikube service` command

```
$ minikube service mongo-express-service
|-----------|-----------------------|-------------|---------------------------|
| NAMESPACE |         NAME          | TARGET PORT |            URL            |
|-----------|-----------------------|-------------|---------------------------|
| default   | mongo-express-service |        8081 | http://192.168.49.2:30000 |
|-----------|-----------------------|-------------|---------------------------|
🏃  Starting tunnel for service mongo-express-service.
|-----------|-----------------------|-------------|------------------------|
| NAMESPACE |         NAME          | TARGET PORT |          URL           |
|-----------|-----------------------|-------------|------------------------|
| default   | mongo-express-service |             | http://127.0.0.1:54678 |
|-----------|-----------------------|-------------|------------------------|
🎉  Opening service default/mongo-express-service in default browser...
❗  Because you are using a Docker driver on darwin, the terminal needs to be open to run it.
```

6. Cleanup

```
$ kubectl delete all --all
$ minikube stop
```
