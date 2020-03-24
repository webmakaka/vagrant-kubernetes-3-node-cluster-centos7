# 3 node local kubernetes cluster by centos7


<br/>

### On Host (with linux)

<br/>

**Cubectl installation:**

```
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```

<br/>

    $ sudo vi /etc/hosts

```
#---------------------------------------------------------------------
# Kubernetes cluster
#---------------------------------------------------------------------

192.168.0.10 master.k8s master
192.168.0.11 node1.k8s node1
192.168.0.12 node2.k8s node2
```

<br/>

    $ vagrant plugin install vagrant-hostmanager

<br/>

    $ mkdir ~/vagrant-kubernetes && cd ~/vagrant-kubernetes

    $ git clone https://github.com/webmakaka/vagrant-kubernetes-3-node-cluster-centos7 .

    $ cd ~/vagrant-kubernetes-3-node-cluster-centos7/latest

    $ vagrant box update

    $ vagrant up

    $ vagrant status
    Current machine states:

    master.k8s                running (virtualbox)
    node1.k8s                 running (virtualbox)
    node2.k8s                 running (virtualbox)


<br/>

### Copy kubernetes config for manage kubernetes cluster remotely

<br/>

    $ mkdir -p ~/.kube/config

<br/>

    // root password: kubeadmin
    $ scp root@master:/etc/kubernetes/admin.conf ~/.kube/config

<br/>

    $ kubectl version --short
    Client Version: v1.17.4
    Server Version: v1.17.4

<br/>

    $ kubectl get nodes
    NAME         STATUS   ROLES    AGE     VERSION
    master.k8s   Ready    master   8m14s   v1.17.4
    node1.k8s    Ready    <none>   5m      v1.17.4
    node2.k8s    Ready    <none>   102s    v1.17.4


<br/>

### Get Additional Inforamtion

    $  kubectl get cs
    NAME                 STATUS    MESSAGE             ERROR
    scheduler            Healthy   ok                  
    controller-manager   Healthy   ok                  
    etcd-0               Healthy   {"health":"true"}   


<br/>

    $ kubectl get po -n kube-system
    NAME                                 READY   STATUS    RESTARTS   AGE
    coredns-6955765f44-c754z             1/1     Running   0          8m46s
    coredns-6955765f44-zhp4f             1/1     Running   0          8m46s
    etcd-master.k8s                      1/1     Running   0          8m39s
    kube-apiserver-master.k8s            1/1     Running   0          8m39s
    kube-controller-manager-master.k8s   1/1     Running   0          8m39s
    kube-flannel-ds-amd64-cxrs5          1/1     Running   0          8m46s
    kube-flannel-ds-amd64-dppg8          1/1     Running   0          5m50s
    kube-flannel-ds-amd64-fjfcg          1/1     Running   0          2m32s
    kube-proxy-6j657                     1/1     Running   0          5m50s
    kube-proxy-d2kf2                     1/1     Running   0          8m46s
    kube-proxy-zg52q                     1/1     Running   0          2m32s
    kube-scheduler-master.k8s            1/1     Running   0          8m39s


<br/>

    $ kubectl cluster-info
    Kubernetes master is running at https://192.168.0.10:6443
    KubeDNS is running at https://192.168.0.10:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy



<br/>
<br/>

**Original src:**  
https://github.com/justmeandopensource/kubernetes


---

**Marley**

<a href="https://jsdev.org">jsdev.org</a>

Any questions on eng: <a href="https://jsdev.org/chat/">Telegram or Slack</a>  
Любые вопросы на русском: <a href="https://jsdev.ru/chat/">Telegram or Slack</a>

