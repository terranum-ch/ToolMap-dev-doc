# Organisation du code
Cette partie décrit, de manière générale, la façon dont est organisée le code de ToolMap.

## Généralités
La classe *ToolMapApp* sert de point d'entrée du programme. Elle crée et appelle la classe *ToolMapFrame* qui est vraiment le point central dans ToolMap. Dans *ToolMapFrame* sont traités les événements suivants:
 
  * Réponses aux menus
  * Activation / Désactivation des éléments de l'UI

C'est également cette classe qui construit l'interface utilisateur (cf [](#toolmap_class_ui)). et C'est également *ToolMapFrame* qui initialise les différents "Managers" avec les données dont ils ont besoin. 

[toolmap_class_ui]: ../img/classe_ui_tm.pdf width="100%"
![Interface utilisateur de ToolMap et classes en charges des différents composants][toolmap_class_ui]

Les différentes fenêtres d'outils de ToolMap (TOC, Panneau des raccourcis clavier, Snapping, etc.) sont des panneaux basés sur la classe *wxAui*. Ils sont illustrés en jaune dans la [](#toolmap_class_ui). Ces panneaux contiennent ensuite une autre classe en charge de la gestion du contenu (illustrés en blanc). Il s'agit en général d'une liste ou d'un contrôle de type arbre. La fenêtre centrale est gérée par la classe *tmRenderer* (dérivée de *wxScrolledWindow*). La classe *tmDrawer* est chargée de dessiner les données SIG dans une image qui sera ensuite affichée dans *tmRenderer*.

## Managers
Afin d'éviter de surcharger *ToolMapFrame* des classes "Managers" ont été crées. Ceux-ci servent principalement à séparer les fonctionnalités par domaines. Les principaux "Managers" sont listés ci-dessous

 * *tmLayerManager* : il s'agit de la classe en charge des opérations SIG de base : zoom, pan, etc. C'est également cette classe qui s'occupe de faire le lien entre *tmTOCCtrl, tmRenderer et tmDrawer* pour le rafraichissement du dessin des données SIG.
 * *tmEditManager* : classe en charge de tout ce qui concerne l'édition (dessin des segments, des Béziers, Création d'intersections, fusion de lignes, etc.)
 * *tmAttributionManager* : cette classe à la responsabilité de l'attribution. Elle assure la communication entre *tmCheckListBoxRank* et la base de données *DatabaseTM*. Cette classe répond également aux raccourcis claviers. 
 * *tmExportManager* : classe en charge de l'export des données vers des fichiers. 
 
 D'autres "managers" sont également disponibles et leur nom est en général relativement clair pour que l'on puisse se faire une bonne idée de leur utilité (par exemple *BackupManager*). Historiquement la communication entre *ToolMapFrame*, les "Managers" et d'autres classes étaient effectués par messages. Pour simplifier le déboguage et clarifier l'ordre d'appel des fonctions, nous avons plutôt choisi d'appeler directement des fonctions plutôt que d'envoyer des messages.
 

## Projet
Lors de l'ouverture d'un projet, celui-ci est chargé depuis la base de donnée (*DatabaseTM*) dans une structure hiérarchique de diverses classes. En effet il serait trop long d'interroger la BDD chaque fois qu'une information sur le projet est nécessaire. La structure hiérarchique est décrite ci-dessous et est illustrée à la [](#structure_projet).

 1. *PrjDefMemManage* contient les paramètres du projet ainsi qu'une liste de thèmes (layers).
     1. *ProjectDefMemoryLayers* contient des informations générales sur un thème ainsi qu'une liste d'objets (Kind) appartenants à ce thème. Contient également une liste de champs (Fields) s'appliquant à ce thème.
         1. *ProjectDefMemoryObjects* contient les paramètre d'un objet.
         1. *ProjectDefMemoryFields* contient les paramètres d'un champ ainsi qu'une liste de valeurs pour ce champ (s'applique uniquement pour les champs de type énumérations).
             1. *ProjectDefMemoryFieldsCodedVal* contient les paramètres d'une valeur pour un champ.

[structure_projet]: ../img/structure_projet.jpg width="100%"
![Illustration de la structure des données d'une projet ToolMap (d'un point de vue du code)][structure_projet]


## Dessin
La table des matières (TOC) est gérée par la classe *tmTOCCtrl*. Pour chaque entrée dans la TOC un objet *tmLayerProperties* est associé. Cette classe contient toutes les informations sur un thème SIG : nom, visibilité, type spatial, classe de symbologie associée, etc. Lors du dessin, la classe *tmLayerManager* va parcourir cette liste de *tmLayerProperties*. Pour chaque thème visible il sera demandé à *tmDrawer* de dessiner ce thème dans une image puis finalement l'image sera passée à *tmRenderer* pour affichage. 

 


