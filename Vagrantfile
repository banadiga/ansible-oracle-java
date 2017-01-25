Vagrant.configure(2) do |config|

    # main & default: normal OS series...
    config.vm.define "main", primary: true do |node|
        node.vm.box = "ubuntu/trusty64"
        #node.vm.box = "ubuntu/precise64"
        #node.vm.box = "debian/jessie64"
        #node.vm.box = "debian/wheezy64"
        #node.vm.box = "bento/centos-7.2"
        #node.vm.box = "bento/centos-6.7"

        # VM configuration
        node.vm.provider :virtualbox do |vb|
            vb.name = "main"
            vb.customize ["modifyvm", :id, "--nicpromisc2", "allow-all", "--groups", "/Ansible oracle java"]
        end

        node.vm.provision "ansible" do |ansible|
            ansible.playbook = "test.yml"
            #ansible.playbook = "prefetch.yml"
            ansible.sudo = true
            #ansible.verbose = "vvv"
        end
    end


    # docker: for auto build & testing (e.g., Travis CI)
    config.vm.define "docker" do |node|
        node.vm.box = "williamyeh/ubuntu-trusty64-docker"

        # VM configuration
        node.vm.provider :virtualbox do |vb|
            vb.name = "docker"
            vb.customize ["modifyvm", :id, "--nicpromisc2", "allow-all", "--groups", "/Ansible oracle java"]
        end
        node.vm.provision "shell", inline: <<-SHELL
            cd /vagrant
            docker build  -f test/Dockerfile-ubuntu14.04  -t java_trusty   .
            docker build  -f test/Dockerfile-ubuntu12.04  -t java_precise  .
            docker build  -f test/Dockerfile-debian8      -t java_jessie   .
            docker build  -f test/Dockerfile-debian7      -t java_wheezy   .
            docker build  -f test/Dockerfile-centos7      -t java_centos7  .
            docker build  -f test/Dockerfile-centos6      -t java_centos6  .
        SHELL
    end

end

