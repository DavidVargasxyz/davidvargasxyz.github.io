## Remove Password Prompt

Add user to libvirt and kvm groups:
```bash
sudo usermod -a -G libvirt $(whoami)
sudo usermod -a -G kvm $(whoami)
```
