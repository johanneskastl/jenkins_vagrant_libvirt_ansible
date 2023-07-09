# jenkins_vagrant_libvirt_ansible

This Vagrant setup creates a VM and configures [a Jenkins
server](https://jenkins.io). It also creates a VM that can be used as a
Jenkins agent (manually, there is no full automation yet).

Default OS is openSUSE Leap 15.4. Although that can be changed in the
Vagrantfile, please beware that this will break the Ansible provisioning.

## Vagrant

1. You need vagrant obviously. And ansible. And git...
1. Fetch the box, per default this is `opensuse/Leap-15.5.x86_64`, using
   `vagrant box add opensuse/Leap-15.5.x86_64`.
1. Make sure the git submodules are fully working by issuing `git submodule init
   && git submodule update`
1. Run `vagrant up`
1. The Ansible playbook will output the IP address (and port) of the Jenkins
   server as well as the initial admin password to be used for the first login.
1. Open the URL and log in using the initial password.
1. Configure Jenkins as usual.
1. Party!

## Disabling the Ansible provisioning

In case you do not want Ansible to install teleport (because you want to install
it yourself), just comment out the following lines in the `Vagrantfile`:

```hcl
    node.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.limit = "all"
      ansible.groups = {
        "jenkins_agents"  => [ "jenkins-agent01" ],
        "jenkins_server"  => [ "jenkinsserver" ],
      }
      ansible.playbook = "ansible/playbook-vagrant.yml"
```

You also find all of the playbooks in the `ansible` folder.

## License

BSD-3-Clause

## Author Information

I am Johannes Kastl, reachable via kastl@b1-systems.de.
