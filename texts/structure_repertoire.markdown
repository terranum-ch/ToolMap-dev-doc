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
Ce répertoire contient diverses applications en ligne de commande. En général, elles se composent d'un fichier C++ (main.cpp) et d'un fichier de contruction CMake (CMakeLists.txt). Les diverses options supportées sont listée lorsque l'application est lancée avec la commande `application -h`

### lineunion
Cette application a été développée pour répondre à une demande de support de la part de Swisstopo. En effet, un de leur projet contenant 4500 lignes posait des problèmes à ToolMap. Une partie des lignes de ce projet étaient incorrectes. lineunion permet d'unir de manière incrémentielle les lignes du projet afin de localiser les lignes incorrectes. 

### shpcompare
Cette application permet de comparer 2 fichiers ESRI Shapefiles. Elle s'utilise de la manière suivante : `shpCompare -v fichier1.shp fichier2.shp` (cf [](#shpcompare))

[shpcompare]: ../img/shpcompare.png width="80%"
![Exemple d'execution de l'outil shpCompare. Ici, une erreur a été trouvée.] [shpcompare]

### shpimporter
Cet outil permet d'importer des fichiers ESRI Shapefiles dans un projet ToolMap. Il permet l'import des géométries (points et lignes) ainsi que des attributs. Il a été développé afin de pouvoir importer le projet SION, fabriqué avec l'aide de ToolMap 1, dans un projet ToolMap 2 propre. 

    ShpImporter [-v] [-o] 
    [ToolMap project file] 
    [SHP directory] 
    [rule files directory]

shpimporter fonctionne de la manière suivante. Il prend en entrée un projet ToolMap, un répertoire dans lequel sont stocké les fichiers ESRI Shapefiles ainsi qu'un répertoire dans lequel sont stockés les fichiers de règles (fichiers textes). shpimporter va parcourir touts les fichiers de règles et va, pour chacun, importer les données selon les règles spécifiées. Les fichiers de règles sont donc l'élément clé du processus. Il font le lien entre: 
 
* le nom du Shapefile et le thème de destination
* Les objets du Shapefile et les objets du projet ToolMap
* Les attributs du Shapefile et le attributs du projet ToolMap.

Les fichiers de règles sont des fichiers avec l'extension .TXT, séparés par des tabulateurs. Ils peuvent être ouverts dans Excel (cf [](#shpimporter)). La structure exacte de ces fichiers serait trop longue à décrire ici, Pour un exemple relativement exhaustif, l'on pourra se référer aux fichiers produits pour la migration du projet SION. 


[shpimporter]: ../img/shpimporter.png width="50%"
![Fichier de règle de shpimporter ouvert dans Excel] [shpimporter]

###  toolmerge

Cet outil permet de fusionner deux projet ToolMap. Il s'utilise de la manière suivante : `toolmerge project1 project2`. toolmerge fonctionne de la manière suivante:

 1. Vérification que les deux projets sont identiques
 2. Récupération de l'ID la plus haute pour les géométries du projet1.
 3. Transformation de toutes les ID des géométries du projet2 afin qu'elles soient plus grande que l'ID la plus haute du projet1.
 4. Copie des géometries et des attributs du projet2 dans le projet1.

 Pour que toolmerge fonctionne, les deux projets doivent se situer dans le même répertoire. 

## art
Ce répertoire contient les resources graphiques liées au projet (icônes, curseurs, images).

## build
Ce répertoire contient le fichier cmake principal (CMakeLists.txt) ainsi qu'un répertoire de scripts cmake permettant de localiser les différentes librairies nécessaire à la compilation du projet.
Le sous répertoire build-script contient des scripts python (fonctionnent avec Python 3 uniquement) :

  1.  **update_PDFLib.py** permet de reconstruire la librairie wxPDFDocument (<http://wxcode.sourceforge.net/components/wxpdfdoc/>). Cette librairie doit être compilée à chaque mise à jour de wxWidgets. Ce script supprime la version existante, décompresse le zip de wxPDFDocument et lance la compilation.
  2.  **update_toolmap.py** est un script interactif permettant de mettre à jour à la dernière version SVN, d'effacer les binaires existant, de compiler, de créer l'installeur et de l'uploader sur le serveur du CREALP et de mettre à jour la page dernière version test de ToolMap (<http://www.crealp.ch/down/toolmap/release_status.html>).

## doc
Ce répertoire contient un fichier et des images permettant de créer la documentation développeur avec Doxygen (classes, fonctions, etc.). Doxygen peut être téléchargé depuis <http://www.doxygen.org/>.

## install
Ce répertoire contient diverses ressources destinées à la construction de programme d'installation pour les différentes plateformes. Ils n'ont normallement pas vocation à être utilisé tels quels mais sont appellés par les scripts de compilation (du répertoire [build](#build)).

## plugins
Ce répertoire contient deux composants dont le code est partagé avec d'autres projets. 






     

    



