# Azure

## Virtual Machines

### Virtual Machine Resources

- Virtual machine
- IP address - Public and private id addresses
- Network security group - Firewall
- Network interface - Network cards
- Hard disk
- Virtual network - Network where the VMs are hosted

### Disks

- Managed Disks - Maintained by Azure
- Temporary Disks - Data is lost when machine starts/stops

### Machine Start/Stop

- The data on the temporary disk is lost on Stop and Start
- The data on the temporary disk remains on restart
- The machine would get a new IP address on Stop and Start
- The machine IP address is not changed on restart
- When the machine is stopped there might be charge incurred. This is because the machine is stopped by also hosted on the server

### Managed disks and Unmanaged disks

- For VM better to use managed disks
- Un managed disks are created in the storage account and this need to premium type

### Types of disks

- Ultra disk
- Premium SSD
- Standard SSD
- Standard HHD

### Secondary Network

- Used for security. The first network could have public and private to access internet. The second could be a private address.

### Resizing of the Hard disk

- The resize options depends on the quota available in the region.
- When resizing pay attention to the disk types and size. Some VM sizes doesn't support all the disk types and sizes.

### Custom images

- The process of create the image from the virtual machine is called generalization.
- Once the VM is generalization it can't be used.
- Steps to generalization
  - Login into the windows VM
  - Run sysprep from [system32]/sysprep folder
  - Stop and deallocate the VM
  - Go to VM and select Capture VM to create a image.

### VM availability

- Single instance VM have 99.9 availability.
- Two or more VMs deployed in same availability set have 99.95% availability.
- Two or more VMs deployed in across availability zones have 99.99% availability.
