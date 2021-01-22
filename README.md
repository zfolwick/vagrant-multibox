# What is this?
This is a minimalist node multi-box setup. This is the smallest amount of setup required to run (a) multi-node virtual machines, and (b) run ansible against these. This assumes you are set up on a Mac, that you have ansible and vagrant, and a set up VMs available to you.

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
With ansible installed on a linux-based system, copy the hosts file here to `/etc/ansible/` directory, then install sshpass, as ansible uses this to communicate (there's a brew tap to install on macs, linux is installed the usual way). You can now ping all hosts using `ansible all -i hosts -m ping -u vagrant`. It will ssh into each host and perform the command you specify (in this case, `ping`).

To run the ansible playbook in the local.yml file, first spin up the virtualbox servers using `vagrant up`, and the ssh software will be added to it.  Then, you will need to add ssh keys to it.  This can be done in a few ways.  I like to open my ~/.ssh/know_hosts file and delete the lines for the ip addresses for the VMs (10.0.0.10 through 10.0.0.12 in this basic example). I then need to copy the ssh-keys to my known-hosts directory.  I can do that on a mac using:
```
echo vagrant@10.0.0.10 vagrant@10.0.0.11 vagrant@10.0.0.12 | xargs -n1 ssh-copy-id
```

On linux systems, `xargs` may need to replace the `-n1` command.

Then I can can simply run `ansible -i hosts all -m ping`.
