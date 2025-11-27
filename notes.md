Name : MPWFS02.telefilm.gc.ca
Domain : telefilm.gc.ca

Entra ID?
Quota?
Deduplicacao?


CONTA DO AZURE TEM QUE SER NO MINIMO CONTRIBUTOR
E DO AD TEM QUE TER PERMISSAO PARA ADICIONAR O STORAGE ACCOUNT AO DOMINIO

1. Gpo - update
2. Regiao
3. SKU
4. Network/private endpoint?
5. create file share
6. join AD ( add o file server no ad)
7. Install powershell 
8. download and executar scripts
9. Roles no Azure 
    smb data contributor
    smb share elevated contributor
10. Adicionar os grupos ou usuarios do AD na role smb data contributor
11. Mapear file share em um servidor local com storage account key para gerenciar o share
12. criar pasta com mesmo nome dentro do share para migracao
13. Usar robocopy para migrar e levar permissoes, com ele pode migrar aos poucos
14. robocopy \\origem Z:\destino /E /TEE /ETA /SEC /MT:32 /MIR /R:0 /W:0 /COPYALL /B /XD /LOG:C:\FS-MGT.TXT



Voces tem alguma conexao com o escritorio onde esta o file server com o azure (VPN ou ExpressRoute)?
Os usuarios irao acessar o file share somente do escritorio?
O active directory esta no azure e sincronizado com o on-premises?
