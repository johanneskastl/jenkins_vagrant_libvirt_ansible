ENV['VAGRANT_NO_PARALLEL'] = 'yes'

Vagrant.configure("2") do |config|

  ###################################################################################
  # define number of Jenkins agent VMs
  W = 2

  ###################################################################################
  # provision W VMs as agents
  (1..W).each do |i|

    # disable the /vagrant folder, as / is read-only
    config.vm.synced_folder ".", "/vagrant", disabled: true

    # name the VMs
    config.vm.define "jenkins-agent0#{i}" do |node|

      # which image to use
      node.vm.box = "opensuse/Leap-15.5.x86_64"

      # sizing of the VMs
      node.vm.provider "libvirt" do |lv|
        lv.random_hostname = true
        lv.memory = 1024
        lv.cpus = 1
      end

      # set the hostname
      node.vm.hostname = "jenkins-agent#{i}"

    end # config.vm.define agents

  end # each-loop agents

  ###################################################################################
  # disable the /vagrant folder, as / is read-only
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # name the VMs
  config.vm.define "jenkinsserver" do |node|

    # which image to use
    node.vm.box = "opensuse/Leap-15.5.x86_64"

    # sizing of the VMs
    node.vm.provider "libvirt" do |lv|
      lv.random_hostname = true
      lv.memory = 8192
      lv.cpus = 4
    end

    # set the hostname
    node.vm.hostname = "jenkinsserver"

    #################################################
    # only target one node to not run ansible twice
    # but do not limit this to this node (set ansible.limit to all)
    node.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.limit = "all"
      ansible.groups = {
        "jenkins_agents"  => [ "jenkins-agent01", "jenkins-agent02" ],
        "jenkins_server"  => [ "jenkinsserver" ],
      }
      ansible.playbook = "ansible/playbook-vagrant.yml"

    end # node.vm.provision

  end # config.vm.define servers

end # Vagrant.configure
