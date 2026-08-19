# guide for setting up a Rocky Linux VM for experimentation
*many instructions are intentionally left vague. Fill in the gaps with your own knowledge, research and by asking your peers. If you are ambitous try to limit online research and AI usage to a minimum. In a potential RHCSA exam your only help will be man pages and the built in help of commands. You also would not have access to GUI tools like VSCode. If you want to challenge yourself use Vim or Nano and the command line for everything. This of course is not a must. The most important goal is to have your VM running by the end of the day.*

## 1. Get the files
*commands: `sha256sum`, `curl`, `wget`, `uname`*

- Read [this](https://42berlin.notion.site/cluster-storage) to inform your decisions on where to save different (potentially large) files
- Download the latest version of Rocky Linux from their official website. There are a couple of different options. Ask yourself: What architecture am I targeting? Do I need everything contained in the downloaded file or can I donwload more components during installation? *hint: The downloaded file will only be used for installing the VM. After that it is not needed anymore.*
- use the `CHECKSUM` file to verify the validity of your download
- without accessing the browser download the `make_vm.sh` script from this url: https://raw.githubusercontent.com/alneuma/vbox_make_vm/refs/heads/main/make_vm.sh

## 2. Use the script to setup the VM
*commands: `chmod`, `vim`, `nano`, `VBoxManage`, `realpath`, `grep`*

- open the script and read the comments
- make necessary adjustments
- run the script

You should see something like this:
```
$ ./make_vm.sh 
Virtual machine 'rocky_vm' is created and registered.
UUID: b1e97b9a-2c6d-4f58-a54a-2221c9b4c5ad
Settings file: '/sgoinfre/goinfre/Perso/your_intra/VBox/vms/rocky_vm/rocky_vm.vbox'
0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
Medium created. UUID: 20a90670-aab4-4866-8d08-360c999022f9
```

*hint: The script is very bare bones. If you like you could revisit it later to fully understand it and make it more production grade.*

## 3. Install Rocky Linux to your VM
*commands: `VBoxManage`*

- start your VM from the command line
- go through the installation process
- create one admin user
- use the whole disk and check `automatic`
- when everything is complete take a snapshot of your VM

## 4. Use SSH to connect from your host to the VM
Using the VM through the VirtualBox GUI window is super annoying. SSH to the rescue!
*commands: `dnf`, `systemctl`, `ssh`*

- update all the packages on your vm
- verify that sshd is running
- connect from you host to the VM (Which port, username and host would you need to use?)

*Hint: Depending on how you have used SSH in the past, there might be a scary looking warning appearing when you try to connect. Can you figure out what this is and how to best deal with it?*

*Question for later: What is the difference between ssh and sshd?*

## 5. Set up public key authentification for SSH
Having to type the password each time you connect to your VM is not super convenient. Also allowing password authentification for SSH is considered to be a security risk. Let's now add some minimal security and convenience by setting up public key authentification for SSH.
*commands: `ssh-keygen`, `ssh-copy-id`, `ssh`, `systemctl`, `vim`, `nano`, `sudo`*

- Figure out how public key security works on a high abstraction level. Which files are needed to set it up?
- create a new ssh key pair on your host. What is the recommended algorithm?
- copy the public key to your VM
- connect to your VM via SSH
- on your VM: configure the SSH service to not permit password authentification and no root login
- restart the ssh service on your VM

## 6. Finalize
The very basic convenience setup for your VM is now done.
*commands: `VBoxManage`*

- take a snapshot
- get comfortable with the different ways of starting and stopping your VM from the command line with VBoxManage
- start experimenting and go back to a snapshot if something breaks

some recommendations:

- overwrite the `PS1` shell variable in your `.bashrc`
- install EPEL (What is that?)
- install a command line text editor you feel comfortable with
- install `tldr` (probably not available for RHCSA examination but super useful if you don't want to go to the browser every five seconds)
