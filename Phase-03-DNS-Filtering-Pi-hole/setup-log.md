# 1. Resolve the Host Port 53 Conflict

Since Mordern Ubuntu uses 'systemd-resolved', which runs a local DNS stub listener on Port 53, we must disable this stub listener so our Pi-hole container can use Port 53 to answer network queries.
To do so we will need to edit the Resolved configuration file located at `/etc/systemd/`:

```
sudo nano /etc/systemd/resolved.conf
```

Uncomment the DNSStubListener line and set it to no:  

<img width="409" height="72" alt="Screenshot 2026-07-18 140944" src="https://github.com/user-attachments/assets/a8875ff6-e355-41eb-af4e-8ed1b1a186c7" />
