## Automating Deployment of a Kubernetes Cluster in Your Development Environment
   Utilizing Ansible and Vagrant to rapidly deploy a Kubernetes cluster using kubeadm.
 


## Kubernetes Cluster Setup
   The Kubernetes cluster we will create is shown in the diagram above. It consists of three vagrant nodes(kube nodes):
   - k8smaster
   - k8snode1 
   - k8snode2

   These servers are located in different subnets compared to your development machine (ansible node):
   - devbuild

## Getting Started
   Follow these steps to implement the setup:

1. **Provision VMs**
   Change aa, bb, cc and dd located at last octet of ip addresses assigned to each node in Vagrantfile.
   Now you can provision your VMs
   ```bash
   vagrant up
   ```

2. **Login into your dev node(devbuild)**
   ```bash
   vagrant ssh devbuild
   ```

3. **Change the root password for devbuild**
   ```bash
   sudo passwd
   ```
4. **Switch to sudo user and use the new password**
   ```bash
   su -
   ```

5. **Update the package manager and install git**
   ```bash
   apt update
   apt install git -y
   ```

6. **Clone the Repository**
   
   Download this repo to devbuild to get access to Ansible and bash scripts
   ```bash
   cd /
   git clone https://github.com/odennav/kubeadm-ansible-ubuntu.git
   cd kubeadm-ansible-ubuntu
   ```
7. **Generate public/private keys**
   ```bash
   ssh-keygen -o -t rsa
   ```
   
   Once the key-pair is generated, manually copy public RSA key(id_rsa.pub) to all kube nodes(/root/.ssh/authorized_keys)

8. **Install Ansible in devbuild**
   ```bash
   sudo apt install software-properties-common
   sudo add-apt-repository --yes --update ppa:ansible/ansible
   sudo apt install ansible
   ```

### Using Ansible
The bootstrap and k8s folders in this repository contain the Ansible scripts necessary to set up your servers with the required packages and applications.
Edit values of aa, bb and cc with same values used in Vagrantfile.

### Bootstrapping Vagrant Nodes
   All nodes need to be bootstrapped.This process involves updating the OS, creating a non-root user, and setting up SSH to prevent remote login
   by the root user for security reasons.Once the bootstrap is complete, you will only be able to log in as odennav-admin.

   Confirm SSH access to k8snode1:   
   ```bash
   ssh -i /root/.ssh/id_rsa odennav-admin@<k8snode1-ip>
   ```  
   To return to devbuild, type "exit" and press "Enter" or use "Ctrl+D".
   
   Confirm SSH access to k8snode2:
   ```bash
   ssh -i /root/.ssh/id_rsa odennav-admin@<k8snode2-ip>
   ```  
  
   Now you can now bootstrap them:
   ```bash
   cd ../bootstrap
   ansible-playbook bootstrap.yml --limit k8s_master,k8s_node
   ```

### Setting up Kubernetes Cluster
   Your kube nodes are now ready to have a Kubernetes cluster installed on them.
   Execute playbooks in this particular order:

   ```bash
   cd ../k8s
   ansible-playbook k8s.yml  --limit k8s_master
   ansible-playbook k8s.yml  --limit k8s_node
   ```

   Check status of your nodes
   ```bash
   kubectl get nodes
   ```

   Your Kubernetes cluster is ready.


   Enjoy!

