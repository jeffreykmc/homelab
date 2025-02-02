# homelab
Notes for Home Lab Design

# Why
Bought a N100 PC with 6 ports. wanna to do something fun

# vi

To delete one character, position the cursor over the character to be deleted and type x . The x command also deletes the space the character occupied—when a letter is removed from the middle of a word, the remaining letters will close up, leaving no gap.


# proxmox 

## remove bannder

https://www.reddit.com/r/Proxmox/comments/tgojp1/removing_proxmox_subscription_notice/

## Disable auto boot
https://forum.proxmox.com/threads/is-there-a-way-to-disable-the-automatic-start-of-vms-before-proxmox-boots.83636/

    vi /etc/pve/qemu-server/VMID.conf
    or
    vi /etc/pve/qemu-server/VMID.conf

# Opnsense

https://homenetworkguy.com/how-to/virtualize-opnsense-on-proxmox-as-your-primary-router/


Change VM Script in opnsense shell

    bash -c "$(wget -qLO - https://github.com/community-scripts/ProxmoxVE/raw/main/misc/post-pve-install.sh)"


https://home.gamer.com.tw/creationDetail.php?sn=5510132



# alpine linux in LXC

## install
    pveam available | grep -i alpine
    pveam download local alpine-3.19-default_20240207_amd64.tar.xz

https://www.tecmint.com/proxmox-create-container/

https://www.ichiayi.com/tech/alpine_docker

## post-install

    apk update
    apk upgrade --available && sync
    apk add vim
    apk add curl
    apk add docker docker-cli-compose

## install ollema

    curl -fsSL https://ollama.com/install.sh | sh
    ollama run deepseek-r1:671b

## install adguard home

https://github.com/AdguardTeam/AdGuardHome#getting-started


## bridge 2 network


# OpenWrt 

https://upsangel.com/openwrt/how-to-install-openwrt-on-proxmox/

## Install Method

### one line install

    bash -c "$(wget -qLO - https://github.com/tteck/Proxmox/raw/main/vm/openwrt.sh)"

### Containers

Youtube: https://www.youtube.com/watch?v=3mPbrunpjpk

a. Images:
    https://images.linuxcontainers.org/images


    1. copy the [rootfs.tar.xz](https://youtu.be/3mPbrunpjpk?si=UI_H6Qz94A3rK6Y6&t=164)
    2. goback to shell
        pct create 107 ./rootfs.tar.xz --unprivileged 1 --ostype unmanaged --hostname openwrt --net0 name=eth0 --net1 name=eth1 --storage app-data

    3. vi /etc/pve/lxc/10x.conf
        lxc.cgroup2.devices.allow: c 10:200 rwm
        lxc.mount.entry: /dev/net dev/net none bind,create=dir
    
    4. Update Interface

    5. Reboot

    6. install OpenVPN OpenVPN-luci

    7. setup OpenVPN

## for my setup

    Virtual Machine ID: 103
    Using Hostname: openwrt
    Allocated Cores: 2
    Allocated RAM: 256
    Using WAN Bridge: vmbr1
    Using LAN Bridge: vmbr2
    Using LAN IP ADDRESS: 192.168.1.1
    Using LAN NETMASK: 255.255.255.0
    Using WAN MAC Address: [MAC1]
    Using LAN MAC Address: [MAC2]
    Using WAN Vlan: Default
    Using LAN Vlan: 999
    Using Interface MTU Size: Default
    Start VM when completed: yes

# Ollama

    sudo systemctl stop ollama.service
    sudo systemctl status ollama.service
## Install Ollama

    curl -fsSL https://ollama.com/install.sh | sh

    ollama pull llama2
    ollama server
    ollama 

## Install webui

    git clone https://github.com/ollama-webui/ollama-webui
    cd ollama-webui
    docker compose up -d

## when there is error error-saving-credentials-error-storing-credentials-err-exit-status-1-out

    cd $HOME/.docker/ && mv config.json config.json.bak && rm config.json




## Install using docker

    docker run -d --name ollama -p 11434:11434 -v ollama_storage:/root/.ollama \
  ollama/ollama:latest

# pFsense

# NGINX

# authentik


# Vaultwarden
https://github.com/dani-garcia/vaultwarden



# Pterodactyl

## install lxc ubuntu

    apt update
    apt upgrade
    apt install vim curl

## Install Pterodactyl
https://pterodactyl.io/panel/1.0/getting_started.html

### Add "add-apt-repository" command
    apt -y install software-properties-common curl apt-transport-https ca-certificates gnupg

### Add additional repositories for PHP (Ubuntu 20.04 and Ubuntu 22.04)
    LC_ALL=C.UTF-8 add-apt-repository -y ppa:ondrej/php

### Add Redis official APT repository
    curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg
    echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list

### MariaDB repo setup script (Ubuntu 20.04)
    curl -LsS https://r.mariadb.com/downloads/mariadb_repo_setup | sudo bash

### Update repositories list
    apt update

### Install Dependencies
    apt -y install php8.3 php8.3-{common,cli,gd,mysql,mbstring,bcmath,xml,fpm,curl,zip} mariadb-server nginx tar unzip git redis-server


## MariaDB

### If using MariaDB (v11.0.0+) (This is the default when installing Pterodactyl by following the documentation.)
    mariadb -u root -p

### Remember to change 'yourPassword' below to be a unique password
    CREATE USER 'pterodactyl'@'127.0.0.1' IDENTIFIED BY 'yourpassword';
    CREATE DATABASE panel;
    GRANT ALL PRIVILEGES ON panel.* TO 'pterodactyl'@'127.0.0.1' WITH GRANT OPTION;
    exit

### /var/www/pterodactyl#
    cp .env.example .env
    COMPOSER_ALLOW_SUPERUSER=1 composer install --no-dev --optimize-autoloader

### Only run the command below if you are installing this Panel for
### the first time and do not have any Pterodactyl Panel data in the database.
    php artisan key:generate --force

### Environment Configuration
    php artisan p:environment:setup
    php artisan p:environment:database

### To use PHP's internal mail sending (not recommended), select "mail". To use a custom SMTP server, select "smtp".
    php artisan p:environment:mail

### Database Setup
    php artisan migrate --seed --force

### Add The First User
    php artisan p:user:make

### Crontab Configuration

    crontab -e


    * * * php /var/www/pterodactyl/artisan schedule:run >> /dev/null 2>&1

### Create a file called pteroq.service in /etc/systemd/system with the contents below.
    # Pterodactyl Queue Worker File
    # ----------------------------------

    [Unit]
    Description=Pterodactyl Queue Worker
    After=redis-server.service

    [Service]
    # On some systems the user and group might be different.
    # Some systems use `apache` or `nginx` as the user and group.
    User=www-data
    Group=www-data
    Restart=always
    ExecStart=/usr/bin/php /var/www/pterodactyl/artisan queue:work --queue=high,standard,low --sleep=3 --tries=3
    StartLimitInterval=180
    StartLimitBurst=30
    RestartSec=5s

    [Install]
    WantedBy=multi-user.target



### enable redis-server
    sudo systemctl enable --now redis-server



