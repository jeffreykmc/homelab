# homelab
Notes for Home Lab Design

# Why
Bought a N100 PC with 6 ports. wanna to do something fun

# vi

To delete one character, position the cursor over the character to be deleted and type x . The x command also deletes the space the character occupied—when a letter is removed from the middle of a word, the remaining letters will close up, leaving no gap.


# proxmox 

## remove bannder

[]https://www.reddit.com/r/Proxmox/comments/tgojp1/removing_proxmox_subscription_notice/

## Disable auto boot
https://forum.proxmox.com/threads/is-there-a-way-to-disable-the-automatic-start-of-vms-before-proxmox-boots.83636/



# Ophsense

https://homenetworkguy.com/how-to/virtualize-opnsense-on-proxmox-as-your-primary-router/


Change. VM Script


bash -c "$(wget -qLO - https://github.com/community-scripts/ProxmoxVE/raw/main/misc/post-pve-install.sh)"


https://home.gamer.com.tw/creationDetail.php?sn=5510132



#alpine linux in LXC

~# pveam available | grep -i alpine
~# pveam download local alpine-3.19-default_20240207_amd64.tar.xz

https://www.tecmint.com/proxmox-create-container/

https://www.ichiayi.com/tech/alpine_docker

apk update
apk upgrade --available && sync
apk add vim
apk add curl
apk add docker docker-cli-compose

## install adguard home

https://github.com/AdguardTeam/AdGuardHome#getting-started


## bridge 2 network



# OpenWrt 

https://upsangel.com/openwrt/how-to-install-openwrt-on-proxmox/






