
Les ports : 
ports:
	- "8080:80"
	signifie PORT_HOST : PORT_CONTAINER

On tape dans localhost:8080, qui est le port du pc. Et le port sur lequel écoute l'application est celui de droite 80. Localhost:8080 redirige vers le container 80 qui renvoie l'application