# What is this?
This is a minimalist node multi-box setup. This is the smallest amount of setup required to run (a) multi-node virtual machines, and (b) run ansible against these.

# SSH into box
master: `ssh vagrant@localhost -p 2222`  
node1: `ssh vagrant@localhost -p 2200`  
node2: `ssh vagrant@localhost -p 2201`  

or use vagrant-cli:

```
vagrant ssh master
vagrant ssh node1
vagrant ssh node2
```
# Ansible usage
With ansible installed on a linux-based system, copy the hosts file here to `/etc/ansible/` directory, then install sshpass, as ansible uses this to communicate (there's a brew tap to install on macs, linux is installed the usual way). You can now ping all hosts using `ansible all -m ping -u vagrant`. It will ssh into each host and perform the command you specify (in this case, `ping`).
