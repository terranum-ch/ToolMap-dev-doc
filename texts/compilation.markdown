# Compilation

## Prerequis
Un certain nombre de librairies sont nécessaire au bon fonctionnement de ToolMap. Avant de pouvoir compiler ToolMap, il est nécessaire de compiler ces différentes librairies. Pour chaque librairie sont décrite les commandes nécessaire à la compilation sous Windows et sous Unix. Les commandes pour compiler sous Linux et OSX sont en général identique. Le cas échéant, les éventuelles différences seront mise en évidence. Pour les systèmes Unix, j'utilise systématiquement l'option `--prefix` afin de pouvoir installer les librairies ailleurs que dans les répertoire système. Cela permet de garder un système propre. La convention des dossiers suivie pour les librairies est la suivante :

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

Sous Windows, afin d'éviter des comportements bizarres ainsi que des erreurs lors du linking, il est nécessaire de compiler toutes les librairies avec le même compilateur (même version et mêmes options de compilation). Par défaut Visual Studio utilise les options /MD et /MDd (pour la version Debug). Ces options lient l'exécutable produit avec la librairie MSVCR100.DLL. Cela peut poser des problèmes lors de la distribution sur des systèmes plus anciens n'ayant pas cette librairie. Nous avons donc systématiquement utilisé les options de remplacement /MT et /MTd (pour la version Debug). Plus d'informations sur ces options sont disponible ici : <http://msdn.microsoft.com/en-us/library/2kzt1wy3.aspx>

Sous Linux, il est possible d'utiliser les librairies fournies par les gestionnaires de paquets. Cette approche présente l'avantage d'être plus rapide mais en contrepartie les versions des librairies ne sont pas les dernières disponible. Cette approche est impérative pour distribuer un paquet d'installation de ToolMap pour les systèmes Linux. Les librairies qui peuvent être récupérées grâce au gestionnaires de paquets sont :

 1. GEOS (libgeos-dev)
 1. GDAL (libgdal1-dev)
 1. CURL (libcurl4-dev)
 1. MySQL (libmysqld-dev, mysql-server-core-5.1)
 
 La commande suivante permet de télécharger et d'installer ces librairies
 
     sudo apt-get install 
     libmysqld-dev mysql-server-core-5.5 
     libgeos-dev 
     libcurl4-gnutls-dev 
     libgdal1-dev 
     libwrap0-dev
     
wxWidgets doit être compilé manuellement, en effet, la version 3.0 n'est pas encore disponible dans les dépôts et la version 2.8 est trop ancienne. Avant de suivre les instructions pour compiler wxWidgets, certaines librairies sont nécessaires et peuvent être installées avec la commande ci-dessous.
     
     sudo apt-get install
     libgtk2.0-dev
     libwebkitgtk-dev
     libaio-dev
  

### wxWidgets 
wxWidgets permet de créer des interfaces multi-plateformes (<http://wxwidgets.org/>). Si l'on désire travailler avec la toute dernière version de wxWidgets, on peut effectuer un checkout depuis le dépôt Subversion du projet avec la commande suivante. Cette commande est à lancer directement dans le répertoire LIB: 

    svn co 
    http://svn.wxwidgets.org/svn/wx/wxWidgets/trunk 
    wxWidgets-svn

#### Compilation automatique
En plus de la méthode manuelle de compilation de wxWidgets décrite un peu plus bas, il est également possible d'utiliser un script python (**build_wxwidgets.py**) que nous avons développé et qui permet de 

1. mettre à jour wxWidgets avec la dernière version SVN
2. supprimer éventuellement le code déjà existant
3. Compiler la librairie sur toutes les plates-formes

Ce script Python s'exécute de la façon suivante

    python3 build-wxwidgets.py 
    <svn folder> 
    <prefix folder>
    

#### Unix
Configuration de wxWidgets

    ../configure --prefix=~\LIB\_LIBWX 
    --enable-unicode 
    --disable-monolithic 
    --disable-shared  
    --without-libtiff 
    --enable-mediactrl=no 
    --with-libjpeg=builtin 
    --with-libpng=builtin
    
   
Options spécifiques à OSX: `--with-osx_cocoa`
       
Sous OSX, si l'on veut spécifier une version minimum du SDK 

    --with-macosx-version-min=10.6
    --with-macosx-sdk=.../SDKs/MacOSX10.6.sdk
    
Options spécifiques à Linux: `--enable-graphics-ctx`

Compilation et installation: `make -j8; make install` Le numéro spécifié en regard de l'option `-j` indique le nombre de processeurs à utiliser pour la compilation.

    
#### Windows

Deux fichiers *setup.h* doivent être copiés de la manière suivante

    copy D:\LIB\wxWidgets-svn\include\wx\msw\setup0.h 
         D:\LIB\wxWidgets-svn\include\wx\setup.h
    copy D:\LIB\wxWidgets-svn\include\wx\msw\setup0.h 
         D:\LIB\wxWidgets-svn\include\wx\msw\setup.h

Lancer les commandes suivantes dans le répertoire *D:\LIB\wxWidgets-svn\build\msw* afin de compiler les versions Debug et Release.

    nmake -f makefile.vc BUILD=debug CPPFLAGS="/MTd /MP" UNICODE=1 MONOLITHIC=1
    nmake -f makefile.vc BUILD=release CPPFLAGS="/MT /MP" UNICODE=1 MONOLITHIC=1


### wxPDFDocument
wxPDFDocument permet de créer des fichiers PDF de manière simple depuis wxWidgets. wxPDFDocument peut être téléchargé à l'adresse : <http://wxcode.sourceforge.net/components/wxpdfdoc/>

#### Compilation automatique
En plus de la méthode manuelle de compilation décrite un peu plus bas, il est également possible d'utiliser un script python (**update_PDFLib.py**) afin d'automatiser le processus. Ce script est situé dans le répertoire *build/build-script* de ToolMap et effectue les opérations suivantes : 

1. Supprime le répertoire de wxPDFDocument (wxpdfdoc-0.9.4)
2. Supprime le contenu de _LIBPDF
2. Décompresse l'archive wxpdfdoc-0.9.4.zip
3. Patch les fichiers si nécessaire
4. Configure, compile et installe la librairie

Ce script fonctionne de manière interactive et s'appelle de la manière suivante : `python3 update_PDFLib.py`. Les fonctions *Windows(), MacBook(), LinuxHome() et MacPro()* peuvent être éditées afin de spécifier les chemins corrects pour ces différentes plates-formes.

#### Windows
Dans le fichier *build29/makefile.vc* modifier la ligne WX_VERSION = 29 par WX_VERSION = 30 (ou 31 selon la version de wxWidgets utilisée). Dans le répertoire *build29*, lancez les commandes suivante pour compiler wxPDFDocument

    nmake -f makefile.vc
    WX_DIR=D:\LIB\wxWidgets-svn
    WX_MONOLITHIC=1
    WX_DEBUG=0
    CPPFLAGS=/nologo /MT /MP /EHsc /Ox /W3

remplacer les deux dernières lignes de la commande précédente pour construire la version Debug comme illustré ci-dessous

    WX_DEBUG=1
    CPPFLAGS=/nologo /MTd /MP /EHsc /Zi /W4 /DDEBUG

    

### GEOS
 GEOS est une librairie permettant d'effectuer diverses opérations sur les objets géométriques (<http://trac.osgeo.org/geos/>). Elle n'est pas utilisée directement mais est appelée par [GDAL](#gdal). Pour compiler GEOS, il est possible d'utiliser CMake. Actuellement cela pose les problèmes suivants:
  
  * Problèmes lors de l'édition des liens sous OSX
  * Impossibilité de remplacer /MDd par /MTd et /MD par /MT.
  
Nous utiliserons donc la méthode traditionnelle. 
 
 
#### Unix
A part le préfixe il n'y a pas d'options spécifique pour compiler cette librairie. Lancer la commande suivante dans le répertoire de GEOS (*~LIB/geos-3.4.2*)

    mkdir build64
    cd build64
    ./configure --prefix=~/LIB/_LIBGIS
    make -j8; make install

#### Windows
Dans le répertoire de GEOS (geos-3.4.2) on lance la commande `autogen.bat` puis on édite le fichier *nmake.opt* dans lequel on remplace toutes les occurrences de /MDd par /MTd puis toutes les occurrences de /MD par /MT. On peut ensuite lancer la compilation avec la commande

    nmake -f makefile.vc GEOS_MSVC=10

La version spécifiée ici de `GEOS_MSVC` correspond à Visual Studio 2010.

### PROJ4
Proj4 est une librairie permettant de projeter les données SIG dans divers systèmes de projections. Cette librairie n'est également pas directement utilisée mais elle est appelée par [GDAL](#gdal). Elle est disponible au téléchargement à l'adresse : <http://trac.osgeo.org/proj/>

#### Unix
Il n'y a pas d'option particulière pour construire cette librairie, les mêmes options que pour [GEOS](#geos) s'appliquent.

#### Windows
Editer le fichier *nmake.opt* et remplacer /MD par /MT. Dans le même fichier, changer également le répertoire INSTDIR pour le faire pointer vers *D:\LIB\_LIBGIS*. Lancer la compilation avec la commande suivante:

    nmake -f makefile.vc
    nmake -f makefile.vc install-all
    
### GDAL
GDAL est une librairie permettant d'accéder aux fichiers SIG de type image, de manière unifiée. A l'intérieur du code de GDAL se trouve la librairie OGR qui permet l'accès aux données SIG vectorielles (<http://www.gdal.org/>). GDAL supporte un très grand nombre de formats de fichiers. Certains sont supportés nativement, d'autres nécessitent des librairies externes. Afin d'éviter d'augmenter de manière significative les dépendances de GDAL, nous ne compilerons que le strict minimum (GEOS et PROJ4).

Un certain nombre de programmes en ligne de commande sont produits durant la compilation de GDAL. A eux seuls, ils permettent déjà d'effectuer un grand nombre d'opérations sur les données spatiales. Une documentation plus complète est disponible en ligne :

 * Outils de manipulations des images avec GDAL : <http://www.gdal.org/gdal_utilities.html>
 * Outils de manipulation des vecteurs avec OGR : <http://www.gdal.org/ogr_utilities.html>

#### Unix
Lancer la commande suivante dans le répertoire de GDAL (gdal-1.10.1)

    ./configure 
    --prefix=~/LIB/_LIBGIS 
    --with-geos=~/LIB/_LIBGIS/bin/geos-config
    --with-static-proj4=~/LIB/_LIBGIS 
    --with-sqlite3=yes 
    --with-python=no 
    --with-pg=no 
    --with-grass=no 
    --with-jasper=no 
    --with-jpeg=internal 
    --with-png=internal

Avec OSX, si l'on désire supporter un SDK particulier, rajouter ces lignes après le `./configure` et avant le `--prefix` :

     
    CFLAGS="-02 -isysroot .../SDKs/MacOSX10.6.sdk" 
    CXXFLAGS="-02 -isysroot .../SDKs/MacOSX10.6.sdk" 
    LDFLAGS="-isysroot .../SDKs/MacOSX10.6.sdk"
    

#### Windows
Editer le fichier nmake.opt et modifier les points suivants:

1. *MSVC_VER=1500* avec le numéro correct de votre compilateur (pour VS2010 il s'agit de 1600).
2. *GDAL_HOME = "C:\warmerda\bld"* avec le chemin *D:\LIB\_LIBGIS* 
3. Remplacer tous les /MDd par /MTd et tous les /MD par /MT
4. Enlever le caractère dièse devant les 3 lignes commençant par *GEOS_…*
5. *GEOS_DIR=C:/warmerda/geos* avec le chemin *D:/LIB/geos-3.4.2*
6. La structure de GEOS ayant changé dès la version 3.3.0 il faut également changer les deux lignes suivantes avec les valeurs suivantes
    1. *GEOS_CFLAGS = -I$(GEOS_DIR)/capi -I$(GEOS_DIR)/include -DHAVE_GEOS* 
    2. *GEOS_LIB = $(GEOS_DIR)/src/geos_c_i.lib*
    
Lancer la compilation avec la commande

    nmake -f makefile.vc
    nmake -f makefile.vc install
    nmake -f makefile.vc devinstall


### MySQL
MySQL est une base de donnée relationnelle. Dans Toolmap, nous utilisons la version embarquée de cette base de donnée. Le serveur MySQL n'a pas besoin d'être installé chez le client, il est embarqué dans l'application ToolMap. Le code source de MySQL est disponible à l'adresse : <http://dev.mysql.com/downloads/mysql/>. 

La version 5.6.15 nécessite d'être corrigée avant de pouvoir être construite. Normalement ce point devrait être résolu dans les prochaines versions. Le bug est décrit ici : <http://bugs.mysql.com/bug.php?id=71010>. Pour le corriger, télécharger le correctif à l'adresse : <http://bugs.mysql.com/file.php?id=20813&bug_id=71010> puis l'appliquer sur le code de MySQL avec la commande suivante (dans le répertoire de MySQL):

    patch -p0 < patch.path

Pour les utilisateurs Windows, veuillez vous référer à la section concernant [WinMerge](#winmerge) pour plus d'explications sur comment appliquer un patch.

#### Unix
Dans le répertoire MySQL (mysql-5.6.15) lancer la commande suivante:

    cmake . 
    -DCMAKE_INSTALL_PREFIX=~/LIB/_LIBMYSQL
    -DWITH_UNIT_TESTS:BOOL=OFF 
    -DFEATURE_SET:STRING=small
    
Les options suivantes peuvent être utilisées avec OSX pour contrôler l'architecture ainsi que le SDK de référence.
    
    -DCMAKE_OSX_ARCHITECTURES:STRING="x86_64"
    -DCMAKE_OSX_DEPLOYMENT_TARGET:STRING="10.6"
    -DCMAKE_OSX_SYSROOT:PATH=.../SDKs/MacOSX10.6.sdk
   
La compilation et l'installation peuvent être lancés avec la commande : `make -j8; make install`.

#### Windows
Modifier le fichier *cmake/build_configurations/feature_set.cmake*, changer community par small.
Lancer la commande suivante depuis le répertoire de MySQL (mysql-5.6.15)
    
    cmake . 
    -G"Visual Studio 10" 
    -LH -DWITH_DEFAULT_FEATURE_SET=ON

Pour compiler la version Debug, lancer la commande

    msbuild 
    /property:Configuration=Debug
    MySQL.sln

Puis pour compiler la version Release, modifier la ligne `/property:Configuration=Release` et relancer la commande précédente. Une fois les deux versions compilées, un petit script \*.bat est disponible dans le répertoire source de ToolMap (*install/windows/mysql/copy_mysql.bat*). Modifier les lignes :
 
 * @SET DESTDIR
 * @SET SOURCEDIR
 
 Et lancer le script pour copier les librairies MySQL dans le répertoire D:\LIB\_LIBMYSQL.
 
# ToolMap
Une fois toutes les librairies compilées ou installées depuis le gestionnaire de paquet, il est possible de passer à la compilation de ToolMap. L'organisation des fichiers proposée est la suivante :

  * **LIB** répertoire contenant les librairies (voir [Librairies](#prerequis)).
  * **ToolMap**
      * **bin** contiendra l’exécutable de ToolMap
      * **bin_apps** contiendra des sous-répertoire pour les applications en ligne de commande fournies avec toolmap
      * **bin_tbv** contiendra l’exécutable de ToolBasView
      * **install** contiendra les programme d'installation de ToolMap 
      * **trunk** code source de ToolMap, voir description dans [Structure des répertoires](#structuredesrpertoires)
      * **trunk_tbv** code source de ToolBasView
      
Un script Python (**update_toolmap.py**) permettant de :

 1. Mettre à jour ToolMap depuis le dépôt SVN
 1. Créer le projet ToolMap (projet Visual Studio, Solution XCode, etc.)
 1. Compiler ToolMap
 1. Lancer les tests unitaires (optionnel).
 1. Créer le programme d'installation (optionnel)
 
Est disponible dans le répertoire *build/build-script*. Avant de lancer ce script pour compiler ToolMap de manière interactive, il est nécessaire d'éditer, dans le même répertoire, le script Python correspondant à la plateforme désirée. Les premières lignes de ces scripts de configuration permettent de spécifier les répertoires dans lesquels se trouvent les différentes librairies.

Lancer ensuite la compilation interactive de ToolMap avec la commande `python3 update_toolmap.py`

# ToolApps
Sous ce nom se cachent les applications en ligne de commande développées dans le cadre du projet ToolMap. Leurs utilités est décrite à la partie [Apps](#apps).

Chacune de ces application dispose d'un script python permettant de la compiler. Editer ces scripts python pour corriger les chemins des différentes librairies puis exectuer : `python update_toolmerge.py` par exemple. Une fenêtre identique à celle illustrée à la [](#update_toolmerge) devrait apparaître. 

[update_toolmerge]: ../img/update_toolmerge.png width="50%"
![Fenêtre permettant de configurer l'application en ligne de commande ToolMerge][update_toolmerge]

# ToolBasView
ToolBasView est une application permettant d'ouvrir et de manipuler les bases de données MySQL embarquées. Pour plus d'informations sur l'outil, voir la partie consacrée à [ToolBasView](#toolbasview).

## Dépôt SVN
Le code source de ToolBasView est disponible sur l'ancien serveur SVN du CREALP. Il peut être récupéré avec la commande suivante (dans le répertoire ToolMap).

    svn co https://213.221.129.20/svncrealp/toolbasview/trunk/ trunk_tbv
    
Par souci de cohérence, il serait judicieux de migrer le code vers le dépôt SVN principal du CREALP (<http://svn.crealp.ch/svn>).

## Compilation
ToolBasView (TBV) peut être compilé grâce à un script Python (**update_toolbasview.py**) disponible dans le répertoire *trunk_tbv/build/script*. Une fois lancé avec la commande `python update_toolbasview.py` ce script affiche la fenêtre illustrée à la [](#tbv_build_ui). Pour que ce script fonctionne Python doit comprendre l'extension tkinter. Dans la plupart des cas, il n'y a rien à faire, cette extension est déjà installée. 

[tbv_build_ui]: ../img/tbv_build_ui.png width="50%"
![Interface TK permettant de compiler ToolBasView][tbv_build_ui]

En fonction des chemins choisis il sera peut-être nécessaire de modifier quelque peu le script Python. Principalement les chemins listés dans les fonctions *BuildNomDeLaPlateforme()*.











 
