
# Virtualisation avec cokpit machines

Dans ce tutoriel, je vais vous expliquer comment faire de la Virtualisation avec QEMU et cockpit machines

## Quelques informations avant de commencer
Pour réaliser ce tutoriel, j'utilise une machine executant Ubuntu server 24.04.2, je pense que ce tutoriel fonctionne également avec Debian et ses dérivés !

Je ne vais pas revenir sur la présentation de l'interface de cockpit, pour cela, je vous invite à consulter mon tutoriel [ici](https://github.com/Aymen-Hammache/Presentation-et-installation-de-cockpit)

Je précise également que je ne peux être tenu responsable en cas de dommage éventuel sur votre matériel (même si ce risque est extrêmement faible).

## Notions techniques
*Cockpit :* interface web qui permet d’administrer facilement un serveur Linux à distance. Elle offre une vue graphique pour surveiller l’état du système, gérer les services, le réseau ou encore les utilisateurs.

*Cockpit Machines* : extension de Cockpit qui permet de gérer des machines virtuelles depuis l’interface web. Elle s’appuie sur libvirt, QEMU et KVM pour créer, démarrer ou configurer des VMs sans ligne de commande.

*QEMU* : logiciel qui permet de créer et exécuter des machines virtuelles ou d’émuler différents types de processeurs. Il est très utilisé pour tester des systèmes, simuler des architectures différentes ou faire de la virtualisation.

*KVM* : technologie intégrée au noyau Linux qui permet d’utiliser la virtualisation matérielle du processeur. Il améliore les performances des machines virtuelles en travaillant avec QEMU.

*Libvirt* : couche logicielle qui sert à gérer les machines virtuelles, les réseaux et le stockage. Elle permet à des outils comme Cockpit ou Virt-Manager de communiquer avec QEMU et KVM pour piloter les VMs.

## Installation des paquets et des dépendances nécessaires

Dans un premier temps, nous allons mettre à jour les dépôts et les paquets :

```
sudo apt update && sudo apt upgrade -y
```

Vérifiez si votre processeur supporte la virtualisation :

```
egrep -c 'vmx|svm' /proc/cpuinfo
```

Ensuite, installez cockpit et cockpit machines :

```
sudo apt install cockpit cockpit-machines -y
```

Puis, installez QEMU, KVM et libvirt :

```
sudo apt install -y qemu qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst virt-manager
```
__libvirt gère les VM, bridge-utils pour réseau, virtinst pour les créer les machines virtuelles via CLI.__

Activez et démarrez les sevices :

```
sudo systemctl enable --now cockpit.socket
sudo systemctl enable --now libvirtd
```

Integrez votre utilisateur au groupe libvirt : 

```
sudo usermod -aG libvirt votrenomdutilisateur
```
## Télécharger les fichiers iso

Tout d'abord, sachez que les images doivent être stockés dans le chemin suivant : */var/lib/libvirt/images* __si elles ne sont pas télécharger à cet emplacement, vous risquerez de rencontrer des soucis lors de la création de vos machines virtuelles__

Vous pouvez mettre les images iso via FTP avec Filezilla ou WinSCP

## Accéder à l'interface web

Ouvrez votre navigateur sur votre PC ou votre téléphone et tappez DANS LA BARRE D'ADRESSE :

```
votreadresseip:9090
```

Vous arriverez sur une page de connexion, vous avez juste à entrer le nom d'utilisateur et le mot de passe de votre session.

En haut de la page, __pensez à bien activer le mode administrateur__  (vous avez juste à re entrer le mot de passe de votre session)

Dans le menu à gauche, vous pouvez constater qu'il y a un onglet "machines virtuelles" cliquez dessus !

![Aperçu](images/aperçu.png "Menu apperçu de cockpit")

Ensuite, cliquez sur le boutton "créer une machine virtuelle"

![MV](images/Machinevirtuelles.png "Menu machines virtuelles de cockpit")

Vous verrez une fenêtre apparaître vous invitant à donner les spécificités de votre machine virtuelle.

![Conf MV](images/Créationmachinevirtuelle.png "Configuration de la  machine virtuelle")

Une fois cela fait, cliquez sur "créer nouveau et exécuter" votre VM devrait s'éxecuter et vous serez en capacité de faire de la virtualisation sur votre serveur avec cockpit !