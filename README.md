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

You will notice that `vagrant ssh` is quite slow (at last in Windows), and to improve the speed, you can define an entry in your `.ssh/config` with the Vagrant guest. See the [SSH config](docs/vagrant.md#ssh-config) part in the `vagrant.md`

### Basic commands

The basic [Vagrant commands](docs/vagrant.md) are:

| command | meaning |
| ------- | ------- |
| init | initalize the Vagrantfile |
| up | boot the VM |
| ssh | to ssh into the VM |
| halt | shutdown the VM |
| reload | restart the VM |
| destroy | deletes the VM |

## Apache2

The focus of this lab is in Ansible, but it's possible to do simple configuration directly in the Vagrant file. As an example we will [install the Apache webserver](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-debian-11), first directly on the server (that is "by-hand"), and then using the _provisioning_ option in Vagrant. In either ways, we will need to [expose the HTTP port](https://developer.hashicorp.com/vagrant/docs/networking/forwarded_ports) of the virtual machine to the host (our computer):


```
  # Expose the HTTP port (80) of the VM to the host
  config.vm.network "forwarded_port", guest: 80, host: 8080
```

### Directly on the Server

Start the server (the VM):

```
vagrant up
```

Finally we add the web server:

```
vagrant@sidon:~$ sudo apt update
vagrant@sidon:~$ sudo apt install apache2
vagrant@sidon:~$ sudo systemctl status apache2
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; preset: enabled)
     Active: active (running)
     (...)
```

Verify that Apache is running by visiting the URL:

```
http://localhost:8080/
```

And you should see the welcome page of the Apache web server.

### Vagrant Provisioning

Another option is to do the provisioning with Vagrant, via [`shell provisioner`](https://developer.hashicorp.com/vagrant/docs/provisioning/shell). The easiest way to put the script directly in the Vagrant file (`inline`), but an interestion alternative is to put the script in separated file (`path`), which can be even on a remote server.

> It's better to destroy the VM between this kind of test.

Using the `inline` option first. Add the following script to the beginning of the Vagranf file

```
(...)
$script = <<-SCRIPT
echo I am provisioning...
sudo apt update
sudo apt install -y apache2
SCRIPT
```

Then call the script

```
Vagrant.configure("2") do |config|
  (...)
  config.vm.provision "shell", inline: $script
  (...)  
end
```

Again it's possible to check the web server on the host:

```
http://localhost:8080/
```

## Ansible

Ansible is often described as a _configuration management tool_ and with this kind of tool, the user describe the desired state of the server: which packages should be installed, the configuration files have the expected values, the right permissions, and so on. Another use of Ansible is with _orchestration_, that is, the coordination of the deployment of servers or services. For example, the database has to started before the web server.
