## Journalisation

Il est essentiel que nous journalisions toutes les tentatives de scan et d'attaque et que nous conservions les sorties brutes des outils autant que possible. Cela nous aidera grandement au moment de la rédaction du rapport.

https://github.com/tmux-plugins/tmux-logging

ctiver/désactiver la journalisation dans le panneau actuel.

- Raccourci clavier :`prefix + shift + p`
- Format du nom de fichier :`tmux-#{session_name}-#{window_index}-#{pane_index}-%Y%m%dT%H%M%S.log`
- Chemin d'accès au fichier : `$HOME`(répertoire personnel de l'utilisateur)
    - Exemple de fichier :`~/tmux-my-session-0-1-20140527T165614.log`


## Installation 

Tout d'abord, clonez le dépôt du gestionnaire de plugins Tmux  dans notre répertoire personnel (dans notre cas `/home/htb-student` ou simplement `~`).

```
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

Ensuite, créez un fichier `.tmux.conf` dans le répertoire personnel.

```
touch .tmux.conf
```

Le fichier de configuration doit avoir le contenu suivant :

```
cat .tmux.conf 
# List of plugins 
set -g @plugin 'tmux-plugins/tpm' 
set -g @plugin 'tmux-plugins/tmux-sensible' 
set -g @plugin 'tmux-plugins/tmux-logging'

# Initialize TMUX plugin manager (keep at bottom) 
run '~/.tmux/plugins/tpm/tpm'
```

Après avoir créé ce fichier de configuration, nous devons l'exécuter dans notre session actuelle pour que les paramètres du fichier `.tmux.conf` prennent effet.

```
tmux source ~/.tmux.conf
```

Ensuite, nous pouvons démarrer une nouvelle session Tmux

Une fois dans la session, tapez `[Ctrl] + [B]` puis `[Shift] + [I]`  pour installer le plugin 
Une fois le plugin installé, commencez à journaliser la session (ou le volet) actuelle en tapant `[Ctrl] + [B]` suivi de `[Shift] + [P]` 
la combinaison de touches `prefix` + `[Shift] + [P]` ou tapez `exit` pour tuer la session

Une fois la journalisation terminée, vous pouvez retrouver toutes les commandes et leurs sorties dans le fichier journal associé.

Si nous oublions d'activer la journalisation Tmux et que nous sommes déjà bien avancés dans un projet, nous pouvons effectuer une journalisation rétroactive en tapant `[Ctrl] + [B]` puis en appuyant sur `[Alt] + [Shift] + [P]` (`prefix` + `[Alt] + [Shift] + [P]`), et tout le volet sera sauvegardé.

Pour se prémunir contre cette situation, nous pouvons ajouter les lignes suivantes au fichier `.tmux.conf` (en ajustant le nombre de lignes à notre convenance) :

```
set -g history-limit 50000
```

Si nous essayons de copier/coller la sortie d'un volet, nous récupérerons également des données de l'autre volet, ce qui rendra la sortie très désordonnée et nécessitera un nettoyage. Nous pouvons éviter cela en faisant une capture d'écran comme suit : `[Ctrl] + [B]` suivi de `[Alt] + [P]` (`prefix` + `[Alt] + [P]`)

https://github.com/tmux-plugins/list