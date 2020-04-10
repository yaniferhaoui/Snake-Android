# Ferhaoui Yani - Guerrier Hugo - 2A21

Attention : 
	- Le Jeu nécessite l'API 23 au minimum
	- Les tablettes de l'iut qui supportent le jeu : Tablette 08, Tablette 14, Tablette 22, Tablette 04, Tablette 21, Tablette 16

Fonctionnalités :
	Activité 1 (Page connexion) : 
		- Header affichant le meilleur joueur et son score grace à un SharedPreferences
		- Création de compte : (si le compte n'existe pas)
			- Vérification que le nom d'utilisateur fait entre 1 et 6 caractères
			- Vérification que le mot de passe n'est pas vide
		- Connexion au compte : (si le compte existe, autrement création du compte)
			- Si le pseudo existe déjà, vérification que le mot de passe (chiffré en sha256) correspond
	
	Activité 2 (Home - Liste des meilleurs Joueurs) :
		- Affichage du pseudo de l'utilisateur connecté
		- Affichage du meilleur score pour chaque utilisateur ayant fait au moins une partie (trié par meilleur score ou ancienneté si egalité)
	
	Activité 3 (My Compte) :
		- Possibilité de changer le mot de passe (vérification que le mot de passe courant correspond et vérification que le second mot de passe n'est pas vide)
		- Bouton qui redirige vers l'activité 4 (Mon historique des parties)

	Activité 4 (History) :
		- Liste de mes scores triée par date

	Activité 2-3-4 :
		- Affichage du pseudo du joueur connecté
		- Bouton permettant de se déconnecter et de retourner à la page de connexion
		- Bouton permettant de lancer une partie
		- Bouton permettant d'aller dans l'activité liée. (Home -> Account, Account -> Home, History -> Account)

	Activité 5 (Le Jeu) :
		- Utilisation Bitmap et des Canvas pour l'affichage des éléments de la partie
		- Ajout d'ombrages aux briques, aux cases du serpent et à la pizza afin de donner du contraste.
		- Utilisation de la méthode runOnUiThread() de la class Activité afin de faire apparaitre le bouton (depuis un Thread différent de celui du lancement du UI) 
		  pour retourner au Home à la fin de la partie (au lieu d'un AsyncTask)
		- Possibilité de mettre le jeu en pause avec un OnLongClickListener, se qui met le thread principal en waiting
		- Accélération du serpent d'1ms à chaque pizza mangée, la vitesse varie entre 50ms et 10ms par rafraichissement
		- Utilisation de MediaPlayer pour les mouvements, le kill du serpent ou le moment ou le serpent mange la pizza (chaque mouvement à un son différent en fonction de sa direction)
		- Affichage sur le fond d'écran le pseudo du meilleur joueur ainsi que son score et affichage du score de la partie en cours. Si le score courant est le meilleur,
		  l'affichage du meilleur joueur avec son score correspond à la partie courante
		- Le serpent avance de manière fluide (de 0.2f par rafraichissement), mais est soumis au quadrillage de la map, c'est-à-dire il ne peut circuler que sur les axes
		  entiers, grace au modulo. Ainsi lorsqu'un changement de direction est demandé, elle n'est appliqué que lorsque le serpent est sur un axe d'entier x et y
		- Chaque case du serpend possède une liste de 10 ancètres, ainsi chaque case correspond au 10ème élément de la case le précédant. Cela nous a facilité le changement 
		  de direction qui du coup ne s'applique que sur la tête du serpent (les autres cases ne font que suivre le mouvement)
		- Chaque swipe envoie une direction dans une liste d'attente, ainsi cela nous permet d'éviter les erreurs de trajectoires et d'anticiper des mouvements complexes

	Données :
		- Utilisation de SharedPreferences pour le stockage du meilleur joueur avec son score

		Base de données (en sql) :
			- Une table player qui contient le pseudo et le mot de passe d'un utilisateur
			- Une table score qui contient un score, la date et le pseudo du joueur qui doit exister dans la table player (foreign key)
			- Suppression des scores plus vieux de 5 jours qui ne sont pas le meilleur score du joueur l'ayant fait (automatique à chaque insertion de score)
			- Insertion d'un joueur
			- Insertion d'un score
			- Requête récupérant tous les scores d'un joueur trié par la date
			- Requête récupérant les meilleurs scores classés par bestscore et date si égalité
			- Update du mot de passe d'un utilisateur

Remarques : 
	- Le jeu fonctionne globalement bien, aucun bug n'est présent
	- Par la suite :
		- Nous souhaitons améliorer les performances en utilisant une autre solution que les canvas pour l'affichage
		- De plus nous souhaitons ne pas mettre en place de fin (impossible de gagner), lorsque le serpent remplit entièrement la salle,
		  celui-ci réapparaît comme au début (3 cases de longueur) mais avec le même score et la même vitesse dans une nouvelle salle, ainsi le jeu serait infini

## Fin