# Devoxx 2022

## (Re) Découvrir les outils UNIX

[Deck](https://speakerdeck.com/lyrixx/re-decouvrir-les-outils-unix)
[Vidéo](https://www.youtube.com/watch?v=Z9DUR4IBR94)

.bashrc a la racine de la home

Ce fichier nous appartient

Permet de définir des alias, de changer de prompts, changer les couleurs

File Descriptor Unix

### Commandes :

#### tree : illustre sois forme d’arbre le répertoire commun

#### namei -ol /path/to/file

#### cut -d “;” : extraire 3eme colonne d’un csv.

#### awk : récupérer la colonne dans une suite de texte

#### grep : recherche

#### xargs : transposer quelque chose en colonne vers ligne

#### dd : génère des fichiers de la taille qu’on veut

#### watch : commande qui execute périodiquement une autre commande

#### timeout : stopper un processus près x temps

#### Ctrl + Z : met en background une commande

#### ssh -TL 1234:192.168.2.206:9002 \ [my-server.io](http://my-server.io) : ouverture d’un tunnel ssh

#### lsof :

- Liste les fichiers qui son ouvert par un autre programme
- Liste les processus qui utilise un port

#### strace :

- permet d’isoler tous les appels effectués au kernel
- permet de savoir où est les dossiers d'installation d’un soft

#### tcpdump : permet de profiler des appels réseaux
