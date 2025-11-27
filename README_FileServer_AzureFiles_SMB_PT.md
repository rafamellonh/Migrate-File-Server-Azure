# README — Migração de File Server Windows para Azure Files (SMB)

## Objetivo
Este documento descreve todo o processo necessário para migrar um File Server Windows Server para o Azure Files SMB, preservando permissões NTFS e garantindo continuidade operacional.

## 1. Checklist inicial — Verificações no File Server atual

### 1.1 Compartilhamentos SMB
```
Get-SmbShare
```

### 1.2 Permissões NTFS
Exportar ACLs:
```
icacls D:\Shares /save C:\ACLs_Backup.txt /t
```

### 1.3 Shadow Copies
```
vssadmin list shadows
vssadmin list shadowstorage
```

### 1.4 Sessões SMB
```
Get-SmbSession
Get-SmbOpenFile
```

### 1.5 Tamanho dos dados
```
Get-ChildItem D:\Shares -Recurse | Measure-Object -Sum Length
```

## 2. Preparação no Azure

### 2.1 Criar Storage Account
- Tipo: StorageV2
- Redundância: LRS ou ZRS
- SMB habilitado
- AD DS habilitado

### 2.2 Configurar autenticação com Active Directory
Comando:
```
Join-AzStorageAccountForAuth ...
```

## 3. Criar compartilhamentos no Azure Files
Via portal → File Shares → + Create.

## 4. Aplicar permissões NTFS no Azure Files
Montar e restaurar ACLs:
```
icacls Z:\ /restore C:\ACLs_Backup.txt
```

## 5. Migração dos dados com Robocopy
```
robocopy "D:\Shares" "Z:\" /MIR /COPYALL /MT:32 /R:2 /W:2 /LOG:robolog.txt
```

## 6. Testes finais
- Testar acesso SMB
- Verificar permissões
- Testar bloqueio de arquivos
- Atualizar GPO

## 7. Desativar File Server antigo
```
Get-SmbShare | Remove-SmbShare -Force
```

## 8. Checklist final
| Item | OK |
|------|----|
| Shares documentados | ✔ |
| ACLs exportadas | ✔ |
| Azure Files configurado | ✔ |
| Dados migrados | ✔ |
| Testes concluídos | ✔ |
| GPO ajustada | ✔ |

