Benefits:
- Central Management
- Resource sharing
- Uptime
- Configure one NFS share for all nodes

Requires 3 nodes to function properly
Keep servers in odd numbers

Steps:
Install Proxmox on each server

Need to switch to community repo

 ```bash
root@twwp-pxmx-01:/etc/apt/sources.list.d# cat /etc/apt/sources.list.d/pve-enterprise.list
#deb https://enterprise.proxmox.com/debian/pve bookworm pve-enterprise
deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription
```

```bash
root@twwp-pxmx-01:/etc/apt/sources.list.d# cat /etc/apt/sources.list.d/ceph.list
#deb https://enterprise.proxmox.com/debian/ceph-quincy bookworm enterprise
deb http://download.proxmox.com/debian/ceph-quincy bookworm no-subscription
```

Install Ceph Quincy
`pveceph install --repository no-subscription`

Actually, just go to the GUI and install reef version on all nodes with "no-subscription" selected

![](Pasted%20image%2020240821111355.png)

Go to Cluster > Create Cluster > name it and hit create. Close window when finished.

TO get Join information, click the Join information tab

To join a cluster press the "Join Cluster" button from the same place. 

