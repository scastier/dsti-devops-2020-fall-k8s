# Lab

## Before starting

Before you can start the lab, you have to:

1. Install [Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/) following the instructions depending on your OS.

2. Start Minikube with:

```
minikube start
```

3. Verify that everything is OK with:

```
minikube status
```

4. Clone the lab repository to your computer:

```
git clone https://github.com/adaltas/dsti-devops-2020-fall-k8s.git
cd dsti-devops-2020-fall-k8s/modules/01.k8s-storage
```

## 1. Use `emptyDir` storage

1. Complete the [`lab/emptyDir/deployment.yml`](lab/emptyDir/deployment.yml) file.

References:
- `emptyDir` usage - https://kubernetes.io/docs/concepts/storage/volumes/#emptydir
- Nginx Docker image usage - https://hub.docker.com/_/nginx

**Hint.** Nginx will start an HTTP web server and respond with a content of HTML files located in `/usr/share/nginx/html` directory.

2. Run a pod applying configuration:

```
kubectl apply -f lab/emptyDir/deployment.yml
```

3. Enter to a container and `curl` localhost

List all the pods and find a name of a created pod

```
kubectl get pods
```

Enter to the container:

```
kubectl exec -it <POD_NAME> bash
```

Run `curl localhost`

It will output the page of the 403 error, because there is no `index.html` file to respond with:

```
<html>
<head><title>403 Forbidden</title></head>
<body>
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx/1.19.2</center>
</body>
</html>
```

4. Create `index.html` file with some content **inside a container**

Being inside the container, run:
```
echo 'Hello from Kubernetes storage!' > /usr/share/nginx/html/index.html
```

Now, by running `curl localhost` it will output:

```
Hello from Kubernetes storage!
```

5. Verify

- When a **Pod is removed** from a node for any reason, the data in the `emptyDir` is deleted forever.   
  You can remove a pod using Kubernetes Dashboard that is started with `minikube dashboard` command or `kubectl delete pod/<POD_NAME>`.

- When **container in a Pod is removed**, Kubernetes will create a new container and will mount existing `emptyDir` volume to it.   
  You can enter to Minikube Node with `minikube ssh`, find the related container with `docker ps | grep <POD_NAME>` and kill it with `docker kill CONTAINER_ID`. 

## 2. Use `hostPath` storage

1. Complete the [`lab/hostPath/deployment.yml`](lab/hostPath/deployment.yml) file.

Use `/mnt/hostPath/` folder as path on your virtual node file system.

References:
- `hostPath` usage - https://kubernetes.io/docs/concepts/storage/volumes/#hostpath

2. Run a pod applying configuration:

```
kubectl apply -f lab/hostPath/deployment.yml
```

Ensure that `curl localhost` responds with 403 error (step 3 in the previous task).

3. Create `/mnt/hostPath/index.html` file with some content **inside a VM**

Enter to VM with `minikube ssh` command. Then run:

```
sudo mkdir /mnt/hostPath
sudo chmod -R 777 /mnt/hostPath
sudo echo 'Hello from Kubernetes storage!' > /mnt/hostPath/index.html
```

Make sure you have successfully written to file: `cat /mnt/hostPath/index.html`

From you host (not a VM) enter to a container and run `curl localhost`. It will output:

```
Hello from Kubernetes storage!
```

4. Verify

- When a **Pod is removed** from a node for any reason, the data in the `hostPath` still remain.
- When multiple replicas of a **Pod** are created, they all bind to the same volume.   
  You can configure a number of replicas using `spec.replicas` parameter in the deployment configuration yaml file.

## 3. Use PersistentVolume

Reference to this tutorial and reproduce all the steps - https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/

## Bonus tasks

1. Configure a Pod to use Secret - https://kubernetes.io/docs/concepts/configuration/secret/
