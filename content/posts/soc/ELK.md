---
title: "ELK"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
--- 
je travaille sur unbuntu server  Ubuntu 24.04.3 LTS  https://ubuntu.com/download/server (4g RAM 2 CPU)
windows server 2022  microsoft.com/en-us/evalcenter/download-windows-server-2022
et window 10 

# installer d'ubuntu 

installation sur vmware 

![[Pasted image 20251219144300.png]]

 ![[Pasted image 20251219144713.png]]


![[Pasted image 20251219145107.png]]

# installation de elk

Pour commencer, utilisez cURL, l'outil en ligne de commande permettant de transférer des données via des URL, afin d'importer la clé publique GPG d'Elasticsearch dans APT

```
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

Ensuite, ajoutez la liste des sources Elastic au `sources.list.d`répertoire dans lequel APT recherchera les nouvelles sources :

```
echo "deb https://artifacts.elastic.co/packages/9.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
```

```
sudo apt update

sudo apt install elasticsearch
 sudo systemctl start elasticsearch.service
test@test:~$ sudo systemctl status elasticsearch.service
sudo nano /etc/elasticsearch/elasticsearch.yml
```


modifier le fichier de configuration d’Elasticsearch

![[Pasted image 20251219150143.png]]


#bootstrap.memory_lock: true
```
#
# Make sure that the heap size is set to about half the memory available
# on the system and that the owner of the process is allowed to use this
# limit.
#
# Elasticsearch performs poorly when the system is swapping the memory.
#
# ---------------------------------- Network -----------------------------------
#
# By default Elasticsearch is only accessible on localhost. Set a different
# address here to expose this node on the network:
#
network.host: localhost
#
# By default Elasticsearch listens for HTTP traffic on the first free port it
# finds starting at 9200. Set a specific HTTP port here:
#
#http.port: 9200
#
# For more information, consult the network module documentation.
```

```
sudo systemctl restart elasticsearch
```

```
sudo systemctl enable elasticsearch
```

```
 curl -X GET "localhost:9200"
```

![[Pasted image 20251219150441.png]]

# installation de kibana 

```
sudo apt install kibana
```

Activez ensuite le service Kibana et démarrez-le

```
sudo systemctl enable kibana
sudo systemctl start kibana
```

installer ngnix 

```
sudo apt update
sudo apt install nginx
```

La commande suivante créera l'utilisateur et le mot de passe d'administrateur Kibana et les enregistrera dans un `htpasswd.users`fichier.

```
echo "kibanaadmin:`openssl passwd -apr1`" | sudo tee -a /etc/nginx/htpasswd.users
```





![[Pasted image 20251219150552.png]]




https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-18-04-fr