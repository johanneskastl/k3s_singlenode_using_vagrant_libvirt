# k3s_singlenode_using_vagrant_libvirt

Vagrant setup for a k3s singlenode cluster (using the libvirt provider for vagrant)

This setup creates a single VM where k3s can be installed via ansible.

Default OS is openSUSE Leap 15.2, but that can be changed in the Vagrantfile.

## Vagrant

1. You need vagrant obviously.
2. Fetch the box, per default this is `opensuse/Leap-15.2.x86_64`, using `vagrant box add opensuse/Leap-15.2.x86_64`.
3. Make sure the git submodules are fully working by issuing `git submodule init && git submodule update`
4. Run `vagrant up`
5. Run `kubectl --kubeconfig ansible/k3s-kubeconfig get nodes` and you should see your server node.
6. Party!

## Disabling the Ansible provisioning

In case you do not want Ansible to install k3s (because you want to install it yourself), just comment out the following lines in the `Vagrantfile`:
```
    node.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.groups = {
        "k3s"  => [ "k3s1" ]
      }
      ansible.playbook = "ansible/playbook-vagrant.yml"
    end # node.vm.provision
```

## Cleaning up

When tearing down the machine, the kubeconfig file that was download does not get deleted unfortunately.
To not cause problems the next time you start, just run `rm ansible/k3s-kubeconfig` and all is well.
