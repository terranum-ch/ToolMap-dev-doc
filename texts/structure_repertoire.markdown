# Structure des répertoires

Cette partie décrit le contenu du répertoire trunk de ToolMap. Il s'agit du répertoire placé sous gestion de version (Subversion). Le répertoire trunk contient l'arborescence listée ci-dessous (uniquement les deux premiers niveaux)
 
1. apps
	 1. lineunion
	 1. shpcompare
	 1. shpimporter
	 1. toolmerge
 1. art
 1. build
	 1. build-script
	 1. cmake
 1. doc
	 1. doc_images
 1. install
	 1. linux
	 1. mac
	 1. windows
 1. plugins
	 1. lscrashreport
	 1. lsversion
 1. resource
	 1. database
	 1. dialogs
	 1. unused_web
	 1. web
 1. src
	 1. components
	 1. core
	 1. database
	 1. gis
	 1. gui
	 1. img
 1. template
 1. test
	 1. build
	 1. src_test

## apps
Ce répertoire contient diverses applications en ligne de commande. En général, elles se composent d'un fichier C++ (main.cpp) et d'un fichier de construction CMake (CMakeLists.txt). Les diverses options supportées sont listée lorsque l'application est lancée avec la commande `application -h`

### lineunion
Cette application a été développée pour répondre à une demande de support de la part de Swisstopo. En effet, un de leur projet contenant 4500 lignes posait des problèmes à ToolMap. Une partie des lignes de ce projet étaient incorrectes. lineunion permet d'unir de manière incrémentielle les lignes du projet afin de localiser les lignes incorrectes. 

### shpcompare
Cette application permet de comparer 2 fichiers ESRI Shapefiles. Elle s'utilise de la manière suivante : `shpCompare -v fichier1.shp fichier2.shp` (cf [](#shpcompare))

[shpcompare]: ../img/shpcompare.png width="80%"
![Exemple d'exécution de l'outil shpCompare. Ici, une erreur a été trouvée.] [shpcompare]

### shpimporter
Cet outil permet d'importer des fichiers ESRI Shapefiles dans un projet ToolMap. Il permet l'import des géométries (points et lignes) ainsi que des attributs. Il a été développé afin de pouvoir importer le projet SION, fabriqué avec l'aide de ToolMap 1, dans un projet ToolMap 2 propre. 

    ShpImporter [-v] [-o] 
    [ToolMap project file] 
    [SHP directory] 
    [rule files directory]

shpimporter fonctionne de la manière suivante. Il prend en entrée un projet ToolMap, un répertoire dans lequel sont stocké les fichiers ESRI Shapefiles ainsi qu'un répertoire dans lequel sont stockés les fichiers de règles (fichiers textes). shpimporter va parcourir touts les fichiers de règles et va, pour chacun, importer les données selon les règles spécifiées. Les fichiers de règles sont donc l'élément clé du processus. Ils font le lien entre: 
 
* le nom du Shapefile et le thème de destination
* Les objets du Shapefile et les objets du projet ToolMap
* Les attributs du Shapefile et les attributs du projet ToolMap.

Les fichiers de règles sont des fichiers avec l'extension .TXT, séparés par des tabulateurs. Ils peuvent être ouverts dans Excel (cf [](#shpimporter)). La structure exacte de ces fichiers serait trop longue à décrire ici, Pour un exemple relativement exhaustif, l'on pourra se référer aux fichiers produits pour la migration du projet SION (répertoire *sion_transfert*). 


[shpimporter]: ../img/shpimporter.png width="50%"
![Fichier de règle de shpimporter ouvert dans Excel] [shpimporter]

###  toolmerge

Cet outil permet de fusionner deux projet ToolMap. Il s'utilise de la manière suivante : `toolmerge project1 project2`. Toolmerge fonctionne de la manière suivante:

 1. Vérification que les deux projets sont identiques
 2. Récupération de l'ID la plus haute pour les géométries du projet1.
 3. Transformation de toutes les ID des géométries du projet2 afin qu'elles soient plus grande que l'ID la plus haute du projet1.
 4. Copie des géométries et des attributs du projet2 dans le projet1.

 Pour que toolmerge fonctionne, les deux projets doivent se situer dans le même répertoire. 

## art
Ce répertoire contient les ressources graphiques liées au projet (icônes, curseurs, images).

## build
Ce répertoire contient le fichier cmake principal (CMakeLists.txt) ainsi qu'un répertoire de scripts cmake permettant de localiser les différentes librairies nécessaire à la compilation du projet.
Le sous répertoire build-script contient des scripts python (fonctionnent avec Python 3 uniquement) :

  1.  **update_PDFLib.py** permet de reconstruire la librairie wxPDFDocument (<http://wxcode.sourceforge.net/components/wxpdfdoc/>). Cette librairie doit être compilée à chaque mise à jour de wxWidgets. Ce script supprime la version existante, décompresse le zip de wxPDFDocument et lance la compilation.
  2.  **update_toolmap.py** est un script interactif permettant de mettre à jour à la dernière version SVN, d'effacer les binaires existant, de compiler, de créer l'installeur et de l'uploader sur le serveur du CREALP et de mettre à jour la page dernière version test de ToolMap (<http://www.crealp.ch/down/toolmap/release_status.html>).

## doc
Ce répertoire contient un fichier et des images permettant de créer la documentation développeur avec Doxygen (classes, fonctions, etc.). Doxygen peut être téléchargé depuis <http://www.doxygen.org/>.

## install
Ce répertoire contient diverses ressources destinées à la construction de programme d'installation pour les différentes plateformes. Ils n'ont normalement pas vocation à être utilisé tels quels mais sont appelés par les scripts de compilation (du répertoire [build](#build)).

## plugins
Ce répertoire contient deux composants dont le code est partagé avec d'autres projets. 
 
1. **lscrashreport** contient le code et les ressources permettant de gérer les rapports de crashs. Lorsqu'un crash survient, une boîte de dialogue (cf. [](#lscrashreport_dlg)) est crée afin de pouvoir envoyer le rapport. Ce composant supporte les proxy web. Afin d'utiliser ce composant, il faut ajouter le code `wxHandleFatalExceptions();` dans le constructeur de l'application et ensuite il faut surcharger la méthode `virtual void wxApp::OnFatalException();`. Dans cette méthode la classe `lsCrashReport` peut être initialisée et appelée (voir le fichier *toolmap.cpp* ligne 88 et suivantes).
2. **lsversion** contient les classes : `lsVersion` et `lsVersionDlg` et permet l'affichage d'une fenêtre listant les différentes versions des composants utilisés ainsi que le numéro de version SVN du logiciel (cf [](#lsversion_dlg)).

[lscrashreport_dlg]: ../img/lscrashreport.png width="80%"
![Fenêtre apparaissant lors d'un crash de ToolMap] [lscrashreport_dlg]

[lsversion_dlg]: ../img/lsversion.png width="60%"
![Fenêtre listant les différents composants utilisés par ToolMap] [lsversion_dlg]

## resource
Ce répertoire contient les ressources suivantes en lien avec ToolMap:

### database
Contient diverses ressources en lien avec la base de donnée embarquée. L'on trouvera ici des fichiers *.MWB. Ces fichiers s'ouvrent avec le logiciel MySQL Workbench <http://www.mysql.fr/products/workbench/>. Ils contiennent les schémas jusqu'à la version 224 (la dernière version dans ToolMap actuellement est la 230).

On trouvera également le fichier décrivant les changements effectués dans la base de donnée jusqu'à la version 221. Ce fichier n'est plus maintenu à jour, en effet, depuis la version 220, ToolMap est capable de mettre à jour d'une version de la base de donnée intégrée à la suivante. Pour plus de détails sur ce processus, on se référera au fichier *tmprojectupdater.cpp* 

### dialogs
Contient principalement les fichiers de définition des fenêtres (fichiers FBP s'ouvrant avec [wxFormBuilder](#wxformbuilder)). Les fichiers *.CPP et *.H sont des fichiers automatiquement générés par wxFormBuilder et conservé ici pour référence. Ils ne devraient pas être modifiés.

### unused_web
Contient des pages web pour Yahoo street et Yahoo satellite. Yahoo fonctionne visiblement selon un autre système de coordonnées ou avec d'autres niveaux de zoom que ceux présents dans Bing ou Google. Du coup ces ressources web ne sont actuellement pas intégrées dans ToolMap mais conservée ici à des fins d'archive.

### web
Contient les fichiers *.HTML et *.JS permettant d'afficher les images de support web dans ToolMap. Ces fichiers sont copiés lors de la compilation (ainsi que lors de la création de l'installeur) dans les ressources de ToolMap. 

## src
Contient les fichiers de code (\*.CPP) et d'en-têtes (\*.H) répartis dans les répertoires suivants:

1. **components** il s'agit en général de code, tout ou partiellement récupéré ailleurs. Ces derniers sont trop petits pour constituer des plugins mais sont suffisamment indépendants pour être mis dans des composants séparés. 
2. **core** contient les fonctions de base du code. Contient notamment le fichier d'entrée *toolmap.cpp*.
3. **database** contient les fichiers et le code permettant l'interaction avec la base de donnée MySQL.
4. **gis** contient le code lié de près ou de loin au fichiers SIG.
5. **gui** contient les fichiers de code définissant l'interface graphique. Toutes les définition de fenêtres sont situées ici. Ces fichiers ont le suffixe *_dlg*.
6. **bmp** ce répertoire contient les images utilisées (barre d'outils, icônes, etc.) sous forme de code source. Ces fichiers ont été générés avec le script [png2wx](#png2wx)

## template
Contient un fichier de code (*.CPP) ainsi qu'un fichier d'en-têtes (\*.H) qui ont vocation à servir de modèle lorsqu'un nouveau fichier de code doit être crée dans l'arborescence de ToolMap.

## test
Ce répertoire contient le fichier CMake ainsi que les sources pour les tests unitaires. Ces derniers permettent de s'assurer qu'aucune régression n'est apparue lors des derniers changements effectués. Les tests unitaires peuvent être lancé par le script **update_toolmap.py** et utilisent [cxxtest] (#cxxtest).




     

    



