# Storage in Kubernetes

## Volumes in Docker

- is simply a directory on disk or in another Container
- lifetimes are not managed

## Kubernetes Volumes

- has an explicit lifetime - the same as the Pod that encloses it
- a volume outlives any Containers that run within the Pod
- when a Pod ceases to exist, the volume will cease to exist, too. 
- Kubernetes supports many types of volumes, and a Pod can use any number of them simultaneously.

[Read more](https://kubernetes.io/docs/concepts/storage/volumes/)

## Kubernetes Volume types

- [`hostPath`](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath)
- ...

[**Ephemeral Volumes**](https://kubernetes.io/docs/concepts/storage/ephemeral-volumes/):

- [`emptyDir`](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir) - empty at Pod startup, with storage coming locally from the kubelet base directory (usually the root disk) or RAM
- [`configMap`](https://kubernetes.io/docs/concepts/storage/volumes/#configmap), [`downwardAPI`](https://kubernetes.io/docs/concepts/storage/volumes/#downwardapi), [`secret`](https://kubernetes.io/docs/concepts/storage/volumes/#secret) - inject different kinds of Kubernetes data into a Pod
- ...

## Persistent Volumes

[****](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

Provides an API for users and administrators that abstracts details of how storage is provided from how it is consumed.

2 parts:

- **PersistentVolume** - a piece of storage in the cluster that has been **provisioned by an administrator**
- **PersistentVolumeClaim** - a request for storage **by a user**
