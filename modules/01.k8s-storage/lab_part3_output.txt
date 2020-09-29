scast@MSI MINGW64 ~
$ minikube status
🤷  There is no local cluster named "minikube"
👉  To fix this, run: "minikube start"

scast@MSI MINGW64 ~
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

scast@MSI MINGW64 ~
$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured


scast@MSI MINGW64 ~
$ minikube ssh
                         _             _
            _         _ ( )           ( )
  ___ ___  (_)  ___  (_)| |/')  _   _ | |_      __
/' _ ` _ `\| |/' _ `\| || , <  ( ) ( )| '_`\  /'__`\
| ( ) ( ) || || ( ) || || |\`\ | (_) || |_) )(  ___/
(_) (_) (_)(_)(_) (_)(_)(_) (_)`\___/'(_,__/'`\____)

$ sudo mkdir /mnt/data
$ sudo sh -c "echo 'Hello from Kubernetes storage' > /mnt/data/index.html"
$ cat /mnt/data/index.html
Hello from Kubernetes storage
$ exit


scast@MSI MINGW64 ~/Documents/GitHub/dsti-devops-2020-fall-k8s/modules/01.k8s-storage/lab/PersistentVolume (master)

$ kubectl apply -f https://k8s.io/examples/pods/storage/pv-volume.yaml
persistentvolume/task-pv-volume created

$ kubectl get pv task-pv-volume
NAME             CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
task-pv-volume   10Gi       RWO            Retain           Available           manual                  33s

$ kubectl apply -f https://k8s.io/examples/pods/storage/pv-claim.yaml
persistentvolumeclaim/task-pv-claim created

$ kubectl get pv task-pv-volume
NAME             CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                   STORAGECLASS   REASON   AGE
task-pv-volume   10Gi       RWO            Retain           Bound    default/task-pv-claim   manual                  6m43s

$ kubectl get pvc task-pv-claim
NAME            STATUS   VOLUME           CAPACITY   ACCESS MODES   STORAGECLASS   AGE
task-pv-claim   Bound    task-pv-volume   10Gi       RWO            manual         3m48s

$ kubectl apply -f https://k8s.io/examples/pods/storage/pv-pod.yaml
pod/task-pv-pod created

$ kubectl get pod task-pv-pod
NAME          READY   STATUS    RESTARTS   AGE
task-pv-pod   1/1     Running   0          42s

$ kubectl exec -it task-pv-pod bash
root@task-pv-pod:/#  

root@task-pv-pod:/# apt update
Get:1 http://security.debian.org/debian-security buster/updates InRelease [65.4 kB]
Get:2 http://deb.debian.org/debian buster InRelease [121 kB]
Get:3 http://deb.debian.org/debian buster-updates InRelease [51.9 kB]
Get:4 http://security.debian.org/debian-security buster/updates/main amd64 Packages [234 kB]
Get:5 http://deb.debian.org/debian buster/main amd64 Packages [7906 kB]
Get:6 http://deb.debian.org/debian buster-updates/main amd64 Packages [7868 B]
Fetched 8387 kB in 2s (3919 kB/s)
Reading package lists... Done
Building dependency tree
Reading state information... Done
3 packages can be upgraded. Run 'apt list --upgradable' to see them.

#apt install curl

root@task-pv-pod:/# curl http://localhost/
Hello from Kubernetes storage

root@task-pv-pod:/# exit
exit
command terminated with exit code 127

scast@MSI MINGW64 ~/Documents/GitHub/dsti-devops-2020-fall-k8s/modules/01.k8s-storage/lab/PersistentVolume (master)

$ kubectl delete pod task-pv-pod
pod "task-pv-pod" deleted

$ kubectl delete pvc task-pv-claim
persistentvolumeclaim "task-pv-claim" deleted

$ kubectl delete pv task-pv-volume
persistentvolume "task-pv-volume" deleted

$ minikube ssh
                         _             _
            _         _ ( )           ( )
  ___ ___  (_)  ___  (_)| |/')  _   _ | |_      __
/' _ ` _ `\| |/' _ `\| || , <  ( ) ( )| '_`\  /'__`\
| ( ) ( ) || || ( ) || || |\`\ | (_) || |_) )(  ___/
(_) (_) (_)(_)(_) (_)(_)(_) (_)`\___/'(_,__/'`\____)

$ sudo rm /mnt/data/index.html
$ sudo rmdir /mnt/data
$
