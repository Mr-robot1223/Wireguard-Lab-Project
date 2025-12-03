**WireGuard VPN Lab – Project Documentation**

Introduction
For this project, I set up my own WireGuard VPN server on a DigitalOcean droplet and connected to it from both my laptop and my iPhone. The goal was to have all my internet traffic go through a secure, private tunnel using an IP address that I control. This protects my connection on public Wi-Fi and also gives me a consistent remote IP.

**Setting Up the Server**

I created a small Ubuntu droplet on DigitalOcean and connected to it through SSH. Once I was in, I installed Docker and prepared a folder where the WireGuard container would run.
Inside the wireguard folder, I created the docker-compose.yml file using nano. When nano asked me to pick a file name, I typed docker-compose.yml, saved it, and exited.
After that, I ran:
docker compose up -d
WireGuard started downloading, and once it finished, I checked with:
docker ps
The container was running on UDP port 51820, which meant the server side was active.
