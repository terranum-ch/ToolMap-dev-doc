# Ressources

## Subversion

Le code source de ToolMap est stocké dans un dépôt Subversion. Il est accessible à l'adresse : <http://svn.crealp.ch/svn/toolmap2>. 
Pour le télécharger, la commande suivante peut être utilisée: 
    
    svn co 
    http://svn.crealp.ch/svn/toolmap2/trunk 
    D:\DEV\ToolMap\trunk

Un compte avec les droits d'accès suffisant est nécessaire pour pouvoir lire et écrire dans ce dépôt.

### Création d'un compte 

Créer un nom d'utilisateur ainsi qu'un mot de passe avec le formulaire de la page: <https://213.221.129.20/apr-htpasswd.php>. Copier et insérer la chaîne d'identification dans le fichier /data/subversion/crealp/conf/utilisateurs.txt du serveur web du CREALP. 

Puis ajouter le nom d'utilisateur ainsi que les droits désiré dans le fichier /data/subversion/crealp/conf/authz. 

## TRAC

L'outil TRAC est utilisé pour le suivi des bugs ainsi que pour les demandes de nouvelles fonctionnalités. Il est disponible à l'adresse <http://trac.crealp.ch/toolmap2>

Un compte est nécessaire afin de pouvoir créer des tickets. En effet nous avons considéré qu'il était plus productif d'entrer nous même les tickets plutôt que de laisser les utilisateurs s'en charger. Lorsqu'un utilisateur fait remonter un problème ou un désir (par email, de vive voix, etc.) nous ajoutons le problème comme nouveau ticket. En fonction de son importance nous l'assignons à la prochaine milestone ou à une version ultérieure.

Cette manière de faire est plus contraignante que la méthode habituelle permettant aux utilisateurs d'accéder directement à TRAC mais présente les avantages suivants : 

  * Tickets plus détaillés
  * Style rédactionnel cohérant
  * Absence de doublons

### Création d'un compte

Créer un nom d'utilisateur ainsi qu'un mot de passe avec le formulaire disponible à l'adresse : <https://213.221.129.20/trac-htpasswd.php> Copier et insérer la chaîne d'identification dans le fichier /data/trac/crealp/conf/utilisateurs.txt 

L'utilisateur est alors enregistré et peut créer des tickets. Pour accéder au niveau administrateur, l'utilisateur peut être ajouté par un autre administrateur en utilisant le panneau Admin. de TRAC ou alors si il n'existe pas d'autre administrateur en utilisant la commande : 

    $ trac-admin
    /data/trac/crealp/projects/toolmap2 
    permission add nom_utilisateur TRAC_ADMIN`


## Outils

Pour pouvoir développer ToolMap un certain nombre d'outils doivent être installés. Certains outils sont communs à toutes les plates-formes (Windows, OSX, Linux). D'autres sont spécifiques à une plate-forme. 

Nous avons choisi d'utiliser le plus possible l'environnement de développement recommandé pour chaque plate-forme afin d'obtenir les meilleures performances possible. Les quelques tests menés au début du projet en utilisant par exemple MinGW sous Windows montraient des exécutables beaucoup plus gros et plus lent que ceux produits par Visual Studio. Ces tests datant de 2008 il serait intéressant de voir dans quelle mesure les résultats obtenus alors sont toujours pertinents.

### Outils communs

#### CMake
Le logiciel CMake téléchargeable gratuitement à l'adresse : <http://www.cmake.org> permet de créer, depuis un fichier unique, des projets pour les différents environnements de développements. Après installation, la commande `cmake --version` doit fonctionner et retourner le numéro de version installée. la commande `cmake --help` permet de lister les différents types de projets que CMake peut générer.

#### Python
Python est utilisé pour faciliter le développement de ToolMap. Des scripts ont été développé  (dans le répertoire trunk/build/build-script) pour automatiser la création d’exécutable de ToolMap. Il est nécessaire d'installer une version 3.X de Python. Si d'autres version de Python sont présentes sur le système ce n'est pas un problème par contre il faudra lancer les scripts avec la version 3 exclusivement. 

#### wxFormBuilder
Disponible gratuitement à l'adresse suivante : <http://sourceforge.net/projects/wxformbuilder/> wxFormBuilder est un logiciel permettant de créer graphiquement des interfaces utilisateur pour wxWidgets puis de générer le code C++ correspondants. 

wxWidgets utilisant beaucoup le concept de Sizer (des boîtes pouvant se redimensionner automatiquement selon leur contenu) l'écriture de code d'interface utilisateur est relativement longue et source d'erreurs. L'utilisation de wxFormBuilder permet de développer plus rapidement des interfaces.

#### ToolBasView
ToolBasView permet d'ouvrir, d'explorer et de manipuler des bases de données MySQL embarquées (cf [](#toolbaseview_ui)). Les projets ToolMap sont en effet des base de données MySQL embarquées. Nous avons développé ToolBasView en parallèle à ToolMap afin d'avoir un moyen simple de visualiser le contenu des projets ToolMap. 

Lors de chaque installation de ToolMap (sur Windows), une version de ToolBasView est également installée. Pour les autres plateformes il est nécessaire de compiler manuellement ToolBasView.

[toolbaseview_ui]: ../img/toolbasview.png width="70%"
![Interface utilisateur de ToolBasView] [toolbaseview_ui]

#### png2wx
Il s'agit d'un petit script PERL, permettant de convertir des fichiers images (*.PNG) dans du code source C++. Cette manière de faire permet d'éviter de distribuer des images avec notre application : tout est inclus dedans. L'avantage par rapport aux XPM est le support du canal alpha. Plus d'informations sur l'inclusion des images dans du code : <http://wiki.wxwidgets.org/Embedding_PNG_Images>.
Une fois installé ce script s'execute de la manière suivante : 

    png2wx.pl 
    -C images.cpp 
    --H images.h
    --M _IMAGES_H_ images/*

#### cxxtest
Téléchargeable à l'adresse <http://cxxtest.com/> cxxtest permet de créer automatiquement une suite de tests en parcourant des fichiers tests au format *.H.

### Windows

#### Visual Studio Express
Téléchargeable depuis l'adresse : <http://www.visualstudio.com/products/visual-studio-express-vs>. Il s'agit du compilateur gratuit de Microsoft. La dernière version utilisée est Visual Studio 2010. Une version plus à jour devrait également fonctionner. Il faut cepandant s'assurer que cmake peut générer des projets pour la version choisie (voir la partie concernant [CMake] [cmake]).

#### NSIS
NSIS permet de créer des programmes d'installation pour Windows depuis des fichiers script. Ces scripts peuvent être produits manuellement ou de manière automatique par CMake (cette option est mise en oeuvre pour ToolBasView). <http://nsis.sourceforge.net/Main_Page> 

#### Dependency Walker
<http://www.dependencywalker.com/>  Permet de scanner les executables et librairies (DLL) afin d'établir la liste des dépendances. Cela permet de s'assurer que les programmes proposés ne dépendent pas d'une librairie exotique.

#### WinMerge
Disponible à l'adresse : <http://winmerge.org/>, WinMerge est un programme permettant d'afficher des différences entre deux fichiers ou dossiers. Il permet également de créer des fichier de différences (aussi appelés patchs.). Pour appliquer un fichier patch, il est nécessaire d'installer l'outil patch depuis GnuWin (<http://gnuwin32.sourceforge.net/packages.html>). 
Il est également possible d'utiliser l'outil TortoiseMerge pour appliquer des patchs. Cet outil fait partie de l'installation de TortoiseSVN (<http://tortoisesvn.net/>).

### OSX

#### Xcode
Xcode est l'environnement de développement gratuit proposé par Apple. Il peut être téléchargé à l'adresse : <https://developer.apple.com/xcode/> ou directement depuis l'App Store. Une fois Xcode installé et afin de s'assurer du bon fonctionnement des scripts, il est également nécessaire d'installer les outils permettant de travailler en ligne de commande. Cette opération peut être effectuée depuis l'onglet *Download* de la fenêtre Préférence de Xcode.

### Linux

#### Code::Blocks
Code::Blocks est un IDE développé avec la librairie wxWidgets. Il est téléchargeable à l'adresse : <http://www.codeblocks.org/>. Il peut également être installé avec la commande : `sudo apt-get install codeblocks`. Pour avoir la toute dernière version il est nécessaire d'ajouter le dépôt PPA suivant: <https://launchpad.net/~pasgui/+archive/ppa/>




