**WireGuard VPN Lab – Project Documentation**

**Introduction** <br>
For this project, I set up my own WireGuard VPN server on a DigitalOcean droplet and connected to it from both my laptop and my iPhone. The goal was to have all my internet traffic go through a secure, private tunnel using an IP address that I control. This protects my connection on public Wi-Fi and also gives me a consistent remote IP.

**Setting Up the Server** <br>
I created a small Ubuntu droplet on DigitalOcean and connected to it through SSH. Once I was in, I installed Docker and prepared a folder where the WireGuard container would run.
Inside the wireguard folder, I created the docker-compose.yml file using nano. When nano asked me to pick a file name, I typed docker-compose.yml, saved it, and exited.
After that, I ran:
docker compose up -d
WireGuard started downloading, and once it finished, I checked with:
docker ps
The container was running on UDP port 51820, which meant the server side was active.

**Getting the WireGuard Client Config** <br>
At first, I kept trying to find the configuration files on the host machine, but they weren’t there. After some troubleshooting, I realized the configs live inside the container.
So I opened the container’s shell:
docker exec -it wireguard bash
Inside /config, I found the folders: <br>
•	peer1 <br>
•	peer2 <br>
•	server <br>
Since I used peer2 for my laptop, I opened the file: <br>
cat /config/peer2/peer2.conf <br>
This gave me the exact configuration I needed. It included my private key, the internal VPN address, DNS, and the endpoint pointing back to my droplet’s public IP:
134.209.161.192:51820.

