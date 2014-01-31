# Divers

## Manuel utilisateur
Le manuel utilisateur est disponible en ligne à l'adresse : <http://www.crealp.ch/toolmap/documentation/doku.php>. Il est disponible sous forme de wiki. En plus du manuel utilisateur l'on trouveras également un autorail permettant de prendre en main ToolMap ainsi que quelques trucs et astuces pour utilisateurs expérimentés. 

### Conversion en PDF
Lors de la sortie officielle d'une version de ToolMap, le manuel utilisateur doit être compilé en un PDF pour être disponible au téléchargement. La méthode suivie pour cela est décrite ci-dessous :

 1. Ouvrir la page <http://www.crealp.ch/toolmap/documentation/doku.php?id=total> dans votre navigateur.
 2. Enregistrer au format *.ODT en pressant sur l'icône OpenOffice située en haut à droite de la page.
 3. Installer le plugin Writer2Latex (téléchargeable depuis <http://writer2latex.sourceforge.net/>)
 4. Lancer la commande `java -jar writer2latex.jar fichier.odt`. 
 5. (optionnel) Réduire les images avec le script python **change_image_width.py**
 6. Modifier le fichier *.TEX pour inclure le code gérant la première page (utiliser les anciennes version pour ça).
 7. Exporter le fichier *.TEX en PDF avec la commande `texi2pdf fichier.tex`.
 

## Gestion des téléchargements
Le site <http://www.crealp.ch/toolmap> propose le téléchargement de la dernière version stable de ToolMap. Les avantages de ce site sont les suivants :
 
  * Construction automatique des pages en scandant le contenu du répertoire *toolmap/download*
  * Formulaire permettant de recevoir des informations lors d'un téléchargement
  * Design simple, réactif et adaptatif (Responsive web design).
  * En anglais.
  
### Nouveau contenu
Pour proposer du contenu, procéder comme suit:
 
 1. renommer les répertoire existant dans *toolmap/download* pour que le premier commence par "02_".
 2. Ajouter un répertoire commençant par "01_" suivi du numéro de version
 3. Ajouter un fichier **folder_desc.txt** contenant deux lignes : Le nom de code de la version et en dessous la date (seulement le mois et l'année).
 4. Déposer les fichiers que vous voulez distribuer dans ce répertoire.
 5. (optionnel) Pour masquer le nom du programme dans la page téléchargement et afficher quelque chose de plus clair, il est possible de créer un fichier texte portant le même nom qu'un fichier proposé. La première ligne de texte sera alors utilisée en lieu et place du nom du fichier.
 

### Statistiques
Le nombre de téléchargements ainsi que les données soumises par les utilisateurs sont stockées dans une base de donnée au format SQLite. Cette dernière se situe dans le répertoire *toolmap/db*. Pour exporter facilement le contenu de cette base de donnée il est possible d'utiliser l'outil SQLite Export librement téléchargeable à l'adresse : <http://www.speqmath.com/tutorials/sqlite_export/>.


## Gestion des modèles de données
La création de modèle de données depuis l'interface utilisateur fonctionne correctement. Par contre pour créer et faire évoluer de gros modèles de données, elle présentait quelques inconvénients : 

 * Impossibilité de visualiser facilement les différences entre plusieurs versions successive d'un modèle de donnée
 * Impossibilité de gérer simplement les langues
 
Pour adresser ces problèmes, nous avons mis en place la méthode suivante :

 1. Editer le fichier user_content.txt (compatible Excel) et renseigner les thèmes, objets, attributs
 2. Editer le fichier user_structure.sql et ajouter du code pour stocker les attributs.
 3. Télécharger depuis <http://trac.crealp.ch/datamodel/browser/trunk/models/base> le fichier de base s'appliquant au projet (Swiss ou WGS84).
 4. Créer à l'aide de TmDMCreator (disponible ici <http://trac.crealp.ch/datamodel/downloads>) le fichier result.sql avec la commande suivante :
         
         TmDmCreator 
         base_structure.sql 
         user_structure.sql 
         user_content.txt 
         result.sql

 5. Créer une nouvelle base de donnée avec ToolBasView (donner le nom désiré pour le projet) 
 6. Dans ToolBasView, utiliser le menu *Data > Import > From SQL* et choisir le fichier **result.sql**
 7. Si tout c'est bien passé, il est possible de fermer ToolBasView et d'ouvrir le projet crée avec ToolMap.
 
Un tutoriel plus complet décrivant cette méthode est disponible en ligne à l'adresse :  <http://trac.crealp.ch/datamodel/downloads> (en anglais).

## Conversion de projets
Actuellement il y a deux versions de ToolMap disponible :

 1. **2.5.1548 - Nendaz** : il s'agit de la version officielle. Elle ouvre les projets ToolMap version 227 (disponible à l'adresse <http://www.toolmap.ch>).
 2. **2.6.1646 - Meyrin** : il s'agit de la version test. Elle ouvre les projets ToolMap version 230 (disponible à l'adresse : <http://www.crealp.ch/down/toolmap/release_status.html>).  
 
 L'apparition des systèmes de projections ainsi que des labels nécessite de modifier la version des projets. Lorsqu'un projet (version 227) est ouvert par la dernière version de ToolMap il est automatiquement converti. Il est possible de revenir manuellement en arrière de la manière suivante :
 
 1. Ouvrir le projet dans ToolBaseView
 2. *View > Show Query Panel*
 3. *View > Show Log Panel*
 4. Dans l'espace de requête entrer la commande suivante :
 
 
 
  
         ALTER TABLE prj_toc DROP LABEL_VISIBLE;
         ALTER TABLE prj_toc DROP LABEL_DEF;
         DELETE FROM prj_toc WHERE GENERIC_LAYERS=104;
         UPDATE prj_toc SET GENERIC_LAYERS=104  where GENERIC_LAYERS=105;
         UPDATE prj_settings SET PRJ_UNIT = "Meters";
         UPDATE prj_settings SET PRJ_PROJECTION = "No projection";
         ALTER TABLE prj_settings MODIFY COLUMN `PRJ_UNIT` VARCHAR(10) NOT NULL;
         UPDATE prj_settings SET PRJ_VERSION=227;
      
 Si tout c'est bien passé le projet peut maintenant être ouvert avec la version stable (2.5) de ToolMap. Attention cependant cette méthode ne va fonctionner correctement que pour des projets avec la projection Suisse. Le support des degrés décimaux n'étant pas présent dans la version 2.5.
 
      
      
     
 
 
 
 