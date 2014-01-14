# Ressources

## Subversion

Le code source de ToolMap est stocké dans un dépôt Subversion. Il est accessible à l'adresse : <http://svn.crealp.ch/svn/toolmap2>. 
Pour le télécharger, la commande suivante peut être utilisée: `svn co http://svn.crealp.ch/svn/toolmap2/trunk D:\DEV\ToolMap\trunk`.

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

L'utilisateur est alors enregistré et peut créer des tickets. Pour accéder au niveau administrateur, l'utilisateur peut être ajouté par un autre administrateur en utilisant le panneau Admin. de TRAC ou alors si il n'existe pas d'autre administrateur en utilisant la commande : `$ trac-admin /data/trac/crealp/projects/toolmap2 permission add nom_utilisateur TRAC_ADMIN`


## Outils

Pour pouvoir développer ToolMap un certain nombre d'outils doivent être installés. Certains outils sont communs à toutes les plateformes (Windows, OSX, Linux). D'autres sont spécifiques à une plateforme. 

Nous avons choisi d'utiliser le plus possible l'environnement de développement recommandé pour chaque plate-forme afin d'obtenir les meilleures performances possible. Les quelques tests menés au début du projet en utilisant par exemple MinGW sous Windows montraient des executables beaucoup plus gros et plus lent que ceux produits par Visual Studio. Ces tests datant de 2008 il serait interessant de voir dans quelle mesure les résultats obtenus alors sont toujours pertinants.

### Outils communs

#### CMake
Le logiciel CMake téléchargeable gratuitement à l'adresse : <http://www.cmake.org> permet de créer, depuis un fichier unique, des projets pour les différents environnements de développements. 


