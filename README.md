# Configuration et utilisation du gestionnaire de mot de passe :+1: 

**Étapes préliminaires :**

**Créez un compte utilisateur sur votre poste. Celui-ci devra disposer d'un mot de passe fort, et être membre du groupe « Administrateurs ».**

Os: Ubuntu 18.04
Utilisateur:  exploit
Mots de passe: vxqz4W9qb$O"Wa

![](https://i.imgur.com/FUfUR4d.png)

Le groupe administrateur sur Ubuntu est le groupe "root"

Nous allons rajouté notre nouvelle utilisateur à se groupe:

```
sudo adduser exploit root
```
![](https://i.imgur.com/uBYuGw5.png)

---
* Installation de keepass:

Nous installons keepass par la commande:

```
sudo apt install keepass2
```
![](https://i.imgur.com/3lEuEdE.png)



---
 **Ouvrez Keepass, puis renseignez un « Master Password » d'au moins 15 caractères variés, à l'aide d'une des deux méthodes4 indiquées en cours.**

Ont ouvre keepass et ont crée notre première base de donnée:

![](https://i.imgur.com/qIo4wjn.png)

Préalablement nous avons crée un répertoire cacher dans notre filesystem nommé .db ou seras stocké notre base de donnée.

A la suite de cela il nous est demandé de crée le mots de passe du trousseau keepass avec un politique de mots de passe correct et respectant certaines des régles établient pas L'anssi:

1. Utilisez un mot de passe unique pour chaque service
2. Choisissez un mot de passe qui n’a pas de lien avec vous
3. Ne demandez jamais à un tiers de générer pour vous un mot de passe
4. Renouvelez vos mots de passe avec une fréquence raisonnable. Tous les 90 jours est un bon compromis pour les systèmes contenant des données sensibles ;

![](https://i.imgur.com/stEZC2p.png)

Afin de constituer un mots de passe correct j'ai utilisé la méthode des premières lettre qui est un moyen mémotechnique pour se rappeler d'un mots de passe: 

"J'aime lire en hiver ça occupe lors de longue journé ennaigé " se transforme en "J'Lehc0L2lJE"

Pour rendre la sécurité plus importante il est possible de recommander un changement et de le forcer.

![](https://i.imgur.com/SFY9dLK.png)


**Utilisation de l'outil:**

* Dans Keepass, enregistrez les deux comptes suivants:
◦ le compte Administrateur que vous venez de créer, dans la section « Windows ». Ce compte vous servira à démarrer une application avec une élévation de privilège, cmd.exe par exemple.
◦ votre compte YNOV dans la section "email"

![](https://i.imgur.com/ZZ5iRmY.png)

Ensuite ajoutons nos informations.

![](https://i.imgur.com/QA7Puls.png)

Ont créer ensuite une entrée pour YNOV via l'onglet email:

![](https://i.imgur.com/MaqzngP.png)

* Connectez vous ensuite, à l'aide de ces deux comptes, des deux façons suivantes:
◦ d'abord en copiant le mot de passe dans le clipboard, via l'option dans Keepass, puis
◦ en utilisant la fonctionnalité auto-type.

Copier dans le clipboard:

![](https://i.imgur.com/vKtN4cd.png)

Utiliser la fonctionnalité d'autotype:

![](https://i.imgur.com/vTyLviB.png)

---

**Fonctionnalités annexes du gestionnaire de mot de passe :**
Générateur de mots de passe :
  Keepass vous permet également de générer des mots de passe. Vous modifierez donc le « Master Password » de votre database, en utilisant le générateur de mots de passe de Keepass. Vous respecterez les contraintes de complexité imposées précédemment pour la génération de ce mot de passe.

Utilisation de la génération automatique de mots de passe fort de keepass:

![](https://i.imgur.com/hmHvx1s.png)

**Tâches automatisées :**

◦ Expliquez la fonctionnalité « Triggers » intégrée à Keepass.

La fonctionnalité Triggers ou déclancheur va réagir à un évènement tel que la fermeture de l'application keepass pour y executer un backup de la base ou autre.

◦ Vous créerez alors un Trigger qui permettra de sauvegarder la base automatiquement à la fermeture de Keepass. Ce Trigger ne devra conserver que 3 exemplaires de la base de données dans son historique.

Nous allons ajouter un triggers:

![](https://i.imgur.com/j6Yn85T.png)

![](https://i.imgur.com/uGpfUlC.png)

Puis le paramètrer:

Ont le nomme puis on rajoute l'évènement à la fermeture de l'application "Application exit":

![](https://i.imgur.com/Hr8vNwH.png)

Puis nous allons paramétrer les actions:

![](https://i.imgur.com/oY4bEjv.png)

D'abord une sauvegarde 

Ensuite un simple command de copie dans un répertoire de backup:

```
cp /home/exploit/Bureau/.db/NewDatabase.kdbx /home/exploit/Bureau/.db/backup/NewDatabase.kdbx.{DT.SIMPLE}
```
![](https://i.imgur.com/8dbgnf0.png)

Il était ensuite demandé de faire une sauvegarde incrémentiel et de garder les 3 dernières version de la base et logger les erreurs:

```
sh -c "cd /home/exploit/Bureau/.db/backup && ls -t | tail -n +4 | xargs rm -- >> /var/log/keepass.log 2>&1"
```
![](https://i.imgur.com/6uEaVAO.png)

---
**Sécurisation de la solution :**

Une fois la solution mise en œuvre, il convient de sécuriser son utilisation. Vous mettrez donc en œuvre les deux mesures de sécurité suivantes :

◦ vous « imprimerez » la feuille « d'urgence », qui permettra de conserver le « Master Password » en lieu sûr.

![](https://i.imgur.com/DAAWPLX.png)

Cela génére un fichier .html à conserver en sécurité et à remplir soit même.

![](https://i.imgur.com/yADcYZo.png)

◦ Vous créerez ensuite un script avec Robocopy5 qui permettra de sauvegarder la base de données.

Pour mon cas j'utilise un système ubuntu j'utilise alors la commande "cp":

```
#!/bin/bash
cp /home/exploit/Bureau/.db/NewDatabase.kdbx /path/to/your/security/folder/NewDatabase.kdbx
```



