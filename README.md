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
```
