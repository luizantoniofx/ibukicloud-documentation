# IbukiCloud Documentation | Last updated 02/Feb/2022
On this post I will be writing a little about what goes behind the infrastructure of my self-hosting project which I call IbukiCloud. It started with the curiosity but also the desire of having my own cloud storage server that's when I found NextCloud. In the beginning all I wanted was a place to have photo backups and files without depending on an external server and external cloud storage I wanted everything to be hosted at home! But of course I had some problems to deal with before achieving my goals. First of all I had to deal with port mapping and also find a safe place to host the NextCloud instance. **A place that I could freely reinstall the OS on my PC without affecting the installation**. 

## Virtualization and Friends
![My VMware Workstation Home Page](https://i.imgur.com/U19QMN5.png)
That's when I had the idea of messing around with virtualization but getting a physical hardware for the time being would be expensive and require extra time and patience. In the end I thought about getting a cheap network card and use a Type 2 Hypervisor, **VMware Workstation**. As much as I like open-source projects many people questioned me why not going with something like QEMU. The choice of VMware software came with reliability and ease of access. I use AutoProtect snapshots to avoid problems and my VMs have constant backups which happens twice every half hour. And now if you are wondering why the cheap network card? I manage all my VMs on that separate network card so my PC can even go offline without affecting the VMs (which is a big plus as I want to get a decent network switch soon) currently using a cheap one + network management of **VMware Workstation** itself.
> If you have any alternative open-source choice besides QEMU contact me and we can have some discussion together. I would love to hear other people opinions but also meet new software. 
## Operating System choice for hosted services and ports setup
> Of course this list is not permanent and there are a lot more services to come. I'm thinking about trying a mail server but after the things I heard on setting them up I don't think it will be worth the hazzle.

| Service        |Operating System               |External Port                |
|----------------|-------------------------------|-----------------------------|
|NextCloud       |`Ubuntu Server 20.04.3`        |90 (trigger,subdomain)                           |
|Minecraft,Active Directory       |`Windows Server 2012`          |42069,9999,40964 (triggers,subdomains)|
|Sonarr          |`Ubuntu Server 20.04.3`| No external port, Internal only|
|Wordpress       |`Ubuntu Server 20.04.3`        |90 (trigger,subdomain)
|Node-red        |`Ubuntu Server 20.04.3`        |1888 (trigger,subdomain)

> Reverse proxy is being used for the homepage,nextcloud,blog and node-red thus why nextcloud and wordpress have the same port. Internal ports are different across all services and they are subject to change. all internal/external ports,mac addresses,disk locations and more and managed on a spreadsheet which is saved locally on my PC,Nextcloud Instance and on my phone.

As you can see on the table above the only service running under Windows Server is Minecraft and Active Directory. Why you may ask me? Well...if I had to create a VM to mess with Active Directory why not use it for something more :p treating it like a server anyway. Ubuntu Server is my go-choice for home servers because I don't see the reason of going CentOS and totally overkill to go with Arch Linux. I always go with LTS though so I can just chill and have my services run for some time without worrying too much about maintaining the operating systems they live in :D

## Virtual Machine codenames and some explanations
![Main Temporary Page of IbukiCloud](https://i.imgur.com/yoWwyIV.png)
As you can see on the main page of ibukicloud.tk:90 (as of now) you can see every server I have with codenames. Those codenames came from many MMORPG games I've played on the past. The main server being Moltencore (where my Nextcloud,Wordpress and Minecraft instances live in) if you want to know where it came from you can read about it [here](https://classic.wowhead.com/molten-core) besides that there's not much to talk about :p the codenames only exist so I can manage the VMs easier and all of the disks and everything linked to the said VMs have the same codename set up.

## Backups they are everywhere unless when we need them 🤡
![Auto Protect Page on VMware Workstation](https://i.imgur.com/Z94hzb9.png)

All my backups are managed through VMware [AutoProtect](https://docs.vmware.com/en/VMware-Workstation-Pro/16.0/com.vmware.ws.using.doc/GUID-8EF5CFFB-E59F-429B-B497-B52CD65DB3D6.html) with the files being sent over my NextCloud instance, my local PC (spread between two hard drives) and an external disk. All to guarantee the backups are safe I plan on having off-site backups in the future but first I'd need to decide a good place to do so. Some VMs have lower backup rates because they are not as important the higher backup rate is on my Nextcloud instance 30 minutes of data can be important! While other instances like my reverse-proxy and wordpress instances 30 minutes of data is not that important leading those VMs to have hourly or even nearly daily backups instead of half-hour backups. 

## Apache sites/reverse-proxy
![Apache HTTP Server Logo](https://wiki.ixcsoft.com.br/images/0/07/Apache-http-server.png)

This is something that I began playing around recently the main idea was not exposing my internal ports anymore at least not as easy. But also allow me to have certain services only accessible through certain ports so they all seem to be running under the same port (but they aren't) that's what I'm using for Nextcloud,Wordpress and the homepage of IbukiCloud (I'm still building one with react) but the main idea is that I have everything running under random ports internally from 30-80000 and externally everything is shown as "90" besides the Minecraft servers... I wasn't able to find a way to route them properly so I used triggers so the port only opens when the servers are online then closes if the servers are offline. If someday I find a way to route Minecraft I'd probably do so and keep everything under "90" no matter what. I know how to do reverse-proxy on Nginx but I find it way overkill for my needs so Apache is my go for home hosting 😁.

## Talking about Minecraft
![Minecraft Photo](https://img.redbull.com/images/c_limit,w_1500,h_1000,f_auto,q_auto/redbullcom/2016/12/16/1331835109442_2/o-%C3%BAnico-limite-%C3%A9-a-tua-pr%C3%B3pria-imagina%C3%A7%C3%A3o.png)
One of my servers was made for a group of friends so they could play and have fun together on a server that they can call theirs. Even though I'm the one hosting it they have total freedom to choose what and when I change stuff but also the main reason I preferred to host the server is [GeyserMc](https://geysermc.org/)! Geyser is a project which together with floodgate act as a proxy so Java and Bedrock users can play together. Which means whoever is playing on PC Java Edition can play with Mobile Bedrock and even Consoles as long as they can join the server it allows for so much freedom but also a way of not locking out players just because they don't own a PC also I can install plugins and make changes to the Java Edition which is easier and more open to mods than Bedrock :D even though the server is vanilla it's good to know we can spice some things without having problems. I have 3 servers all of them run vanilla Minecraft and are mostly hosted for friends as I don't really play Minecraft.
