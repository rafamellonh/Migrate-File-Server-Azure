Get-SmbShare | Select-Object Name, Path, Description, ScopeName
(Get-Acl "D:\Shares\Financeiro").Access
icacls D:\Shares /save C:\ACLs_Backup.txt /t
Get-FsrmQuota | Select-Object Path, Size, SoftLimit

Get-SmbSession
Get-SmbOpenFile


Get-Volume
Get-Disk
