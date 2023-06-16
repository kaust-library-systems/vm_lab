# Vagrant

## SSH Config

To convinent way of accessing the guest machines is by configuring directly on the `.ssh/config` file. First print the configuration of the guest:

```
vagrant ssh-config
```

And you shold see something similar to the following output:

```
Host default
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile C:/Users/mgarcia/Work/vm_lab/.vagrant/machines/default/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL
```

Next add to your `config`:

```
PS C:\Users\mgarcia> notepad.exe .ssh/config
```

Note that we named the entry with the `hostname` from the `Vagrantfile`

```
Host sidon
  HostName 127.0.0.1
  User vagrant
(...)
  LogLevel FATAL
```

Finally you can test the access to the guest:

```
PS C:\Users\mgarcia\Work\vm_lab> ssh sidon
Linux sidon 6.1.0-9-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.27-1 (2023-05-08) x86_64
(...)
```

And the good thing is that the configuration seems to survive even if the guest is destroyed.

## Basic Commands

The basic Vagrant commands are:

| command | meaning |
| ------- | ------- |
| init | initalize the Vagrantfile |
| up | boot the VM |
| ssh | to ssh into the VM |
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
