# Compilation

## Prerequis
Un certain nombre de librairies sont nécessaire au bon fonctionnement de ToolMap. Avant de pouvoir compiler ToolMap, il est nécessaire de compiler ces différentes librairies. Pour les systèmes UNIX, j'utilise systématiquement l'option `--prefix` afin de pouvoir installer les librairies ailleurs que dans les répertoire système. Cela permet de garder un système propre. La convention des dossiers suivie pour les librairies est la suivante :

 * **LIB** répertoire contenant toutes les librairies construites
     * **_LIBGIS** sert de préfixe pour les librairies GEOS, GDAL et PROJ
     * **_LIBMYSQL** sert de préfixe pour la librairie MySQL
     * **_LIBPDF** sert de préfixe pour la librairie wxPDFDocument
     * **_LIBWX** sert de préfixe pour la librairie wxWidgets
     * **gdal-1.10.1** répertoire de GDAL
     * **geos-3.4.2** répertoire de GEOS
     * **mysql-5.6.15** répertoire de MySQL
     * **proj-4.8.0** répertoire de PROJ4
     * **wxpdfdoc-0.9.4** répertoire de wxPDFDocument
     * **wxWidgets-svn** répertoire de wxWidgets 
	

### wxWidgets 
wxWidgets permet de créer des interfaces multi-plateformes (<http://wxwidgets.org/>). Si l'on désire travailler avec la toute dernière version de wxWidgets, on peut effectuer un checkout depuis le dépôt Subversion du projet avec la commande suivante. Cette commande est à lancer dans le répertoire LIB: 

    svn co 
    http://svn.wxwidgets.org/svn/wx/wxWidgets/trunk 
    wxWidgets-svn

#### Compilation automatique
En plus de la méthode manuelle de compilation de wxWidgets décrite un peu plus bas, il est également possible d'utiliser un script python **build_wxwidgets.py** que nous avons développé et qui permet de 

1. mettre à jour wxWidgets avec la dernière version SVN
2. supprimer éventuellement le code déjà existant
3. Compiler la librairie sur toutes les plateformes

Ce script Python s'exécute de la façon suivante

    python3 build-wxwidgets.py 
    <svn folder> 
    <prefix folder>
    

#### OSX - Linux
Les options pour ces deux plateformes sont très proches. Les éventuelles différences seront mise en évidences.

