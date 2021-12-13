# Kubernetes bootcamp

### Prerequisites

1. At least one kubernetes node running Ubuntu 20.04 LTS, updated fully (sudo apt update && sudo apt upgrade)
2. An NFS server and export configured to use as storage for your Kubernetes podes, preferably on a seperate machine.  There are many guides on the internet for this, including this one: https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-ubuntu-20-04

### Basic node setup

***On ONE of your cluster members, perform the following:***

1. Configure name resolution for so each cluster member can reach the others by edit hosts file to include name and IP of each cluster member
    
    ```nano /etc/hosts```

2. Install microk8s on first cluster member (see ):

    ```sudo snap install microk8s --classic```

3. Add your user to the microk8s group so you dont have to sudo for everything and take ownership of the kubectl configuration file:

    ```sudo usermod -a -G microk8s <YOUR USER NAME>```
    ```sudo chown -f -R <YOUR USER NAME> ~/.kube```

4. Make your life easier by aliasing the "microk8s kubectl" and "microk8s helm3" commands to the more standard "kubectl" and "helm":

    ```nano .bashrc```

    Add ```kubectl='microk8s kubectl'``` and ```helm='microk8s helm3'``` to the bottom of the file

5. Logout or disconnect your SSH session and log back in to load the aliases and group membership you just configured.

 ***See https://microk8s.io/docs/getting-started for more information***

### Add additional cluster members (optional)

1. Repeat the basic node setup steps on the node you wish to join to the cluster

2. On the first node you configured (we will call this the "master node"), enter the following:

    ```microk8s add-node```

    Copy the command/output provided

3. Run the command from the above step on the node you wish to join to the cluster

***Note: You can repeat these steps for as many nodes as you would like to join***

### Enable helm (meta-package manager for Kubernetes)

***Helm is like apt or yum, but for Kubernetes - it makes your life easier but its good to understand what is happening under the covers as well.***
   
1. From the master node, enable helm:

    ```microk8s enable helm3```

### Configure the NFS storage provider

***This will allow for pods (containers or groups of containers) to move between the nodes in your cluster seamlessly while keeping access to the same storage.***

1. Use helm to easily configure the NFS storage and set it as the default for pods to use:
   
    ```
    $ helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/
    $ helm install nfs-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
        --set nfs.server=<YOUR NFS SERVER IP> \
        --set nfs.path=/exported/path
        --set storageClass.defaultClass=true
    ```
   