# fedora-guide
Guide to perform some useful tasks in Fedora

<b> Note</b>: Commands related to system edit and update or anything to do outside `/home` should be assumed to be executed by root (preferred way) or by prepending a sudo before each command.

### Speedup DNF
Add the below two lines to the end of dnf configuration file in `/etc/dnf/dnf.conf`
```
max_parallel_downloads=10
fastestmirror=True
```

### Installing Nvidia drivers
enable RPM fusion from https://rpmfusion.org/Configuration
```
sudo dnf update && sudo dnf upgrade
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda
```
Now restart the system and after successful restart you must be using nvidia drivers by default<br>
Now enter the below command to ensure that graphics don't crash after resuming from a suspend
```
sudo systemctl enable nvidia-{suspend,resume,hibernate}
```

### Install some useful packages
```
sudo dnf install vim neofetch htop gimp fira-code-fonts git gh python3-pip obs-studio gnome-disk-utility
```

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

```
sudo dnf install podman
```

### Pull the mongo image from docker.io</br>

```
podman pull mongo
```

If confronted with options choose the one from docker.io

### Run the image
```
podman run --name mongo -d -p 27017:27017 mongo
```

Next time if you wish to run the container you would have to use
```
podman start mongo
```

### Installing and running Mongosh client
#### Download Mongosh client
```
wget https://downloads.mongodb.com/compass/mongosh-1.8.2-linux-x64.tgz
```

#### Untar the file
```
tar -xvzf mongosh-1.8.2-linux-x64.tgz
```

#### Running the client
```
cd mongosh-1.8.2-linux-x64/bin && ./mongosh
```

### Installing MongoDB Compass
If you desire a GUI client then MongoDB Compass can be directly downloaded from official website. </br>
Select RedHat and download the RPM package and install it
https://www.mongodb.com/try/download/compass

## Installing PostgreSQL and it's Python drivers
An excellent guide for installation and running of PostgresSQL can be found at 
[https://docs.fedoraproject.org/en-US/quick-docs/postgresql/](https://docs.fedoraproject.org/en-US/quick-docs/postgresql/)</br>
Note: you will also have to follow the steps in guide to change your `/var/lib/psql/data/pg_hba.conf` to resolve `ident` issue

### Installing psycopg2 driver for connecting postgres to python
If you directly install the driver from pip you are going to run into compilation issues. We must first install the necessary dev packages in order to build the psycopg2
```bash
sudo dnf install libpq-devel python3-devel postgresql-devel
```
Now after installing the necessary build dependencies we are ready to install our python driver for postgres
```bash
pip3 install psycopg2
```
Incase if the above still fails THEN we will install the binary of the driver
```bash
pip3 install psycopg2-binary


Here’s a **"SELinux and Tomcat Setup on Fedora"** section for your Fedora Guide README:

---

## **SELinux and Tomcat Setup on Fedora**

When running Tomcat on Fedora with SELinux enabled, you may encounter issues starting Tomcat via `systemctl`. This is typically caused by SELinux policies blocking necessary operations. Follow these steps to configure SELinux and ensure Tomcat runs smoothly:

### **1. Check SELinux Status**
Verify if SELinux is enforcing:
```bash
sestatus
```

### **2. Update SELinux Context for Tomcat**
Set the correct SELinux context for Tomcat files:
```bash
sudo semanage fcontext -a -t tomcat_exec_t '/path/to/tomcat(/.*)?'
sudo restorecon -Rv /path/to/tomcat
```
This ensures that all files and directories under `/path/to/tomcat` (e.g., `/opt/tomcat`) have the appropriate SELinux context.

### **3. Verify File Permissions**
Ensure all scripts, such as `startup.sh` and `catalina.sh`, have execute permissions:
```bash
sudo chmod +x /path/to/tomcat/bin/*.sh
```

### **4. Reload and Test**
Reload systemd to reflect changes and start Tomcat:
```bash
sudo systemctl daemon-reload
sudo systemctl start tomcat
```

### **5. Troubleshooting Denials**
If Tomcat still doesn’t start:
1. Check the SELinux audit logs:
   ```bash
   sudo ausearch -m avc -ts recent
   ```
2. Generate a custom SELinux policy module:
   ```bash
   sudo ausearch -c 'startup.sh' | audit2allow -M tomcat_policy
   sudo semodule -i tomcat_policy.pp
   ```

### **6. Verify Tomcat Service**
Ensure the service is running:
```bash
sudo systemctl status tomcat
```

### **Key Notes**
- Always ensure the correct SELinux context (`tomcat_exec_t`) for Tomcat scripts and directories.
- Use `audit2allow` to create tailored SELinux policies for your environment.
- Test changes with `setenforce 0` temporarily to confirm SELinux is the issue before applying permanent fixes.

---
