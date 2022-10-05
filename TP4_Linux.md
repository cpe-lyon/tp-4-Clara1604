20 septembre 2022

Clara Salivet - 3ICS

# TP4 - Gestion des paquets

## Exercice 1 : commande de base

### 1. Commencez par mettre à jour votre système avec les commandes vues dans le cours.
```bash
sudo apt update
sudo apt upgrade
```
### 2. Créez un alias “maj” de la ou des commande(s) de la question précédente. Où faut-il enregistrer cet alias pour qu’il ne soit pas perdu au prochain redémarrage ?
il faut l'enregistrer dans le fichier .bashrc
```bash
alias maj='sudo apt update ; sudo apt upgrade' #dans le fichier .bashrc
source ~/.bashrc # pour redemarer
```
### 3. Utilisez le fichier /var/log/dpkg.log pour obtenir les 5 derniers paquets installés sur votre machine.
```bash
tail -5 /var/log/dpkg.log
```
### 4. Listez les derniers paquets qui ont été installés explicitement avec la commande apt install

```bash
apt list -i
```
### 5. Utilisez les commandes dpkg et apt pour compter de deux manières différentes le nombre de total de paquets installés sur la machine (ne pas hésiter à consulter le manuel !). Comment explique-t-on la (petite) différence de comptage ? Pourquoi ne peut-on pas utiliser directement le fichier dpkg.log ?
```bash
apt list -i | wc -l
dpkg -l | wc -l
```
### 6. Combien de paquets sont disponibles en téléchargement sur les dépôts Ubuntu ?
```bash
apt list | wc -l
```
on obtient 68744.

### 7. A quoi servent les paquets glances, tldr et hollywood ? Installez-les et testez-les.
Ces différents programmes sont des simulations de genres de centres de controle façon matrix.
```bash
sudo apt install
```
### 8. Quels paquets proposent de jouer au sudoku ?
```bash
search sodoku
```

## Exercice 2 

### A partir de quel paquet est installée la commande ls ? Comment obtenir cette information en une seule commande, pour n’importe quel programme ? Utilisez la réponse à cette question pour écrire un script appelé origine-commande (sans l’extension .sh) prenant en argument le nom d’une commande, et indiquant quel paquet l’a installée.

```bash
which -a ls | tail -n 1 | xargs dpkg -S
# dpkg prends de argument, on converti la sortie de la commande which en argument pour que dpkg puisse la lire : xargs
2>/dev/null # redirige les erreurs et n'affuche que la bonne réponse 

which -a ls | xargs dpkg -S 2>/dev/null | cut -f1 -d:

```
paquet : coreutils

le programme :
```bash
#!/bin/bash
which -a ls | xargs dpkg -S 2>/dev/null | cut -f1 -d:
```

## Exercice 3

### Ecrire une commande qui aﬀiche “INSTALLÉ” ou “NON INSTALLÉ” selon le nom et le statut du package spécifié dans cette commande.

```bash
dpkg -l coreutils | grep "ii" 
grep # filtre les lignes qui commencent par ii


dpkg -l coreutils 2>/dev/null | grep "ii" > /dev/null && echo "INSTALLé" || echo "Pas installé"
```

1 && 2 => la commande 2 est executé que si la cde 1 est un succès
1 || 2 => la cde 2 est executé que si la cde 1 est un échec

## Exercice 4 

### Lister les programmes livrés avec coreutils. En particulier, on remarque que l’un deux se nomme [. De quoi s’agit-il ?


## Exercice 5 : aptitude 

### Installez les paquets emacs et lynx à l’aide de la version graphique d’aptitude (et prenez deux minutes pour vous renseigner et tester ces paquets).

## Exercice 6 : Installation d'un paquet par PPA

### Certains logiciels ne figurent pas dans les dépôts oﬀiciels. C’est le cas par exemple de la version ”oﬀicielle” de Java depuis qu’elle est développée par Oracle. Dans ces cas, on peut parfois se tourner vers un ”dépôt personnel” ou PPA.
### 1. Installer la version Oracle de Java (avec l’ajout des PPA)
        ### sudo add-apt-repository ppa:linuxuprising/java
        ### sudo apt update
        ### sudo apt install oracle-java15-Installer
### 2. Vérifiez qu’un nouveau fichier a été créé dans /etc/apt/sources.list.d. Que contient-il ?

## Exercice 7 : Installation d'un logiciel à partir du code source

### Lorsqu’un logiciel n’est disponible ni dans les dépôts oﬀiciels, ni dans un PPA, ou encore parce qu’on souhaite n’installer qu’une partie de ses fonctionnalités, on peut se tourner vers la compilation du code source. C’est ce que nous allons faire ici, avec le programme cbonsai (https://gitlab.com/jallbrit/cbonsai)

### 1. Commencez par cloner le dépôt git suivant : git clone https://gitlab.com/jallbrit/cbonsai Ceci permet de récupérer en local le code source du logiciel cbonsai.
### 2. Rendez vous dans le dossier cbonsai. Un fichier README.md) est livré avec les sources, et vous explique comment compiler le programme (vous pouvez installer un lecteur de Markdown pour Bash, comme mdless pour vous faciliter la lecture de ce type de fichiers).
### Un fichier Makefile est également présent. Un Makefile est un fichier utilisé par l’outil make, et contient toutes les directives de compilation d’un logiciel. Un Makefile définit un certain nombre de règles permettant de construire des cibles. Les cibles les plus communes étant install (pour la compilation et l’installation du logiciel) et clean (pour sa suppression). En suivant les consignes du fichier README.md, et en installant les éventuels paquets manquants, compilez ce programme et installez le en local.
### 3. Malheureusement, cette installation “à la main” fait qu’on ne dispose pas des bénéfices de la gestion de paquets apportés par dpkg ou apt. Heureusement, il est possible de transformer un logiciel installé “à la main” en un paquet, et de le gérer ensuite avec apt ; c’est ce que permet par exemple l’outil checkinstall.
### 4. Recommencez la compilation à l’aide de checkinstall :
```bash sudo checkinstall ```

## Exercice 8 : Création de dépot personnalisé