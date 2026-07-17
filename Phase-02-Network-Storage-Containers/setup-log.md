# 1. Native Samba NAS Installation & Configuration  
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

Next we set the correct ownership and permissions:

```
sudo chown -R user1:user1 /srv/samba/private/user1
sudo chmod 700 /srv/samba/private/user1
sudo chmod -R 775 /srv/samba/public
```

### Creation of Samba User

We then need to add the Linux user to Samba since Samba uses its own password database instead of standard Linux account passwords:

```
sudo smbpassword -a userexample
```

### Configuration of Samba

The configuration file is located at `/etc/samba/smb.conf` and we need to firstly make a back up and then append the active file it with custom directory rules:
```
# Backup
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak

sudo nano /etc/samba/smb.conf
```
We then add the Public and Private shares at the bottom of the config file, here is mine for example:  

<img width="735" height="317" alt="Screenshot 2026-07-18 013231" src="https://github.com/user-attachments/assets/fe2f19de-8a8e-4cf9-8ea1-eea24958a524" />

Lastly restart Samba to apply the changes:
```
sudo systemctl restart smbd
```

<br>

# 2. Docker & Portainer Setup

### Installation of Docker
Instead of installing the older version of Docker found in the standard Ubuntu repositories, the official Docker repository is mapped directly. This ensures the server gets immediate security patches and verifies the software using official cryptographic keys:

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y
```


### Installation and Deployment of Portainer
We then need to create a Docker volume to store Portainer's data:

```
sudo docker volume create portainer_data
```

Next we run the Portainer container, bounding it to port 9000 and ensuring that it runs on startup:

```
sudo docker run -d \
  -p 9000:9000 \
  --name=portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest
```
