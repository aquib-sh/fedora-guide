# fedora-guide
Guide to perform some useful tasks in Fedora

<b> Note</b>: Commands related to system edit and update or anything to do outside `/home` should be assumed to be executed by root (preferred way) or by prepending a sudo before each command.

## Update GRUB
```
grub2-mkconfig -o /boot/grub2/grub.cfg
```	  

## Update system after editing /etc/fstab
```
sytemctl daemon-reload && systemctl restart local-fs.target
```