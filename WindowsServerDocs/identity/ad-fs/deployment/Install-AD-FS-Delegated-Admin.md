---
ms.assetid: 46725afe-8652-4cd7-928c-93b98f7fbae3
title: Criando um farm de AD FS sem privilégios de administrador de domínio
description: Usando o cmdlet Install-AdfsFarm e o script para criar um farm de AD FS usando credenciais de administrador delegadas
author: billmath
ms.author: billmath
manager: daveba
ms.date: 04/01/2020
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1716c1e3684ed09d051970a2ab3b43938b5eeb5e
ms.sourcegitcommit: 20d07170c7f3094c2fb4455f54b13ec4b102f2d7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81269437"
---
# <a name="creating-an-ad-fs-farm-without-domain-admin-privileges"></a>Criando um farm de AD FS sem privilégios de administrador de domínio

>Aplica-se a: Windows Server 2019 e 2016

## <a name="overview"></a>Visão geral
A partir do AD FS no Windows Server 2016, você pode executar o cmdlet Install-AdfsFarm como um administrador local no servidor de Federação, desde que o administrador do domínio tenha preparado Active Directory.  O script a seguir neste artigo pode ser usado para preparar o AD.  As etapas são as seguintes:

1) Como administrador de domínio, execute o script (ou crie o Active Directory objetos e permissões manualmente).
2) O script retornará um objeto AdminConfiguration contendo o DN do objeto do AD recém-criado
3) No servidor de Federação, execute o cmdlet Install-AdfsFarm enquanto estiver conectado como um administrador local, passando o objeto de #2 acima como o parâmetro AdminConfiguration

## <a name="assumptions"></a>Suposições
- Contoso\localadmin é um administrador interno de administrador não pertencente ao domínio no servidor de Federação
- Contoso\FsSvcAcct é uma conta de domínio que será a conta de serviço AD FS
- Contoso\FsGmsaAcct $ é uma conta gMSA que será a conta de serviço AD FS
- $svcCred são as credenciais da conta de serviço do AD FS
- $localAdminCred são as credenciais da conta de administrador local (não DA) no servidor de Federação

## <a name="using-a-domain-account-as-ad-fs-service-account"></a>Usando uma conta de domínio como AD FS conta de serviço
### <a name="prepare-ad"></a>Preparar o AD
Execute o seguinte como administrador de domínio
```
PS:\>$adminConfig=(.\New-AdfsDkmContainer.ps1 -ServiceAccount contoso\fssvcacct -AdfsAdministratorAccount contoso\localadmin)
```
### <a name="sample-output"></a>Saída de exemplo
```
$adminconfig.DkmContainerDN
CN=9530440c-bc84-4fe6-a3f9-8d60162a7bcf,CN=ADFS,CN=Microsoft,CN=Program Data,DC=contoso,DC=com
```
### <a name="create-the-ad-fs-farm"></a>Criar o farm de AD FS
No servidor de Federação como um administrador local, execute o seguinte em uma janela de comando do PowerShell com privilégios elevados.

Primeiro, se o administrador do servidor de Federação não estiver usando a mesma sessão do PowerShell que o administrador de domínio acima, crie novamente o objeto adminConfig usando a saída do acima.
```
PS:\>$adminConfig = @{"DKMContainerDn"="CN=9530440c-bc84-4fe6-a3f9-8d60162a7bcf,CN=ADFS,CN=Microsoft,CN=Program Data,DC=contoso,DC=com"}
```

Em seguida, crie o farm:
```
PS:\>$svcCred = (get-credential)
PS:\>$localAdminCred = (get-credential)
PS:\>Install-AdfsFarm -CertificateThumbprint 270D041785C579D75C1C981DA0F9C36ECFDB65E0 -FederationServiceName "fs.contoso.com" -ServiceAccountCredential $svcCred -Credential $localAdminCred -OverwriteConfiguration -AdminConfiguration $adminConfig -Verbose
```
## <a name="using-a-gmsa-as-the-ad-fs-service-account"></a>Usando um gMSA como a conta de serviço AD FS
### <a name="prepare-ad"></a>Preparar o AD
```
PS:\>$adminConfig=(.\New-AdfsDkmContainer.ps1 -ServiceAccount contoso\FsGmsaAcct$ -AdfsAdministratorAccount contoso\localadmin)
```

### <a name="sample-output"></a>Saída de exemplo
```
$adminconfig.DkmContainerDN
CN=8065f653-af9d-42ff-aec8-56e02be4d5f3,CN=ADFS,CN=Microsoft,CN=Program Data,DC=contoso,DC=com
```

### <a name="create-the-ad-fs-farm"></a>Criar o farm de AD FS
No servidor de Federação como um administrador local, execute o seguinte em uma janela de comando do PowerShell com privilégios elevados.

Primeiro, se o administrador do servidor de Federação não estiver usando a mesma sessão do PowerShell que o administrador de domínio acima, crie novamente o objeto adminConfig usando a saída do acima.
```
PS:\>$adminConfig = @{"DKMContainerDn"="CN=8065f653-af9d-42ff-aec8-56e02be4d5f3,CN=ADFS,CN=Microsoft,CN=Program Data,DC=contoso,DC=com"}
```

Em seguida, crie o farm: Observe que a conta do computador local e a conta do administrador do ADFS precisam receber a permissão recuperar senha e delegar aos direitos de conta no gMSA.
```
PS:\>$localadminobj = get-aduser "localadmin"
PS:\>$adfsnodecomputeracct = get-adcomputer "contoso_adfs_node"
PS:\>Set-ADServiceAccount -Identity fsgmsaacct -PrincipalsAllowedToRetrieveManagedPassword @( add=$localadmin.sid.value, $computeracct.sid.value) -PrincipalsAllowedToDelegateToAccount @( add=$localadmin.sid.value, $computeracct.sid.value)
PS:\>$localAdminCred = (get-credential)
PS:\>Install-AdfsFarm -CertificateThumbprint 270D041785C579D75C1C981DA0F9C36ECFDB65E0 -FederationServiceName "fs.contoso.com" -Credential $localAdminCred -GroupServiceAccountIdentifier "contoso\fsgmsaacct$" -OverwriteConfiguration -AdminConfiguration $adminConfig
```

## <a name="script-for-preparing-ad"></a>Script para preparar o AD
O script do PowerShell a seguir pode ser usado para realizar os exemplos acima

```
[CmdletBinding(SupportsShouldProcess=$true)]
param (
   [Parameter(Mandatory=$True)]
   [string]$ServiceAccount,
   [Parameter(Mandatory=$True)]
   [string]$AdfsAdministratorAccount
)

$ServiceAccountSplit = $ServiceAccount.Split("\");
if ($ServiceAccountSplit.Length -ne 2)
{
    Write-error "Specify the ServiceAccount identifier in 'domain\username' format"
    exit 1
}

$AdfsAdministratorAccountSplit = $AdfsAdministratorAccount.Split("\");
if ($AdfsAdministratorAccountSplit.Length -ne 2)
{
    Write-error "Specify the AdfsAdministratorAccount identifier in 'domain\username' format"
    exit 1
}

#######################################
## Verify AD module is installed
#######################################
$m = "ActiveDirectory"
if (Get-Module | Where-Object {$_.Name -eq $m})
{
    write-verbose "Module $m is already imported."
}
else
{
    if (Get-Module -ListAvailable | Where-Object {$_.Name -eq $m})
    {
        Import-Module $m -Verbose
    }
    else
    {
        write-error "Module $m was not imported, install the Active Directory RSAT package and retry."
        exit 1
    }
}

push-location ad:

#######################################
## Generate random DKM container name
## The OU Name is a randomly generated Guid
#######################################
[string]$guid = [Guid]::NewGuid()
write-verbose ("OU Name" + $guid)

$ouName = $guid
$initialPath = "CN=Microsoft,CN=Program Data," + (Get-ADDomain).DistinguishedName
$ouPath = "CN=ADFS," + $initialPath
$ou = "CN=" + $ouName + "," + $ouPath

#######################################
## Create DKM container and assign default ACE which allows adfs admin read access
#######################################

if ($pscmdlet.ShouldProcess("$ou", "Creating DKM container and assinging access"))
{
    Write-Verbose ("Creating organizational unit with DN: " + $ou)

    if ($AdfsAdministratorAccount.EndsWith("$"))
    {
        write-verbose "ADFS administrator account passed with $ suffix indicating a computer account"
        $userNameSplit = $AdfsAdministratorAccount.Split("\");
        $strSID = (Get-ADServiceAccount -Identity $userNameSplit[1]).SID
    }
    else
    {
        write-verbose "ADFS administrator account is a standard AD user"    
        $objUser = New-Object System.Security.Principal.NTAccount($AdfsAdministratorAccount)
        $strSID = $objUser.Translate([System.Security.Principal.SecurityIdentifier])
    }

    if ($null -eq (Get-ADObject -Filter {distinguishedName -eq $ouPath}))
    {
        Write-Verbose ("First creating initial path " + $ouPath)
        New-ADObject -Name "ADFS" -Type Container -Path $initialPath
    }

    $acl = get-acl -Path $ouPath
    [System.DirectoryServices.ActiveDirectorySecurityInheritance]$adSecInEnum = [System.DirectoryServices.ActiveDirectorySecurityInheritance]::All
    $ace1 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"GenericRead","Allow",$adSecInEnum

    $acl.AddAccessRule($ace1)
    set-acl -Path $ouPath -AclObject $acl

    New-ADObject -Name $ouName -Type Container -Path $ouPath
}


#######################################
## Grant the following permission to the service account
# Read
# Create Child
# Write Owner
# Delete Tree
# Write DACL
# Write Property
#######################################
if ($ServiceAccount.EndsWith("$"))
{
    write-verbose "service account passed with $ suffix indicating a gMSA"
    $userNameSplit = $ServiceAccount.Split("\");
    $strSID = (Get-ADServiceAccount -Identity $userNameSplit[1]).SID
}
else
{
    write-verbose "service account is a standard AD user"
    $objUser = New-Object System.Security.Principal.NTAccount($ServiceAccount)
    $strSID = $objUser.Translate([System.Security.Principal.SecurityIdentifier])
}

if ($pscmdlet.ShouldProcess("$strSID", "Granting GenericRead, CreateChild, WriteOwner, DeleteTree, WriteDacl and WriteProperty"))
{
    [System.DirectoryServices.ActiveDirectorySecurityInheritance]$adSecInEnum = [System.DirectoryServices.ActiveDirectorySecurityInheritance]::All
    $ace1 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"GenericRead","Allow",$adSecInEnum
    $ace2 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"CreateChild","Allow",$adSecInEnum
    $ace3 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"WriteOwner","Allow",$adSecInEnum
    $ace4 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"DeleteTree","Allow",$adSecInEnum
    $ace5 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"WriteDacl","Allow",$adSecInEnum
    $ace6 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"WriteProperty","Allow",$adSecInEnum

    $acl = get-acl -Path $ou

    $acl.AddAccessRule($ace1)
    $acl.AddAccessRule($ace2)
    $acl.AddAccessRule($ace3)
    $acl.AddAccessRule($ace4)
    $acl.AddAccessRule($ace5)
    $acl.AddAccessRule($ace6)

    $acl.SetOwner($strSID)

    set-acl -Path $ou -AclObject $acl
}

#######################################
## Grant the following permission to the adfs admin account
# Read
# Create Child
# Write Owner
# Delete Tree
# Write DACL
# Write Property
#######################################

if ($AdfsAdministratorAccount.EndsWith("$"))
{
    write-verbose "ADFS administrator account passed with $ suffix indicating a gMSA"
    $userNameSplit = $AdfsAdministratorAccount.Split("\");
    $strSID = (Get-ADServiceAccount -Identity $userNameSplit[1]).SID
}
else
{
    write-verbose "ADFS administrator account is a standard AD user"
    $objUser = New-Object System.Security.Principal.NTAccount($AdfsAdministratorAccount)
    $strSID = $objUser.Translate([System.Security.Principal.SecurityIdentifier])
}

if ($pscmdlet.ShouldProcess("$strSID", "Granting GenericRead, CreateChild, WriteOwner, DeleteTree, WriteDacl and WriteProperty"))
{
    [System.DirectoryServices.ActiveDirectorySecurityInheritance]$adSecInEnum = [System.DirectoryServices.ActiveDirectorySecurityInheritance]::All
    $ace1 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"GenericRead","Allow",$adSecInEnum
    $ace2 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"CreateChild","Allow",$adSecInEnum
    $ace3 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"WriteOwner","Allow",$adSecInEnum
    $ace4 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"DeleteTree","Allow",$adSecInEnum
    $ace5 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"WriteDacl","Allow",$adSecInEnum
    $ace6 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"WriteProperty","Allow",$adSecInEnum

    $acl = get-acl -Path $ou

    $acl.AddAccessRule($ace1)
    $acl.AddAccessRule($ace2)
    $acl.AddAccessRule($ace3)
    $acl.AddAccessRule($ace4)
    $acl.AddAccessRule($ace5)
    $acl.AddAccessRule($ace6)

    $acl.SetOwner($strSID)

    set-acl -Path $ou -AclObject $acl

    $adminConfig = @{"DKMContainerDn"=$ou}

    Write-Output $adminConfig
}

pop-location

```
