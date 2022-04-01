# Install of an Openshift cluster through PowerVC

## Check the environment 
* Activate openvpn
* To check it is working properly, make sure you can access the PowerVC : <https://powervc.paris.edu.ihost.com:4443> 
* The manager's ip (that hosts the terraform) is 10.3.48.3   
to access it :
>ssh you_provided_user@10.3.48.3 \
>password : your_provided_password





The terraform is on the manager's vm (distinct from the VM on which the PowerVC runs).
It takes the var.tfvars file in input and will generate a tfstate file.


The ansible playbooks (1) and (2) will be on the bastion once it is deployed. They will run to automatically create the cluster as stated in the tfstate file. 



![clipboard](https://i.imgur.com/TpxKcMx.png)

The ansible helpernode playbook will prepare the bastion for the cluster and will set its environment.
The other ansible playbook will be generated next and will set the nodes to create the cluster. 

![image](https://user-images.githubusercontent.com/91323551/145597040-b9863539-52e8-4e45-b766-b400de1d39e7.png)









## 1) Cluster installation 

Now, we can clone the git repository.

```
git clone https://github.com/ocp-power-automation/ocp4-upi-powervm
```

```
Cloning into 'ocp4-upi-powervm'...
remote: Enumerating objects: 1732, done.
remote: Counting objects: 100% (448/448), done.
remote: Compressing objects: 100% (186/186), done.
remote: Total 1732 (delta 284), reused 398 (delta 262), pack-reused 1284
Receiving objects: 100% (1732/1732), 424.05 KiB | 2.08 MiB/s, done.
Resolving deltas: 100% (1038/1038), done.
```

```
ls
ocp4-upi-powervm 
```

We now have to work in the ocp4-upi-powervm directory
```
cd ocp4-upi-powervm
```
Copy the customized tfvar sample

```
cp /home/samples/sample_demo.tfvars ~/ocp4-upi-powervm/paris.tfvars

```
Initialize terraform
```
terraform init -plugin-dir /usr/local/terraform/
```

Then, in the data directory, we need to copy the SSH key-pair and the pull-secret.txt.
Your personnal Openshift pull secret, is available at :

[<https://cloud.redhat.com/openshift/install/power/user-provisioned > ](https://console.redhat.com/openshift/install/power/user-provisioned)
![clipboard](https://i.imgur.com/0x33IoI.png)

Once copied, it must be pasted in the data/pull-secret.txt file.

```
vi ocp4-upi-powervm/data/pull-secret.txt
```

Create an ssh key, and copy it into the data directory

```
ssh-keygen -q -t rsa -N '' -f ~/.ssh/id_rsa
cp ~/.ssh/id_rsa* ocp4-upi-powervm/data/
```
Now, we can edit the paris.tfvars file in the ocp4-upi-powervm repository.


Once done:

>[radardemo@radardemomgt ocp4-upi-powervm]$ terraform apply -var-file paris.tfvars

The terraform will create the VMs as specified in the demo.tfvars file.

## 2) Monitoring the installation

### a) command lines on the bastion
Once the apply command is launched, we need to wait for the bastion to be fully deployed. 

One can see the progress of the install of the bastion on PowerVC. For that, see Virtual Machines -> VM list. 
![image](https://user-images.githubusercontent.com/91323551/145581536-010e7e17-5353-4172-9b04-65f80fdc4a81.png)


The ip adress of the bastion is specified in the paris.tfvars file, as well as in the powervc (vm list -> network). From the manager's vm, one can access the bastion through ssh once it is properly deployed.
```
ssh root@10.3.48.100
```

>:warning: The openstack repository might take some time to be created (the helpernode and then the playbooks repo should be created first).

Wait for the openshift-install command is available :

```
which openshift-install
```
```
/usr/local/bin/openshift-install
```
Check that the openstack-upi directory as been created :

```
ls -ld ~/openstack-upi/
```
Add the bash completion for both the openshift-install and oc commands :

```
oc completion bash > /etc/bash_completion.d/oc
openshift-install completion bash > /etc/bash_completion.d/openshift-install

source /etc/bash_completion.d/oc
source /etc/bash_completion.d/openshift-install
```

- To follow the installion progress of the bootstrat, once in the openstack repository :
```
cd ~/openstack-upi/
openshift-install wait-for bootstrap-complete --log-level debug
```
- To follow the step of the install :
```
~/openstack-upi/
openshift-install wait-for install-complete --log-level debug
```
- Check the installation :

```
export KUBECONFIG=~/openstack-upi/auth/kubeconfig
watch -n 15 "oc get clusterversions ; echo ; oc get co"
```



### b) through the bastion, using graphics

HA proxy to follow the installation, once the bastion is deployed.
<http://10.3.48.100:9000/>

* At first, all the other machines will still be red, because not deployed yet.
![clipboard](https://i.imgur.com/OvZgX81.png)

* Then, the bootstrap will be deployed next. It will compose a mini cluster on its own.
![clipboard](https://i.imgur.com/YdQWw2s.png)

* After the bootstrap come the masters that will join it in the cluster.
![clipboard](https://i.imgur.com/Sk14RCI.png)

* Then come the workers. Once the masters are all deployed, the bootstrap will be destroyed since it is no longer userful.
![clipboard](https://i.imgur.com/hRcBZ4s.png)



## 3) Check that you can access your new cluster

To get the password to access the cluster, type on the bastion


>cat openstack-upi/auth/kubeadmin-password

<https://console-openshift-console.apps.paris.edu.ihost.com/>

>username : kubeadmin \
>password : in openstack-upi/auth/kubeadmin-password

![clipboard](https://i.imgur.com/1gldP6p.png)


## 4) Cleaning the environment

### a) Delete the current cluster (if needed)
The user radardemo has only a limited range of ip adresses to create their cluster. Therefore, if one cluster already exists, it is better to delete it to free these ips.


On the manager's vm, in /home/radardemo:
````
[radardemo@radardemomgt ~]$ ll
total 20
-rw-rw-r--. 1 radardemo radardemo 5369 Oct 19 12:42 demo.tfvars
drwxrwxr-x. 7 radardemo radardemo 4096 Oct 19 12:22 ocp4-upi-powervm
-rw-rw-r--. 1 radardemo radardemo  156 Oct 19 14:55 terraform.tfstate
-rw-r--r--. 1 radardemo radardemo 1842 Oct 12 17:35 vars_paris.yaml
[radardemo@radardemomgt ~]$ cd ocp4-upi-powervm
[radardemo@radardemomgt ocp4-upi-powervm]$$ terraform destroy -var-file=demo.tfvars
```````

The destroy command must be done inside the right repository (the demo.tfvars will then be found.)
>:warning: Once the destroy command is launched, make sure to wait for the deletion of all the vms of the cluster before moving on to the next step.

![clipboard](https://i.imgur.com/ifR8fth.png)


### b) Remove the old bastion from known_hosts

>[radardemo@radardemomgt ocp4-upi-powervm]$ vi ~/.ssh/known_hosts

and delete the line with the bastion's IP.

### c) Remove the git repository

```````
[radardemo@radardemomgt ~]$ rm -rf ocp4-upi-powervm/
```````





























