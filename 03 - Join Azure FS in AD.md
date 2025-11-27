1 - Adicionar Azure FS no AD

* Instalar o módulo de PowerShell do Azure no file server on-prem
```
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
Install-Module -Name Az -AllowClobber -Scope CurrentUser
```
* Baixar os scripts para add o Storage Account ao domínio ADDS no file server on-prem

https://github.com/Azure-Samples/azure-files-samples/releases

![alt text](img/fs30.png)

* Copiar os scripts para o file server on-prem C:\scripts\AzFilesHybrid e descompactar

* Customizar Scripts para add o Storage Account Enable AD DS authentication 

https://learn.microsoft.com/en-us/azure/storage/files/storage-files-identity-ad-ds-enable

* No powershell ISE do file server on-prem, ir ate o diretorio C:\scripts\AzFilesHybrid.

* Abrir o script storage-files-identity-ad-ds-enable.ps1 no file server on-prem e customizar os parâmetros

```
cd C:\scripts\AzFilesHybrid

PS C:\scripts\AzFilesHybrid> dir


    Directory: C:\scripts\AzFilesHybrid


Mode                LastWriteTime         Length Name                                                                                                                                                                            
----                -------------         ------ ----                                                                                                                                                                            
------        10/4/2024   9:34 PM          21597 AzFilesHybrid.psd1                                                                                                                                                              
------        10/4/2024   9:34 PM         337351 AzFilesHybrid.psm1                                                                                                                                                              
------        10/4/2024   9:34 PM          15201 CopyToPSPath.ps1  

```

* No script storage-files-identity-ad-ds-enable.ps1, execute os 3 primeiros comandos

```
# Change the execution policy to unblock importing AzFilesHybrid.psm1 module
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser

# Navigate to where AzFilesHybrid is unzipped and stored and run to copy the files into your path
.\CopyToPSPath.ps1 

# Import AzFilesHybrid module
Import-Module -Name AzFilesHybrid
```

* Faca login no Azure diretamente no navegador e depois execute o comando para conectar ao Azure que ira gerar um link e um codigo de autenticacao

```
Connect-AzAccount -DeviceCode
```

* Preencha as informacoes sobre os recursos do Azure no scrip.
* OBS, adicionar o usuario que vai executar o script nas permissoes de smb do storage account

```
Storage File Data SMB Share Contributor
Storage File Data SMB Share Elevated Contributor
```

```
$SubscriptionId = "xxxxx-d574-4e0e-b0fa-xxxxxxxx"
$ResourceGroupName = "rg-azure"
$StorageAccountName = "fsharecanada"
$SamAccountName = "fsharecanada"
$DomainAccountType = "ComputerAccount" # Default is set as ComputerAccount
# If you don't provide the OU name as an input parameter, the AD identity that represents the 
# storage account is created under the root directory.
$OuDistinguishedName = "OU=Storage,DC=rafaelmellonh,DC=com,DC=br"
# Encryption method is AES-256 Kerberos.

# Select the target subscription for the current session
Select-AzSubscription -SubscriptionId $SubscriptionId 
```

* Apos preencher as informacoes, execute o restante do script para 




```
Summary of checks:
Name                             Result 
----                             ------ 
CheckKerberosTicketEncryption    Passed 
CheckChannelEncryption           Skipped
CheckADObjectPasswordIsCorrect   Passed 
CheckADObject                    Passed 
CheckAadKerberosRegistryKeyIsOff Passed 
CheckUserFileAccess              Skipped
CheckDomainJoined                Passed 
CheckPort445Connectivity         Passed 
CheckGetKerberosTicket           Passed 
CheckDomainLineOfSight           Passed 
CheckDefaultSharePermission      Passed 
CheckStorageAccountDomainJoined  Passed 
CheckAadUserHasSid               Skipped
CheckUserRbacAssignment          Passed 
CheckSidHasAadUser               Passed 
```

* OBS, desativar a expiracao da senha do objeto no AD (storage)
```
Set-ADComputer -Identity "fsharecanada" -PasswordNeverExpires $true
Get-ADComputer "fsharecanada" -Properties PasswordNeverExpires | Select Name,PasswordNeverExpires
```

* Adicionar os usuarios/grupos do AD nas permissoes do storage account (grupos ou usuarios precisam estar sincronizados no ADConnect)

```
Storage File Data SMB Share Contributor
Storage File Data SMB Share Elevated Contributor # para admin
```

![alt text](img/sto31.png)

* Mapear a pasta do storage account no file server on-prem com o usuario que esta no grupo Storage File Data SMB Share Contributor para fazer a migracao e a gestao do file share.
Use o  ``` Storage account key ``` para mapear a pasta do storage account no file server on-prem.

* Criar os diretorios raiz dos compartilhamentos dentro do share que foi mapeado no file server on-prem para fazer a gestao e quebrar a heranca de permissoes do storage account.

![alt text](img/sto32.png)


* Testar a conexao direto ao storage de um usuario que esta no grupo Storage File Data SMB Share Contributor (\\fsharecanada.file.core.windows.net\fscanadacentral)

* Executar o robocopy para migrar os arquivos do file server on-prem para o storage account

```
robocopy \\srv-fs01\e$\RH Z:\RH /E /TEE /ETA /MT:32 /MIR /R:0 /W:0 /COPY:DATS /LOG:C:\scripts\RH.TXT
```

* Antes de ajustar as GPOs, é preciso configurar o private link e private endpoint no storage account para a conexao ser direto pela rede privada e nao pela internet.

* Antes


![alt text](img/sto33.png)

* Depois

![alt text](img/sto34.png)
