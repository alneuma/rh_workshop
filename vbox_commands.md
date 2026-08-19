# useful commands to control VirtualBox from the command line
*hint: create an alias for VBoxManage*

| command | effect |
|-|-|
| `VBoxManage list vms` | list your registered VMs |
| `VBoxManage list runningvms` | list your running VMs |
| `VBoxManage unregistervm <vm-name> --delete` | unregister a VM and delete its files |
| `VBoxManage startvm <vm-name>` | start a VM |
| `VBoxManage startvm <vm-name> --type=headless` | start a VM without the GUI window |
| `VBoxManage snapshot <vm-name> take <snapshot-name>` | take a snapshot |
| `VBoxManage snapshot <vm-name> delete <snapshot-name>` | delete a snapshot |
| `VBoxManage snapshot <vm-name> restore <snapshot-name>` | restore a snapshot |
| `VBoxManage snapshot <vm-name> list` | list snapshots of a VM |
| `VBoxManage controlvm <vm-name> savestate` | shut down the VM while saving its current state |
| `VBoxManage controlvm <vm-name> acpipowerbutton` | instruct the VM to shut down gracefully (if configured) |
| `VBoxManage controlvm <vm-name> poweroff` | force the VM to shut down |
