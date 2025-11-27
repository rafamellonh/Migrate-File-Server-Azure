
https://learn.microsoft.com/en-us/azure/storage/files/storage-files-identity-ad-ds-enable

Name : MPWFS02.telefilm.gc.ca
Domain : telefilm.gc.ca


DNS Active Directory Azure :

Custom DNS servers
tfc-ad01 : 10.103.172.4 - Canada East
tfc-ad02 : 10.104.172.4 - Canada Central

RG : tfc-prod-rg

Qual RG do Azure?
Replicacao ?  LRS OU ZRS ou GZRS    https://learn.microsoft.com/en-us/azure/storage/common/storage-redundancy

![alt text](img/lrs.png)
![alt text](img/zrs.png)
![alt text](img/gzrs.png)
 
| Tipo | Cópias | Onde replica | Protege contra | Ideal para |
|------|--------|-----------------------------|------------------------|------------------------------|
| **LRS** | 3 | Um único datacenter | Falhas locais pequenas | Teste, dev, backups simples |
| **ZRS** | 3+ | 3 zonas de disponibilidade | Queda de zona | Apps críticos |
| **GZRS** | ZRS + réplica geográfica | Zonas + região secundária | Queda total da região | Alta resiliência empresarial |





CONTA DO AZURE TEM QUE SER NO MINIMO CONTRIBUTOR
E DO AD TEM QUE TER PERMISSAO PARA ADICIONAR O STORAGE ACCOUNT AO DOMINIO

![alt text](image.png)