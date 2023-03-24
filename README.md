# Private-Cloud-Openstack-RDO

![WhatsApp Image 2023-03-12 at 3 55 18 PM](https://user-images.githubusercontent.com/59062315/227630679-fdfa5d78-9e72-43e6-93a1-e668bee3b4b4.jpeg)

## A Project on how to build a private cloud using Openstack RDO.
OpenStack is a free, open standard cloud computing platform. It is mostly deployed as infrastructure-as-a-service in both public and private clouds where virtual servers and other resources are made available to users and in this project we will build Our Platform which includes full network topology and instances.
You can know more about openstack through the following link - https://docs.openstack.org/yoga/

## 1. Set Up Your Environment

In this project we used CentOS Stream 8-streamn, a single VM of MINIMUM 12 GB RAM, 2 CPUs and 2 NICs
FQDN of machine openstack.lab.local, so we should edit /etc/hosts file.

## 2. Machine preparation :
- Enabling nested virtualization
<img width="533" alt="image" src="https://user-images.githubusercontent.com/59062315/227531020-788b19c2-594a-4769-afef-a28396233404.png">


## 3. Operating System Installation :
During the installation of the CentOS OS. Move to Install CentOS Stream 8-stream and click tab on your keyboard.

<img width="420" alt="image" src="https://user-images.githubusercontent.com/59062315/227531143-07199d06-7b9f-48b2-96c7-175b84c2fc95.png">

Type a space to separate than the last kernel option, type [ net.ifnames=0 biosdevname=0 ]

<img width="468" alt="image" src="https://user-images.githubusercontent.com/59062315/227531309-65989616-686c-421b-a66c-af21b70ba2c7.png">

This will enable old Linux NIC card naming (eth0, eth1, …etc)

Now you see the installation screen, 
Click on Network & Host.

<img width="485" alt="image" src="https://user-images.githubusercontent.com/59062315/227538825-4128539a-d72d-4233-9364-0f6f28c9153c.png">

First step we need to set the Host Name,

<img width="476" alt="image" src="https://user-images.githubusercontent.com/59062315/227539047-5b0d9398-c2d4-4c54-b2fb-54c64ca516d9.png">



## 4. Prerequisites Deployment:

As a prerequisite, you have to install the needed repos and Adding to that, you must disable some services and enable the legacy network service.
The below section illustrates the actions we have to follow:
Firstly, we need to check the hostname, and check if we can reach the internet or not, and switch to be root.


4.1- Add the hostname to /etc/hosts and make an alias for it.

<img width="438" alt="image" src="https://user-images.githubusercontent.com/59062315/227540100-0ce683ab-db60-4dfb-8e84-21c925067589.png">


4.2- Disable and stop firewalld:

```
 systemctl disable firewalld 
 ```
 ```
 systemctl stop firewalld 
 ```
 ```
 systemctl status firewalld
 ```

<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227540211-bab0ac47-bdb6-4282-9a56-8679026a5185.png">


4.3- Disable SeLinux:

```
setenforce 0
```
```
sed -i 's/SELINUX=.*/SELINUX=disabled/' /etc/selinux/config
```

<img width="466" alt="image" src="https://user-images.githubusercontent.com/59062315/227540273-8d7949a5-bb8b-4214-9d96-9d6b9b03aebb.png">

<img width="437" alt="image" src="https://user-images.githubusercontent.com/59062315/227540297-69ef247a-fabd-4a63-9515-7e0778654dcf.png">


4.4- Disable and stop Networkmanager
```
systemctl disable NetworkManager
  ```
 ```
 systemctl stop NetworkManager 
  ```  
 ```
 systemctl status NetworkManager
  ```

<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227540344-1ca8126e-ed00-4ccf-85e6-3f52c6535cf1.png">


4.5- Download network-scripts:

```
dnf install network-scripts -y
```

<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227540476-69c8fb70-e4d1-4ff2-b7f0-0ba82ddc1cc7.png">

4.6- Enable and Start network-scripts:
```
systemctl enable network
```
```
systemctl start network 
```
```
systemctl status network
```

<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227540752-5abeaa22-71ff-49d5-9c83-9fb70346d424.png">

4.7- Enable power tools and install yoga:

```
dnf config-manager --enable powertools 
dnf install -y centos-release-openstack-yoga
```

<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227542844-b2893efc-7ea2-4546-ad77-ca3ed50e84ea.png">

4.8- Update the system:

```
 dnf -y update  
 ```

4.9- Install Packstack:

```
dnf install -y openstack-packstack
```

<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227545656-425d856d-8929-4f2f-8a40-be9456700086.png">

4.10- Generate answer file:

```
packstack --gen-answer-file=/root/answers.txt --os-neutron-l2-agent=openvswitch 
--os-neutron-ml2-mechanism-drivers=openvswitch --os-neutron-ml2-tenant-network-types=vxlan --os-neutron-ml2-type-drivers=vxlan,flat 
--provision-demo=n --os-neutron-ovs-bridge-mappings=extnet:br-ex --os-neutron-ovs-bridge-interfaces=br-ex:eth0 
--keystone-admin-passwd=redhat --os-heat-install=n
```
<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227546314-fe3ddde5-db80-47a1-b70d-5b0ac04f7852.png">

- To check the answer file:

<img width="431" alt="image" src="https://user-images.githubusercontent.com/59062315/227546854-6da7ba1c-54aa-428b-8ed2-ffa0c168c406.png">


4.11- And finally let's start the installation:

```
packstack --answer-file=/root/answers.txt -t 3600 
```

<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227548237-23b54f60-e7dc-4fe8-a644-818dc341accb.png">

![image](https://user-images.githubusercontent.com/59062315/227548351-9dbddefa-71a5-4051-bf05-492adba1bd69.png)

## 5. Validation

![image](https://user-images.githubusercontent.com/59062315/227549761-3461e116-ef89-4199-9b34-a30c93499109.png)

![image](https://user-images.githubusercontent.com/59062315/227549809-be983d2f-fddf-4735-bc2b-9bcd76f171a8.png)


- NOW WE CAN EXPLORE OPENSTACK DETAILS: 

Heading to => dmin => System => System Information

- Each service has 3 Endpoints, “Admin, Internal, Public”:

![image](https://user-images.githubusercontent.com/59062315/227552962-5b7cd1b1-9354-40ca-96bf-52e1a15bb69b.png)

- Compute Component:

![image](https://user-images.githubusercontent.com/59062315/227553197-ea1f5199-b3f8-4f23-a9da-fffc853f60c8.png)

- Block Storage Services:

![image](https://user-images.githubusercontent.com/59062315/227553347-68593133-d075-4e3b-a936-a44cc07b6c5d.png)

- Network Agents :

![image](https://user-images.githubusercontent.com/59062315/227553478-a6c3839b-fb9b-45ab-8b5d-94be76f8ee18.png)


## 6. Build Our Platform (NETWORK)

Firstly, we need you to be root and source the keystonerc_admin file to run the openstack commands:

<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227559131-c7a925c8-cea4-4152-bffc-c33cfefaadcc.png">

6.1- Create a Private Network:

```
neutron net-create Internal
```

<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227559640-69aac6eb-6ba0-4e4c-b38f-c96c4848353d.png">

- Results :

<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227559759-4dd48279-8201-46a2-84fe-2d330184e780.png">

6.2- Create a Private Subnet:

```
openstack subnet create --network Internal --subnet-range 192.168.10.0/24 --dhcp Internal_subnet
```
<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227560055-ddcce75b-3cc7-49c6-a025-9e6f48038c39.png">

- Results :

<img width="527" alt="image" src="https://user-images.githubusercontent.com/59062315/227560193-75e20938-8947-4aaa-88b5-d50aa097f9bd.png">

6.3- Create an External Network:

```
openstack network create External --provider-network-type flat --provider-physical-network extnet --external
```

<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227561257-5b50ce0b-f606-4b80-b86d-a3908d3bac5e.png">

- Results :

<img width="408" alt="image" src="https://user-images.githubusercontent.com/59062315/227561365-025d206a-a85c-4df7-8dfc-f0c2ac517b44.png">

6.4- Create an External Subnet:

```
openstack subnet create External_subnet --no-dhcp --allocation-pool start=192.168.120.160,end=192.168.120.170 --gateway 192.168.120.2 --network External --subnet-range 192.168.120.0/24
```

<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227561776-e4a2bf60-0bfb-4d58-85fa-e2222a338215.png">

- Results :

<img width="492" alt="image" src="https://user-images.githubusercontent.com/59062315/227561845-64175dce-bdc0-4b03-a34f-9b763572c567.png">



6.5- Create a Router: 
To connect the external network and the internal network to each other.

```
neutron router-create router
```

<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227562507-51695902-faa2-43fc-a46b-313907468637.png">

- Results :

<img width="459" alt="image" src="https://user-images.githubusercontent.com/59062315/227562531-cd006157-39e1-435a-9f9d-1292b91de414.png">

6.6- Set Router Gateway, Using External Network:

```
neutron router-gateway-set router External
```

<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227562722-df135d9b-c5d6-47f1-b61d-fe24119b1346.png">

- Results :

<img width="347" alt="image" src="https://user-images.githubusercontent.com/59062315/227562757-c9abd69b-e733-4a43-af00-83680ef1c672.png">

6.7- Set Router Gateway, Using Internal Network:

```
neutron router-interface-add router Internal_subnet
```

<img width="540" alt="image" src="https://user-images.githubusercontent.com/59062315/227562948-7e03b88b-5e07-45ff-b5fd-5a9b6ec0c388.png">

- Results :

<img width="393" alt="image" src="https://user-images.githubusercontent.com/59062315/227562993-49b295bf-6475-41de-8c05-b7da19d430cb.png">

- FULL TOPOLOGY RESULTS 

<img width="532" alt="image" src="https://user-images.githubusercontent.com/59062315/227563108-5da18207-3ab0-4f19-a299-2ceeb5e59eb2.png">


## 7. Build Our Platform (INSTANCE)

7.1- Create Instance: 

```
openstack server create --image 'cirros image' --flavor m1.tiny --network Internal server1
```
![image](https://user-images.githubusercontent.com/59062315/227564029-020a9103-c365-423f-81c0-48de3ba6e72e.png)

Results :

![image](https://user-images.githubusercontent.com/59062315/227564060-052fb08c-7bed-4f45-849c-01fa52a7934f.png)

7.2- Create FloatingIP, to reach our instance from the Internet.

```
openstack floating ip create External
```

<img width="519" alt="image" src="https://user-images.githubusercontent.com/59062315/227564184-e2576db9-77f5-4c14-a793-07fb69f14f47.png">

7.3- Assign the FloatingIP to the Instance_Port.
![image](https://user-images.githubusercontent.com/59062315/227564357-49e3f920-6031-495c-8dda-e30bb1307811.png)
```
openstack port list
```
![image](https://user-images.githubusercontent.com/59062315/227564604-9e489c21-a7ac-4221-9876-1d9042f6b0be.png)

```
openstack floating ip set --port 967bac43-0eca-4ba2-be64-dfe0a86893d5 192.168.120.162
```
![image](https://user-images.githubusercontent.com/59062315/227564726-3c533e17-f203-46b3-b373-82cf82ffadd9.png)

Results :

![image](https://user-images.githubusercontent.com/59062315/227564857-6c031e3f-dd1e-4cba-827d-5bc63d048be4.png)


7.4- Add two rules in the default security group to enable ssh port and ICMP
Project => Network => Security Groups => Manage Rule

![image](https://user-images.githubusercontent.com/59062315/227565949-d2d196b1-8302-46d7-b159-7535cf2b6164.png)

![image](https://user-images.githubusercontent.com/59062315/227565992-38374690-23d6-445c-801c-1022dc63db41.png)

![image](https://user-images.githubusercontent.com/59062315/227566009-de8c735d-82b4-4bc2-be63-cffb50d070e2.png)


![image](https://user-images.githubusercontent.com/59062315/227566065-0ac106fb-bfe2-4ecd-bded-cccbe84dae9b.png)

7.5- Connect to your instance by SSH:

```
ssh cirros@192.168.120.162
```
![image](https://user-images.githubusercontent.com/59062315/227566261-5866df7c-c0f2-4467-b612-7bb608972bfc.png)










