Vagrant.configure("2") do |config|

    config.vm.define "k8smaster" do |k8smaster|
    config.ssh.insert_key = false
         k8smaster.vm.box = "bento/ubuntu-22.04"
         k8smaster.vm.network "private_network", ip: "192.168.50.2"
         k8smaster.vm.hostname = "k8s-master"
         k8smaster.vm.provider "virtualbox" do |vb|
             vb.memory = "2048"
             vb.name = "k8smaster"
             vb.cpus = "2"
         end
     end

    config.vm.define "k8snode1" do |k8snode1|
    config.ssh.insert_key = false
        k8snode1.vm.box = "bento/ubuntu-22.04"
        k8snode1.vm.network "private_network", ip: "192.168.50.3"
        k8snode1.vm.hostname = "k8s-node1"
        k8snode1.vm.provider "virtualbox" do |vb|
            vb.name = "k8snode1"
            vb.memory = "2048"
        end
    end
    
    config.vm.define "k8snode2" do |k8snode2|
    config.ssh.insert_key = false
        k8snode2.vm.box = "bento/ubuntu-22.04"
        k8snode2.vm.network "private_network", ip: "192.168.50.4"
        k8snode2.vm.hostname = "k8s-node2"
        k8snode2.vm.provider "virtualbox" do |vb|
            vb.name = "k8snode2"
            vb.memory = "2048"
        end
    end

    config.vm.define "devbuild" do |devbuild|
    config.ssh.insert_key = false
        devbuild.vm.box = "bento/ubuntu-22.04"
        devbuild.vm.network "private_network", ip: "192.168.20.1"
        devbuild.vm.hostname = "devbuild"
        devbuild.vm.provider "virtualbox" do |vb|
            vb.name = "devbuild"
            vb.memory = "2048"

        end
    end
    

end


