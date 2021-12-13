# Kubernetes bootcame

### Prerequisites
    1. At least one kubernetes node running Ubuntu 20.04 LTS, updated fully (sudo apt update && sudo apt upgrade)
    2. An NFS server and export configured to use as storage for your Kubernetes podes

### Basic cluster setup


**Note: AWS calls some types of firewalls “security groups” because a firewall policy can be applied to multiple devices the same way multiple users can share the same group.**

1. Configure name resolution for so each cluster member can reach the others by edit hosts file to include name and IP of each cluster member
    
    ```nano /etc/hosts```


2) Configure name resolution for so each cluster member can reach the others by edit hosts file to include name and IP of each cluster member

   nano /etc/hosts 

3) Install microk8s on first cluster member (see https://microk8s.io/docs/getting-started):

   sudo snap install microk8s --classic

4) Add your user to the microk8s group so you dont have to sudo for everything:
   sudo usemod -a -G microk8s <YOUR USER NAME>
   sudo chown -f -R <YOUR USER NAME> ~/.kube
  
4) Add your user to the microk8s group

4) Enable helm (meta-package manager for Kubernetes).  Helm is like apt or yum, but for Kubernetes - it makes your life easier but its good to understand what is happening under the covers as well.
   microk8s enable helm

5) Create

5) Set up NFS storage on the cluster.  This will allow for pods (containers or groups of containers) to move between the nodes in your cluster seamlessly while keeping access to the same storage.  We will use helm for this:
   