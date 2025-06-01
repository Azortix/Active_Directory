# Procédure pour la quête de création de serveur de fichiers

### Prérequis
Nous avons deja le service de serveur de fichier installé sur un Wiondows Server 2022 en anglais.

### Création des groupes AD
On créé les groupes AD ``RH``, ``Comptabilité`` et ``Direction``.

### Mise en place du partage du dossier ``Documents_entreprise``
On créé d'abord le dossier ``Documents_entreprise`` sur la racine C: puis les sous dossiers ``RH`` ``Comptabilité`` ``Direction``.  
Dans le module sur le Server Manager "Files and Storage service", on va créer un nouveau partage via click droit, New Share.  
On choisit SMB SHare Quick, Next  
On choisit Type a custom path, puis on entre le path correspondant au dossier ``Documents_entreprise``  
Comme Share Name, on choisit Docs  
A l'étape de Permissions, on vérifie bien que les utilisateurs du domaine aient un accès en lecture seule, si ce n'est pas le cas,
il suffit de faire Customize permissions, puis Add, puis Select a principal pour choisir le groupe ``Domain Users``
Puis on continue jusqu'à complétion.

### Mise en place du partage des sous dossiers ``RH``, ``Comptabilité`` et ``Direction``
Dans le module sur le Server Manager "Files and Storage service", on va créer un nouveau partage via click droit, New Share.  
On choisit SMB SHare Quick, Next  
On choisit Type a custom path, puis on entre le path correspondant au dossier voulu : ``RH``  
Comme Share Name, on choisit ``RH``  
Pour ce qui est de sPermissions, on va cliquer sur Customize permissions, puis sur Add, puis Select a principal pour choisir le groupe ``RH``
On laisse les permissions en Read, Write, Execute et on clique sur OK  
On supprime les lignes d'octroi de permissions des personnes hors Administration, RH ou Direction pour ne permettre l'accès qu'a des personnes habilitées, puis OK  
Next, Create, CLose  
On répétera l'action avec ``Comptabilité`` et ``Direction``  


### COnfiguration du lecteur réseau sur le client
Il est possible de faire cette étape par ssh (étant donné qu'il s'agit d'une commande powershell), mais dans mon cas, je vais simplement utiliser un client sur le même réseau interne.


On lance la commande suivante sur powershell  
``New-PSDrive -Name "X" -PSProvider FileSystem -Root "\\SRVWIN\Documents_entreprise\Département" -Persist``  
Avec X la lettre du lecteur voulu (qui est disponible), SRVWIN étant le nom de la machine Windows Server 2022, et Département étant le nom du dossier
du département voulu (RH, Comptabilité et Direction).  


### Vérifications
#### Partages
Sur Powershell, on tape la commande ``Get-SmbShare`` pour lister tous les partages sur le serveur
#### Mapping
Sur Powershell du client, on tape la commande ``Get-PSDrive -PSProvider FileSystem`` pour lister tous les mapping de lecteur 
