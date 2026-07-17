# Native Samba NAS Installation & Configuration  
These were the following commands I used on the Ubuntu Server terminal to establish the file storage services:  


### Installation of Samba  

We first start by installing the Samba packages:  

```
sudo apt update
sudo apt install samba samba-common-bin -y
```


### Creation of Shared Folders

To isolate private storage domains, we created seperate folders for the public share and a private user folder:

```
sudo mkdir -p /srv/samba/public
sudo mkdir -p /srv/samba/private/user1
```

