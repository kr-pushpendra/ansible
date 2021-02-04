# K8s cluster with Ansible guide

## Environment Used
##### Hypervisor Type-II :  VMware Workstation <br/>
##### OS : Ubuntu Server 18.04.5 LTS (Bionic Beaver) <br/>
##### RAM: 2Gb (on each VM) <br/>
##### CPU: 2 (on each VM) <br/>
##### Hard Disk Space : 10 Gb (on each VM) <br/>
##### Assign static IP on all nodes. </br>
##### Kubernetes version : 1.20.2-00 &nbsp; CRI: containerd &nbsp; CNI: Flannel

## Environment

![ENV](https://github.com/kr-pushpendra/Ansible/blob/master/env.PNG)

## Steps to Follow:

### Setp 1: Setting up Environment
- Install Ubuntu Server 18.04.5 LTS (Bionic Beaver) on all four virtual machines. <br/>
- Ansible Host: 10.0.0.3  
- Add IP addresses to the /etc/hosts file. </br>

![PING](https://github.com/kr-pushpendra/Ansible/blob/master/hosts.PNG)

- Check Internet connection on all nodes </br>

![PING](https://github.com/kr-pushpendra/Ansible/blob/master/ping.PNG)

- Setup ssh-keygen on Ansible Host. </br>
    > $ ssh-keygen </br>
    > It will store the ssh-keys in .ssh directory.
    
![SSH](https://github.com/kr-pushpendra/Ansible/blob/master/ssh.PNG)    
    
- Check ssh is working.
  
![SSH1](https://github.com/kr-pushpendra/Ansible/blob/master/ssh1.PNG)  

- Install Ansible on Ansible Host machine. <br/>

https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-ubuntu

### Step 2: Clone the git repository on Ansible Host machine.
- Make sure git is installed. </br>
   > $ git version <br/>
   
- clone the repo in the /home/pushp folder <br/>
    > $ git clone https://github.com/kr-pushpendra/ansible.git <br/>
 
### Step 3: Update username and password in ini/k8s.ini inventory file. <br/>
- Update ansible_user and ansible_sudo_pass if used other than "pushp" and "1234". <br/>

![ini](https://github.com/kr-pushpendra/Ansible/blob/master/ini.PNG) 

### Step 4: Workflow of the playbook
- Inventory file location: ini/k8s.ini
- Playbook consists of 4 roles:  <br/>
   - k8s_common_base:  &nbsp; The role will configure and install the prerequisite on all three remote hosts. <br/>
    > Disable swap memory <br/>
    > Update and Upgrade <br/>
    > Install CRI runtime: containerd <br/>
    > Letting iptables see bridged traffic <br/>
    > Install kubeadm, kubelet and kubectl <br/>
        
  - k8s_master:  &nbsp; This role will perform following tasks on master node: <br/>
   > Install a Pod network add-on <br/>
   > Install Kubernetes networking model: Flannel <br/>
    
  - k8s_master_token:  &nbsp; This role will perform following tasks on master node: <br/>
   > Create token
   > Save and copy the token to ansible machine
    
  - k8s_join_worker:  &nbsp;  This role will perform following task on worker node: <br/>
   > Use the token to add the worker to the cluster

### Step 5: Run the ansible playbook
- Go to /home/pushp/ansible directory <br/>
   > $ ansible-playbook -i ini/k8s.ini k8s_cluster.yml
   
## Result
- ssh to master <br/>
  > $ ssh 10.0.0.9 <br/>
  > Run "kubectl get nodes" <br/>

![Cluster](https://github.com/kr-pushpendra/Ansible/blob/master/cluster.PNG)
 

