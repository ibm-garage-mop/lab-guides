# Parameters in the .tfvars file


## 1) PowerVC details

````
auth_url                    = "<https://<HOSTNAME>:5000/v3/>" #hostname : 10.3.47.2
user_name                   = "<powervc-login-user-name>" #radardemo
password                    = "<powervc-login-user-password>" #demoradar
tenant_name                 = "<tenant_name>" #radardemo
domain_name                 = "Default"

network_name                = "RadarDemo"
````

## 2) Openshift Cluster Details

In this part, the information must be found in PowerVC

````
bastion                     = {instance_type    = "bastion",   image_id    = "d6e7192e-79af-498c-8630-93f0e595d553", fixed_ip_v4 = "10.3.48.100"}`
````
* The instance_type must be picked in virtual machine -> compute templates, it gives the provisionning of the VM.

![clipboard](https://i.imgur.com/eUtwelA.png)

* The image_id must be picked in Images -> Image list. Except for the bastion, the OS of the VMs must be CoreOS.

![clipboard](https://i.imgur.com/cu0cDhb.png)
![clipboard](https://i.imgur.com/nYVguBV.png)

* The IP must be picked in the allowed range.

![clipboard](https://i.imgur.com/vDUexXb.png)
![clipboard](https://i.imgur.com/dxbdY2H.png)

Here, we can see that the allowed IP's are 10.3.48.100 and 10.3.48.10 to 10.3.48.19.

An example for the masters :

````
master                      = {instance_type    = "master",    image_id    = "8d20f462-260f-4715-9b95-6a63aad698e3",  "count"   = 3, fixed_ips = ["10.3.48.10", "10.3.48.11", "10.3.48.12"]}
``````

The image_id is different from the bastion since it must be a coreOS. There is also an extra parameter 'count' to create 3 masters.



Link to the full set of tfvars parameters: [var.tfvars-doc.md](https://github.com/ocp-power-automation/ocp4-upi-powervm/blob/master/docs/var.tfvars-doc.md)







    



