* Désactiver l'accès public au compte de stockage (l'accès au share sera perdu)

![alt text](img/pvt01.png)

* Créer le point de terminaison privé sur le compte de stockage

![alt text](img/pvt02.png)

![alt text](img/pvt03.png)

![alt text](img/pvt04.png)

![alt text](img/pvt05.png)


* Vérifier et créer

* Valider l'IP du file share dans la zone DNS créée dans Azure.

![alt text](img/pvt09.png)


* Créer une règle de transfert dans le DNS on-prem pointant vers le serveur DNS dans Azure.

```
file.core.windows.net ->  192.168.0.5
```

![alt text](img/pvt06.png)

* Sur le serveur DNS dans Azure, créer un transfert pointant vers l'IP DNS global d'Azure.

![alt text](img/pvt07.png)  


* Test de ping avant et après la création du Private Endpoint et des transferts :

![alt text](img/pvt08.png)
