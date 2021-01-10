# 3 node local kubernetes cluster by centos7


<br/>

### On Host (with linux)

<br/>

**kubectl installation:**

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

    $ cd latest

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

    $ mkdir -p ~/.kube

<!--

<br/>

    $ export KUBECONFIG=/etc/kubernetes/admin.conf

-->

<br/>

    // root password: kubeadmin
    $ scp root@master:/etc/kubernetes/admin.conf ~/.kube/config

<br/>

    $ kubectl version --short
    Client Version: v1.20.1
    Server Version: v1.20.1

<br/>

    $ kubectl get nodes
    NAME         STATUS   ROLES                  AGE     VERSION
    master.k8s   Ready    control-plane,master   8m48s   v1.20.1
    node1.k8s    Ready    <none>                 5m41s   v1.20.1
    node2.k8s    Ready    <none>                 2m24s   v1.20.1

<br/>

### Get Additional Inforamtion

<br/>

    $ kubectl get po -n kube-system
    NAME                                 READY   STATUS    RESTARTS   AGE
    coredns-74ff55c5b-czjk8              1/1     Running   0          9m11s
    coredns-74ff55c5b-jfg5w              1/1     Running   0          9m11s
    etcd-master.k8s                      1/1     Running   0          9m18s
    kube-apiserver-master.k8s            1/1     Running   0          9m18s
    kube-controller-manager-master.k8s   1/1     Running   0          9m18s
    kube-flannel-ds-amd64-6bvbh          1/1     Running   0          3m5s
    kube-flannel-ds-amd64-97v5g          1/1     Running   0          9m11s
    kube-flannel-ds-amd64-cgsns          1/1     Running   0          6m22s
    kube-proxy-j92xg                     1/1     Running   0          9m11s
    kube-proxy-kbjn6                     1/1     Running   0          3m5s
    kube-proxy-xl9mb                     1/1     Running   0          6m22s
    kube-scheduler-master.k8s            1/1     Running   0          9m18s


<br/>

    $ kubectl cluster-info
    Kubernetes control plane is running at https://192.168.0.10:6443
    KubeDNS is running at https://192.168.0.10:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

    To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.


<br/>


    $ kubectl get cs
    Warning: v1 ComponentStatus is deprecated in v1.19+
    NAME                 STATUS      MESSAGE                                                                                       ERROR
    scheduler            Unhealthy   Get "http://127.0.0.1:10251/healthz": dial tcp 127.0.0.1:10251: connect: connection refused   
    controller-manager   Unhealthy   Get "http://127.0.0.1:10252/healthz": dial tcp 127.0.0.1:10252: connect: connection refused   
    etcd-0               Healthy     {"health":"true"}                                                                             



<br/>
<br/>

**Original src:**  
https://github.com/justmeandopensource/kubernetes

<br/>

---

<br/>

**Marley**

Any questions in english: <a href="https://jsdev.org/chat/">Telegram Chat</a>  
Любые вопросы на русском: <a href="https://jsdev.ru/chat/">Телеграм чат</a>
