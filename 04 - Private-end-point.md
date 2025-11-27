* Desabilitar o acesso publico ao storage account (ira perder o acesso ao share)

![alt text](img/pvt01.png)

* Criar o private endpoint no storage account

![alt text](img/pvt02.png)

![alt text](img/pvt03.png)

![alt text](img/pvt04.png)

![alt text](img/pvt05.png)


* Review e create

* Valide o IP do file share na Zona de DNS criada no Azure.

![alt text](img/pvt09.png)


* Criar um forward rule no DNS on-prem apontando para o servidor dns no Azure.

```
file.core.windows.net ->  192.168.0.5
```

![alt text](img/pvt06.png)

* No servidor DNS no azure, crie um forward apontando para o ip de DNS global do Azure.

![alt text](img/pvt07.png)  


* Teste de ping antes e depois de criar o Private Endpoint e os forwards:

![alt text](img/pvt08.png)

