# fedora-guide
Guide to perform some useful tasks in Fedora

## Update GRUB
```
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```	  

## Update system after editing /etc/fstab
```
sytemctl daemon-reload && systemctl restart local-fs.target
```