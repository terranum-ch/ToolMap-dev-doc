# Organisation du code
Cette partie décrit, de manière générale, la façon dont est organisée le code de ToolMap.

## Entrée du programme
La classe *ToolMapApp* sert de point d'entrée du programme. Elle crée et appelle la classe *ToolMapFrame* qui est vraiment le point central d'entrée dans ToolMap. Dans *ToolMapFrame* sont traités les événements suivants:
 
  * Réponses aux menus
  * Activation / Désactivation des éléments de l'UI

C'est également cette classe qui construit l'interface utilisateur (cf [](#)).  
