Read this to inform your decisions on where to save different (potentially large) files: https://42berlin.notion.site/cluster-storage

- Download the latest version of Rocky Linux from their official website use the **Boot ISO** or the **Minimal ISO** (this will only be used during setup)

- use the `CHECKSUM` file from their website to verify the validity of your download
*hint: use `sha256sum -c`*

- Download the `make_vm.sh` script from github
*challange: do it with curl from the command line*

here is the link:
https://raw.githubusercontent.com/alneuma/vbox_make_vm/refs/heads/main/make_vm.sh

first milestone reached

- Modify the `make_vm.sh` script to suite your needs
    - `name` can be chosen freely
    - `iso` should match the path of your downloaded Rocky Linux ISO
    - `vmhdpath` should ideally be a path somewhere in `sgoinfre`
    - `goinfre` and `sgoingfre` are both *symbolic links*
    - When filling in the paths into the script make certain to not depend on symbolic links but use *absolute paths* (the `realpath` command might be useful for that)
    - `ostype` is the Virtual Box ID for the OS we are using. It should be something that corresponds to the latest version of RedHead, 64 Bit and not ARM
    - use `VBoxManage list ostypes` together with `grep` to figure out the exact name of the ID
    - hd size should be at least 6GB i.e. `6144` better use more
    - `ssh_map_vm` should be `22`
    - All the other fields are not too important. You can leave them as they are

- make the script executable
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

*suggestion for later: use a free moment to understand the script and find ways to improve it*

second milestone reached

- use `VBoxManage startvm <vm-name>` to start your new VM from the command line

- go through the installation process
- create one admin user
- use the whole disk and check `automatic`

- when the installation and subsequent reboot is complete take a snapshot of your vm `VBoxManage snapshot <vm-name> take <snapshot-name>

third milestone reached

Using the vm through the VirtualBox window is super annoying. That is why the first thing we want to do is to setup ssh such that we can connect from the host to the vm from the command line

- update all the packages on your vm (look into the `dnf`) command
- verify that sshd is running on the vm (`systemctl`)
*question for later research: what is the difference between `ssh` and `sshd`? What does the `d` stand for?*
- use `firewall-cmd` to open tcp port 22
- use `ssh` to connect to the vm from your host with `ssh_map_host` from your `make_vm.sh` script as the port your vm admin user as the user and localhost as the destination host to verify that everything works
- disconnect

fourth milestone reached

It generally is a bad idea to allow ssh connections via password authentification. Let's now add some minimal security (and convenience) by setting up public key authentification for ssh.

- Question: On a high abstraction level: How does public key security work? Which files are involved?
- create a new ssh key pair on your host `ssh-keygen`
    - don't just use `rsa` as the algorithm for your ssh-key. What is recommended?
- use `ssh-copy-id` to copy the public key to the vm
- use `ssh` to connect to the vm
- change the sshd config file to forbid root login and password authentification
- use `systemctl` to restart sshd

fifth milestone reached

The very basic convenience setup for your vm is now done. the next time you start it you can use `VBoxManage startvm <vm-name> --type=headless`. This will start the vm in the background without opening a Window. You don't need it anymore. Everything can be done via ssh.

Time to take another snapshot (To save space you can delete the old one)

You can now start experimenting with your vm. If you break something or something goes wrong you can use the snapshot to go back to a previous state.

some recommendations:

- overwrite the `PS1` shell variable in your `.bashrc`
- install EPEL (What is that?)
- install a (command line) text editor you feel comfortable with
- install `tldr` (probably not available for RHCSA examination but super useful if you don't want to go to the browser every five seconds)
