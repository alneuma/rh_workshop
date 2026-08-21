# Guide for setting up a Rocky Linux VM for experimentation
*Many instructions are intentionally left vague. Use your own knowledge, do research, and ask your peers to fill in the gaps. If you are ambitious, try to limit online research and AI usage to a minimum. In an actual RHCSA exam your only help will be man pages and the built-in help of commands. Start getting used to it! You also wouldn't have access to GUI tools such as VS Code. If you want to challenge yourself, use command-line tools for everything. This, of course, is not a must. The most important goal is to have your VM up and running by the end of the day.*

## 1. Get the files
*commands: `sha256sum`, `curl`, `wget`, `uname`*

- Read [this](https://42berlin.notion.site/cluster-storage) to help you decide where to store different (potentially large) files.
- Download the latest release of Rocky Linux from their official website. There are several options. Ask yourself: Which architecture am I targeting? Do I need the full installation image? *Hint for deciding on where to save the file: The downloaded file will only be used to install the VM. You won't need it afterward.*
- Use the `CHECKSUM` file to verify the integrity of your download
- Download the `make_vm.sh` script from this URL: https://raw.githubusercontent.com/alneuma/vbox_make_vm/refs/heads/main/make_vm.sh. (Can you do it without accessing the browser?)

## 2. Use the script to set up the VM
*commands: `chmod`, `vim`, `nano`, `VBoxManage`, `realpath`, `grep`*

- Open the script and read the comments
- Make any necessary changes
- Run the script

You should see something like this:
```
$ ./make_vm.sh 
Virtual machine 'rocky_vm' is created and registered.
UUID: b1e97b9a-2c6d-4f58-a54a-2221c9b4c5ad
Settings file: '/sgoinfre/goinfre/Perso/your_intra/VBox/vms/rocky_vm/rocky_vm.vbox'
0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
Medium created. UUID: 20a90670-aab4-4866-8d08-360c999022f9
```

*Hint: The script is very bare-bones. If you like, you can revisit it later to fully understand it and make it more production-grade.*

## 3. Install Rocky Linux on your VM
*commands: `VBoxManage`*

- Start your VM from the command-line
- Go through the installation process
    - Create one admin user
    - Use the entire disk and check `Automatic`
    - Choose the option for a `Minimal Installation`
- When the installation is complete, take a snapshot of your VM

## 4. Use SSH to connect from your host machine to the VM
Using the VM through the VirtualBox GUI window is super annoying. SSH to the rescue!

*commands: `dnf`, `systemctl`, `ssh`*

- Update all the packages on your VM
- Verify that `sshd` is running
- Connect from your host to the VM (Which port, username, and host do you need to use?)

*Hint: Depending on how you have used SSH in the past, there might be a potentially scary-looking warning when you try to connect. Can you figure out what it means and how to deal with it?*

*Question for later: What is the difference between `ssh` and `sshd`?*

## 5. Set up public key authentication for SSH
Having to type the password each time you connect to your VM is not super convenient. Also allowing password authentication for SSH is considered to be a security risk. Let's now add some basic security and convenience by setting up public key authentication for SSH.

*commands: `ssh-keygen`, `ssh-copy-id`, `ssh`, `systemctl`, `vi`, `sudo`*

- Figure out how public-key authentication works at a high level. Which files do you need to set it up?
- Create a new SSH key pair on your host. Which algorithm is the modern recommendation?
- Copy the public key to your VM.
- Connect to your VM via SSH.
- On your VM, configure the SSH service to disable password authentication and root login.
- Restart the SSH service on your VM.

## 6. Finish up
The basic convenience setup for your VM is now done.
*commands: `VBoxManage`*

- Take a snapshot
- Get comfortable with the different ways to start and stop your VM from the command line using `VBoxManage`
- Start experimenting and restore a snapshot if something breaks

Some recommendations:

- Overwrite the `PS1` shell variable in your `.bashrc`
- Install EPEL (What is that?)
- Install a text editor you feel comfortable using on the command line
- Install `tldr` (probably not available during the RHCSA examination but super useful if you don't want to go to the browser every five seconds)
