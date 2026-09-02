
EyeWitness peut prendre la sortie XML de Nmap et de Nessus et créer un rapport avec des captures d'écran de chaque application web présente sur les différents ports en utilisant Selenium. Il va également plus loin en catégorisant les applications lorsque c'est possible, en prenant leur empreinte (fingerprinting), et en suggérant des identifiants par défaut basés sur l'application. 

installation 

```
sudo apt install eyewitness
```

```
eyewitness --web -x web_discovery.xml -d inlanefreight_eyewitness
```