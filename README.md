## Rapid Deployment of a Kubernetes Cluster with Ansible
   Utilize Ansible and Vagrant to rapidly deploy a Kubernetes cluster in a Test environment.
 
   The Kubernetes cluster we will create is shown in the diagram above. It consists of three vagrant nodes `kube nodes`
   - k8smaster
   - k8snode1 
   - k8snode2

   These servers are located in different subnets compared to your development machine `ansible node`
   - devbuild

## Getting Started

   Follow these steps to implement the setup:

   **Provision VMs**
   
   Login into your ansible node
   ```bash
   vagrant up
   vagrant ssh devbuild
   ```

   Change the root password for ansible node and switch to root
   ```bash
   sudo passwd
   su - 
   ```
  
   Update the package manager and install Git
   ```bash
   apt update
   apt install git -y
   ```

   Download this repository to `devbuild` machine, access ansible and bash scripts
   ```bash
   cd /
   git clone https://github.com/odennav/kubeadm-ansible-ubuntu.git
   cd kubeadm-ansible-ubuntu
   ```

   Generate SSH public/private key pair
   ```bash
   ssh-keygen -t rsa -b 4096
   ```
   
   Once the RSA key-pair is generated, manually copy the public key `id_rsa.pub` to the `/root/.ssh/authorized_keys` file in all kube nodes.

   **Install Ansible in devbuild**
   ```bash
   sudo apt install -y software-properties-common
   sudo add-apt-repository --yes --update ppa:ansible/ansible
   sudo apt install -y ansible
   ```


The bootstrap and k8s folders in this repository contain the Ansible scripts necessary to set up your servers with the required packages and applications.


### Bootstrapping Vagrant Nodes

   All nodes will be bootstrapped using Ansible.

   Bootstrap the master node
   ```bash
   cd kubeadm-ansible-ubuntu/bootstrap/
   ansible-playbook bootstrap.yml --limit k8s_master
   ```

   Bootstrap the worker nodes
   ```bash
   ansible-playbook bootstrap.yml --limit k8s_node
   ```
   
   Once the bootstrap is complete, you can log in as `odennav-admin`.

   Confirm SSH access to master node
   ```bash
   ssh  odennav-admin@192.168.50.2
   ```
   Confirm SSH access to 1st worker node   
   ```bash
   ssh  odennav-admin@192.168.50.3
   ```  
   To return to devbuild, type `exit` and press `Enter` or use `Ctrl+D`
   
   Confirm SSH access to 2nd worker node
   ```bash
   ssh  odennav-admin@192.168.50.4
   ```  
  
### Setting up Kubernetes Cluster
   Your kube nodes are now ready to have a Kubernetes cluster installed on them.
   
   Execute playbooks in this particular order:

   ```bash
   cd ../kubeadm
   ansible-playbook k8s.yml  --limit k8s_master
   ansible-playbook k8s.yml  --limit k8s_node
   ```

   Check status of your nodes
   ```bash
   kubectl get nodes
   ```

   Your Kubernetes cluster is ready.


   ![](https://github.com/odennav/kubeadm-ansible-ubuntu/blob/main/docs/cluster.png)


-----

   Enjoy!

