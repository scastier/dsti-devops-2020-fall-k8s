scast@MSI MINGW64 ~/Documents/GitHub/dsti-devops-2020-fall-k8s/modules/01.k8s-storage (master)
$ minikube start
😄  minikube v1.13.0 sur Microsoft Windows 10 Pro 10.0.18363 Build 18363
✨  Choix automatique du pilote hyperv
👍  Démarrage du noeud de plan de contrôle minikube dans le cluster minikube
🔥  Création de VM hyperv (CPUs=2, Mémoire=4000MB, Disque=20000MB)...
🐳  Préparation de Kubernetes v1.19.0 sur Docker 19.03.12...
🔎  Verifying Kubernetes components...
🌟  Enabled addons: default-storageclass, storage-provisioner

❗  C:\Program Files\Docker\Docker\resources\bin\kubectl.exe is version 1.16.6-beta.0, which may have incompatibilites with Kubernetes 1.19.0.
💡  Want kubectl v1.19.0? Try 'minikube kubectl -- get pods -A'
🏄  Done! kubectl is now configured to use "minikube" by default

scast@MSI MINGW64 ~/Documents/GitHub/dsti-devops-2020-fall-k8s/modules/01.k8s-storage (master)
$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured


scast@MSI MINGW64 ~/Documents/GitHub/dsti-devops-2020-fall-k8s/modules/01.k8s-storage (master)
$ kubectl apply -f lab/emptyDir/deployment.yml
deployment.apps/nginx-empty-dir created

scast@MSI MINGW64 ~/Documents/GitHub/dsti-devops-2020-fall-k8s/modules/01.k8s-storage (master)
$ kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
nginx-empty-dir-76fcbdcd97-n7h7p   1/1     Running   0          8s

scast@MSI MINGW64 ~/Documents/GitHub/dsti-devops-2020-fall-k8s/modules/01.k8s-storage (master)
$ kubectl exec -it nginx-empty-dir-76fcbdcd97-n7h7p bash
root@nginx-empty-dir-76fcbdcd97-n7h7p:/# curl localhost
<html>
<head><title>403 Forbidden</title></head>
<body>
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx/1.19.2</center>
</body>
</html>
root@nginx-empty-dir-76fcbdcd97-n7h7p:/# echo 'Hello from Kubernetes storage!' > /usr/share/nginx/html/index.html
root@nginx-empty-dir-76fcbdcd97-n7h7p:/# curl localhost
Hello from Kubernetes storage!
root@nginx-empty-dir-76fcbdcd97-n7h7p:/#
