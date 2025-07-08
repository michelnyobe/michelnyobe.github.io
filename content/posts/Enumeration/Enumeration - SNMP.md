`Simple Network Management Protocol` SNMP a été créé pour surveiller les périphériques réseau. Ce protocole permet également de gérer les tâches de configuration et de modifier les paramètres à distance.

Pour l'empreinte SNMP, nous pouvons utiliser des outils tels que `snmpwalk`, `onesixtyone`et `braa`. `Snmpwalk`Il permet d'interroger les OID avec leurs informations.

```bash
snmpwalk -v2c -c public 10.129.14.128

snmpwalk -v2c -c backup 10.129.109.205

```

```bash
Community:
admin
manager
public
private
backup
```


```bash
sudo apt install onesixtyone

onesixtyone -c /opt/useful/seclists/Discovery/SNMP/snmp.txt 10.129.14.128

```

utiliser avec [braa](https://github.com/mteg/braa) pour forcer les OID individuels et énumérer les informations qui se cachent derrière eux

```bash
sudo apt install braa
braa <community string>@<IP>:.1.3.6.*   # Syntax
braa public@10.129.14.128:.1.3.6.*
```