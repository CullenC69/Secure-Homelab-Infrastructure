# 1. Resolve the Host Port 53 Conflict

Since Mordern Ubuntu uses 'systemd-resolved', which runs a local DNS stub listener on Port 53, we must disable this stub listener so our Pi-hole container can use Port 53 to answer network queries.
To do so we will need to edit the Resolved configuration file located at `/etc/systemd/`:

```
sudo nano /etc/systemd/resolved.conf
```

Uncomment the `DNSStubListener` line and set it to no:  

<img width="409" height="72" alt="Screenshot 2026-07-18 140944" src="https://github.com/user-attachments/assets/a8875ff6-e355-41eb-af4e-8ed1b1a186c7" />

### Linking the System to a Public DNS Server
Since we removed the stub listener, we need to create a symbolic link so the server can continue resolving internet domain names

```
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```
To apply the changes we will need to restart the network resolution daemon:

```
sudo systemctl restart systemd-resolved
```

<br>

# 2. Update the Host Firewall (UFW)
We will then need to open the standard DNS transmission pathways to allow clients on the local network to query the server and also open the custom port for the Pi-hole web interface

```
sudo ufw allow 53/tcp
sudo ufw allow 53/udp

sudo ufw allow 8080/tcp

sudo ufw reload
```

<br>

# 3. Deployment of the Pi-hole Container
Using the Docker environment built in Phase 2, we created dedicated directories to keep our configuration data safe during any container updates

```
sudo mkdir -p /etc/pihole
sudo mkdir -p /etc/dnsmasq.d
```

### Launch the Pi-hole Docker Container
Next run the deployment script:

```
sudo docker run -d \
  --name pihole \
  -p 53:53/tcp -p 53:53/udp \
  -p 8080:80/tcp \
  -v /etc/pihole:/etc/pihole \
  -v /etc/dnsmasq.d:/etc/dnsmasq.d \
  -e TZ="Asia/Singapore" \
  -e FTLCONF_webserver_api_password="whatever_password" \
  --restart=always \
  pihole/pihole:latest
```

<br>

# 4. Configuration of Local DNS Mapping
To configure the local DNS mapping login to the Pi-hole admin interface at `http://192.168.0.x:8080/admin`. In the sidebar settings head to `DNS records` and add the custom domains which would look like this:  

<img width="612" height="731" alt="Screenshot 2026-07-18 152921" src="https://github.com/user-attachments/assets/624a5fe8-d5bf-4648-9db2-1b8a5c57e677" />




