

# labo-infra-reseau-2023
TP 18/10/2023


## 🎯 Quels sont les trois différents types de cloud computing disponibles ?
Les trois principaux types de cloud computing sont les suivants :

Cloud Public (Cloud public) : Dans un cloud public, les ressources informatiques sont mises à la disposition du public par un fournisseur de services cloud tiers. Ces ressources sont partagées entre de nombreux utilisateurs, ce qui les rend plus économiques et évolutives. Les utilisateurs paient généralement en fonction de leur utilisation. Exemples de fournisseurs de cloud public : Amazon Web Services (AWS), Microsoft Azure, Google Cloud Platform (GCP).

Cloud Privé (Cloud privé) : Un cloud privé est une infrastructure cloud dédiée à une seule organisation. Les ressources d'un cloud privé peuvent être hébergées sur site ou dans un centre de données tiers, mais elles sont utilisées exclusivement par l'organisation qui les possède. Les clouds privés offrent un plus grand contrôle sur la sécurité et la personnalisation, mais ils sont généralement plus coûteux que les clouds publics.

Cloud Hybride (Cloud hybride) : Le cloud hybride combine à la fois des environnements de cloud public et privé, permettant aux données et aux applications de fonctionner de manière transparente entre les deux. Cela offre une plus grande flexibilité, car les organisations peuvent tirer parti des avantages de l'extensibilité et de la rentabilité du cloud public tout en conservant un contrôle plus étroit sur les données sensibles dans un cloud privé. Le cloud hybride est particulièrement utile pour les entreprises qui ont des besoins variés en matière de traitement des données.


Créez deux machines virtuelles sur votre ordinateur avec votre hyperviseur de choix. Nommez-les respectivement landing-vm1 et landing-vm2.
Utilisez une image Rocky Linux pour l'installation.



## Donnez-leur des cartes réseau NAT, et ⏹️ associez-leur les adresses IP 172.16.64.2 et 172.16.64.3.

Utiliser la commande nmtui
```nmtui```
Et rentre les informations
![Alt text](image.png)


## Changez le nom d'hôte des machines pour avoir respectivement vm-landing1 et vm-landing2

Pour la machine 1   
```hostnamectl set-hostname vm-landing1```    
Pour la machine 2   
```hostnamectl set-hostname vm-landing2```  

Penser a reboot les deux machines afin d'appliquer le nouveau hostname   

Après avoir redémarré, vous devriez avoir un nouveau hostname :   

![Alt text](image-1.png)

![Alt text](image-2.png)

## 🎰 Trouvez l'adresse IP locale des machines + ip + broadcast

__Machine 1__

```[root@vm-landing1 ~]# ip a```
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state   UNKNOWN group default qlen 1000  
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00  
    inet 127.0.0.1/8 scope host lo  
       valid_lft forever preferred_lft forever  
    inet6 ::1/128 scope host  
       valid_lft forever preferred_lft forever  
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc   fq_codel state UP group default qlen 1000
    link/ether 00:0c:29:f4:bc:f4 brd ff:ff:ff:ff:ff:ff   */// <- ADDRESS MAC *
    altname enp2s1
    inet 172.16.64.2/24 brd 172.16.64.255 scope global    */// <- ADDRESS IP  + brd = BROADCAST*
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fef4:bcf4/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```
__Machine 2__

```[root@vm-landing1 ~]# ip a```
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state   UNKNOWN group default qlen 1000  
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00    
    inet 127.0.0.1/8 scope host lo      
       valid_lft forever preferred_lft forever   
    inet6 ::1/128 scope host    
       valid_lft forever preferred_lft forever    
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc   fq_codel state UP group default qlen 1000  
    link/ether 00:0c:29:92:83:82 brd ff:ff:ff:ff:ff:ff   */// <- ADDRESS MAC *  
    altname enp2s1  
    inet 172.16.64.3/24 brd 172.16.64.255 scope global    */// <- ADDRESS IP  + brd = BROADCAST*  
       valid_lft forever preferred_lft forever  
    inet6 fe80::20c:29ff:fef4:bcf4/64 scope link noprefixroute  
       valid_lft forever preferred_lft forever  
```


## 🎰 Pingez l'adresse publique du site www.ynov.com avec une des deux machines

```ping www.ynov.com```

```PING www.ynov.com (172.67.74.226) 56(84) bytes of data.
  64 bytes from 172.67.74.226 (172.67.74.226): icmp_seq=1 ttl=128 time=14.5 ms  
  64 bytes from 172.67.74.226 (172.67.74.226): icmp_seq=2 ttl=128 time=16.4 ms  
  64 bytes from 172.67.74.226 (172.67.74.226): icmp_seq=3 ttl=128 time=17.8 ms  
  64 bytes from 172.67.74.226 (172.67.74.226): icmp_seq=4 ttl=128 time=19.8 ms  
  --- www.ynov.com ping statistics ---  
  4 packets  transmitted, 4 received, 0% packet loss, time 3005ms  
  rtt min/avg/max/mdev = 14.465/17.123/19.785/1.948 ms
```

---------------------------------------------------------
__Sur chaque machine, créez respectivement un utilisateur labo-user1 et labo-user2.
Changez son mot de passe, puis de compte et connectez-vous avec l'utilisateur labo-1__
 ---------------------------------------------------------   


## 🎰 Essayez de lire le contenu du fichier /var/log/tallylog

```[labo-user1@vm-landing1 ~]$ cat /var/log/tallylog```
```
cat: /var/log/tallylog: Permission denied
```
## 🎯 Vous remarquerez un problème. Pourquoi obtenez-vous cette erreur ?

![Alt text](image-4.png)

Le user labo-user1 n'a pas les droit afin de lire le fichier *tallylog*  
Ajoutez ces utilisateurs au groupe wheel et retentez

🎯 Il n'y aura plus d'erreur. Pourquoi ?
L'erreur n'est plus la car l'utilisateur a spécifié qui souhaite exécuter cette commande en temps que su

Retentez en changeant d'utilisateur et en passant root
🎯 Ca devrait marcher. Pourquoi ?

Root possède les droits de super utilisateur

Ajoutez l'utilisateur labo-user1 au sudoers (petit coup de Google), puis retentez la commande sudo cat
🎯 Ca devrait marcher. Pourquoi ?

Suite à l'ajout de labo-user1 au groupe sudoers, il devient superuser sur la machine. 




Sur la machine landing-vm1 :

Installez les paquets sl, dnsmasq et htop. 

🎰 Vérifiez leur version
```sl -version``  


```htop -v```     

```
htop 3.2.2
```

```dnsmasq -v```  

```
Dnsmasq version 2.85  Copyright (c) 2000-2021 Simon Kelley   
Compile time options: IPv6 GNU-getopt DBus no-UBus no-i18n IDN2 
DHCP DHCPv6 no-Lua TFTP no-conntrack ipset auth cryptohash   DNSSEC loop-detect inotify dumpfile

This software comes with ABSOLUTELY NO WARRANTY.  
Dnsmasq is free software, and you are welcome to redistribute it
under the terms of the GNU General Public License, version 2 or 3.
```