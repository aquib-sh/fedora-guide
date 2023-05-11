# fedora-guide
Guide to perform some useful tasks in Fedora

<b> Note</b>: Commands related to system edit and update or anything to do outside `/home` should be assumed to be executed by root (preferred way) or by prepending a sudo before each command.

## Update GRUB
```
grub2-mkconfig -o /boot/grub2/grub.cfg
```	  

## Update system after editing /etc/fstab
```
systemctl daemon-reload && systemctl restart local-fs.target
```

## Installing MongoDB server and client
This can be a real pain, </br>Mongo has knowingly or unknowing made it extremely hard to install it on Linux </br>
We will use Docker instead </br>

 ### Install podman</br>

```sudo dnf install podman```

### Pull the mongo image from docker.io</br>

```podman pull mongo```

If confronted with options choose the one from docker.io

### Run the image
```podman run --name mongo -d -p 27017:27017 mongo```

### ------- DOWNLOAD Mongosh or MongoDB Compass ---------
### Download the Mongosh client from official website
```wget https://downloads.mongodb.com/compass/mongosh-1.8.2-linux-x64.tgz```

### Untar the file
```tar -xvzf mongosh-1.8.2-linux-x64.tgz```

### Run the Mongo client
```cd mongosh-1.8.2-linux-x64/bin && ./mongosh```

### Download MongoDB Compass
If you desire a GUI client then MongoDB Compass can be directly downloaded from official website. </br>
Select RedHat and download the RPM package and install it
https://www.mongodb.com/try/download/compass
