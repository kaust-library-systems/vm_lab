# VM Lab

Creating and managing a test or a development environment with virtual machines (VMs). The machines are created and provisioned using [Vagrant](https://www.vagrantup.com/), and for the configuration we will use [Anbible](https://www.ansible.com/).

## Vagrant

The operating system used for this laboratory is [Debian 12](https://www.debian.org/) packaged as a Vagrant Box [Bookworm 64 bit](https://app.vagrantup.com/debian/boxes/bookworm64), because it's similar, but much lighter than Ubuntu. 

Everything starts with a `Vagrantfile`. The  `Vagrantfile` describes the virtual machines that will be used in the project. There can be only one `Vagrantfile` per project. The Vagrant file defines a basic VM with 1GB of memory and 2 CPUs:

```
  config.vm.provider "virtualbox" do |vb|
(...)
    # Customize the amount of memory on the VM:
    vb.memory = "1024"
    vb.cpus = 2
  end
```

With this simple configuration we can start the VM:

```
vagrant up
```

Afte the boot process, we can access the machine,

```
PS C:\Users\mgarcia\Work\vm_lab> vagrant ssh
(...)
vagrant@bookworm:~$ lsb_release -a
No LSB modules are available.
Distributor ID: Debian
Description:    Debian GNU/Linux 12 (bookworm)
Release:        12
Codename:       bookworm
vagrant@bookworm:~$
```

### Basic commands

The basic Vagrant commands are:

| command | meaning |
| ------- | ------- |
| init | initalize the Vagrantfile |
| up | boot the VM |
| halt | shutdown the VM |
| reload | restart the VM |
| destroy | deletes the VM |

For example:

```
vagrant destroy
```

or

```
vagrant up
```

## Ansible
