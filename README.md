# WiC, ROB4
# SH13

En addendum, voici ma proposition d'explication quant à l'utilité des variables fsm et synchro :

### fsmServer
fsm veut dire Finite State Machine -> machine a états finis
Cette variable définit l'état du serveur : lorsqu'elle vaut 0, le serveur est en attente de joueurs, il gère les connexions, entre autres.
Lorsqu'elle vaut 1, le serveur gère le jeu avec les 4 joueurs, les tours, la révélation des cartes, le jeu de manière générale, entre autres.

### synchro
C'est une variable qui va permettre de lier les deux threads "jeu" et "envoi au serveur" du programme sh13
Quand on veut envoyer un message au serveur, on le fait via une écriture dans un socket, processus qui est bloquant.
Donc il faut savoir quand et quoi envoyer.
C'est le rôle de synchro : quand on édite le buffer dans la partie jeu, on change cette variable pour communiquer avec le thread "envoi au serveur" qui va "comprendre" ce changement de valeur de synchro comme "Le buffer a été rempli, il faut envoyer au serveur".
Ensuite, le thread "envoi" au serveur remet synchro à 0 pour dire "Le message a été envoyé, le jeu peut reprendre de notre côté"
