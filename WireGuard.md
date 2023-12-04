Sign up for a DigitalOcean account using [https://m.do.co/c/4d7f4ff9cfe4]

Created An Ubuntu 20.04 Droplet that is Basic, Regular Intel CPU and add a password. Then I choose the data center which was in California and entered the droplets terminal. 

then I followed instructions listed here ([https://thematrix.dev/setup-wireguard-vpn-server-with-docker/](https://thematrix.dev/setup-wireguard-vpn-server-with-docker/)) to set up a dockers project.


First Install the tools needed:
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
```

Add a Dockers key:
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Then add a dockers repo:
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

Then switch to the correct repo:
```
apt-cache policy docker-ce
```

Install dockers on it:
```
sudo apt install docker-ce -y
```

Then Install Docker-Compose: 
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

And set permissions so you can work with it:
```
sudo chmod +x /usr/local/bin/docker-compose
```

Make Directory for wireguard:
```
mkdir -p ~/wireguard/
mkdir -p ~/wireguard/config/
```
Then make yml file to store info about the dockers project:
```
version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=US/Central 
      - SERVERURL=159.223.195.135
      - SERVERPORT=51820
      - PEERS=3
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
```

Then start the Wireguard with:
```
cd ~/wireguard/
docker-compose up -d
```

Then Download the Wireguard app and scan the url codes produced by: 
```
docker-compose logs -f wireguard
```

Then I tested the vpn on my phone with IPLeak.net before connecting to the Wireguard and after.
![[IMG_0857 2.webp]]
![[IMG_0858 1.webp]]

Then I tested It on my computer by copying one of the users .conf files and then activating it and checking on IPleak.net.
![[Screenshot 2023-12-03 233247.png]]
![[Screenshot 2023-12-03 233347.png]]