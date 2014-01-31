# Base de données
Les projets ToolMap sont stockés sous forme de Base de donnée MySQL embarquée. Il se presentent sous forme d'une répertoire portant le nom du projet et contenant des fichiers :
  
  * *.MYD
  * *.MYI
  * *.FRM
  
[ToolBasView](#toolbasview) peut être utilisé pour ouvrir ces Base de données.

## Modèle de donnée
Le modèle de donnée utilisé dans ToolMap est illustré à la [](#datamodel_pdf). Il s'articule comme suit:
 
 * Les thèmes sont listés dans la table *thematic_layers*. Chaque thème défini par l'utilisateur reçoit un LAYER_INDEX. 
 * Les types d'objets (kind) associés aux thèmes sont stockés dans la table *dmn_layer_object* et sont liés à un thème ainsi qu'à un type d'objet (ligne, point, polygone). En effet pour les thèmes du type polygone il y aura 2 types d'objets liés : les labels définissant la valeur du polygone ainsi que les lignes définissant le contour du polygone.  
 * Les attributs sont gérés de la manière suivante. Une table *layer_at<LAYER_INDEX>* est crée. Elle comprend tous les attributs définis par l'utilisateur. 
     * Les attributs de type énumération sont gérés comme des colonnes INTEGER dans les tables *layer_at*. Les énumérations sont en effet gérées avec les tables décrites ci-dessous. Lorsque l'utilisateur choisit une valeur d'énumération, c'est la valeur CATALOG_ID qui est stockée dans la colonne correspondante de la table *layer_at*.
     	* *dmn_layer_attribut* : fait le lien entre le nom de l'attribut et l'identifiant du thème (layerindex).
     	* *dmn_catalog* : liste tous les codes et valeurs d'énumérations
     	* *dmn_attribut_value* : fait les lien entre les deux tables précédentes
     * Les autres types d'attributs sont gérés plus simplement et la valeur choisie est directement écrite dans la colonne correspondante de la table *layer_at*.
 * Les objets spatiaux (points, lignes, labels) sont stockés dans les thèmes *generic_points, generic_lines, generic_labels* 
 * La table *generic_aat* sert de table de mélange entre les types d'objets (kind) et les géométries. En effet, une géométrie peut se voir assigner plus d'un type d'objet. C'est un des principes de la méthode SION.
 * Les raccourcis claviers sont stockés dans la table *dmn_shortcut_key*. Pour chaque raccourci (touches F1-F12) plusieurs types d'objets peuvent être attribués dans la table *shortcut_list*.
 
 Les autres tables sont moins imbriquées et moins compliquées à comprendre. Elles sont décrites sommairement ci-dessous.
 
  * *prj_toc* : contient une entrée par élément présent dans la table des matière. Gère les paramètres d'affichage du thème SIG concerné. Contient également un lien vers le snapping qui s'applique à ce thème.
  * *prj_settings* : contient une seule entrée avec les paramètres du projet. La structure permettant de gérer les projets multilingue est en place mais n'est pas fonctionnelle dans ToolMap.
  * *zoom_level* : stocke les niveaux de zooms
  * *prj_queries* : stocke le code SQL des requêtes
  * *export_poly* : table utilisée lors de l'export des polygones pour trouver et stocker le meilleur facteur de taille des images d'export. Ces images sont utilisées pour accélérer le processus de recherche du polygone correspondant aux labels.
  * *prj_stats* : statistique du projet   
  * *generic_dmn* : pas utilisée actuellement. 
     	
     
[datamodel_pdf]: ../img/datamodel230.pdf width="100%"
![Représentation schématique du modèle de donnée utilisé dans ToolMap][datamodel_pdf]



