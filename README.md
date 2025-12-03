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

**Connecting WireGuard on My Mac** <br>
On my Mac, I opened the WireGuard app, clicked Add Tunnel, and chose Add empty tunnel. I pasted the entire peer2 configuration into the editor and saved it as Wireguard Project.
When I clicked Activate, the status turned green, and after a few seconds the handshake appeared.
To confirm it was working, I opened an IP leak website. My IP changed to the one from DigitalOcean, exactly like in the screenshot: <img width="1800" height="1169" alt="Screenshot 2025-12-02 at 12 28 11 AM" src="https://github.com/user-attachments/assets/d5978211-2c58-4ee4-8bca-603225cf2dc8"/> 
 <br>
134.209.161.192 — New Jersey (DigitalOcean) <br>
That told me that all traffic was being routed through my VPN.

**Connecting From My iPhone** <br>
For my phone, I used the QR code that WireGuard provides. I generated the QR code from the WireGuard app on my laptop and scanned it with the WireGuard app on my iPhone.
The phone connected instantly. <br>
The assigned address was 10.13.13.2, and the server showed the same endpoint:
134.209.161.192:51820. <br>
The log showed successful activation and a good handshake. This meant both my phone and laptop were tunneling traffic through the same server. <br>
<img width="585" height="1266" alt="IMG_8517" src="https://github.com/user-attachments/assets/b9fa1ad5-52f4-468c-b517-43feaac154d9" />

**Troubleshooting** <br>
This project wasn’t smooth the whole way — here are the main issues I had and how I fixed them:
1. “No such file or directory” when trying to open configs <br>
I kept typing the wrong path because the config folder only exists inside the container. <br>
Fix: <br>
docker exec -it wireguard bash <br>
ls /config/peer2 <br>

2. IP leak showing my real IP <br>
At one point, the leak test still showed my regular laptop IP. <br>
Fix: I had forgotten to activate the tunnel in the WireGuard app. <br>

3. Wrong nano usage <br>
Nano asked me to write a file name, which confused me at first. <br>
Fix: I just typed docker-compose.yml and saved. <br>

4. WireGuard not showing in DigitalOcean dashboard <br>
I kept looking for “WireGuard” but only the droplet itself appears. There is no separate VPN section. <br>

5. iPhone not connecting <br>
The first time I added it manually, it didn’t work. <br>
Fix: Using the QR code solved it immediately. <br>

**Final Testing** <br>
I tested the VPN on both devices: <br>
Laptop results <br>
•	IP changed to the server’s IP <br>
•	DNS requests routed through the VPN <br>
•	Handshake updated regularly <br>
•	Data in/out increasing as I browsed <br>

**Phone results** <br>
•	VPN status showed connected in iOS settings <br>
•	Logs showed the tunnel starting successfully <br>
•	IP on my phone matched the DigitalOcean server <br>

**Conclusion** <br>

By the end of the project, I had a fully working WireGuard VPN running on a Docker container, with both my Mac and iPhone connected through it. I learned how to troubleshoot container paths, how WireGuard handles keys and endpoints, and how to verify that the VPN is actually tunneling traffic correctly. <br>
This setup now gives me a private and secure connection I can use anywhere. <br>
 
**My Screenshots** <br>
<br>
<img width="776" height="487" alt="Screenshot 2025-12-02 at 12 29 49 AM" src="https://github.com/user-attachments/assets/639a99ed-0d1b-4a49-8fec-a2665c50413f" />
<img width="776" height="487" alt="Screenshot 2025-12-02 at 12 29 49 AM" src="https://github.com/user-attachments/assets/24de1b61-427b-402e-b171-fbb99645dfeb" />
<img width="585" height="1266" alt="IMG_8515" src="https://github.com/user-attachments/assets/afb76bd1-fc57-435e-8c23-9fd5de606fdc" />

<img width="776" height="487" alt="Screenshot 2025-12-02 at 12 29 49 AM" src="https://github.com/user-attachments/assets/13e7f731-581c-4d2a-bc2c-33964969d7c6" />

<img width="585" height="1266" alt="IMG_8516" src="https://github.com/user-attachments/assets/51933336-a570-4523-a00d-865648aaf16b" />


