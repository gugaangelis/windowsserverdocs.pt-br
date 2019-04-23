---
title: Delegar o acesso do Commandlet do Powershell do AD FS para usuários não administradores
description: Este documento descirbes como delegar permissões para administradores para cmdlets do PowerShell do AD FS.
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: zhvolosh
ms.date: 01/31/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 265d22b045011e34192e56bdb970955b601cda56
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883557"
---
# <a name="delegate-ad-fs-powershell-commandlet-access-to-non-admin-users"></a>Delegar o acesso do Commandlet do Powershell do AD FS para usuários não administradores 
Por padrão, a administração do AD FS por meio do PowerShell só pode ser realizada por administradores do AD FS. Para muitas organizações grandes, isso não pode ser um modelo operacional viável ao lidar com outras pessoas, como uma equipe de suporte técnico.  

Com administração JEA (Just Enough), os clientes agora podem delegar commandlets específicos para grupos de uma equipe diferente.  
Um bom exemplo desse caso de uso é permitir que a equipe de suporte técnico a consulta do AD FS status de bloqueio de conta e redefinir o estado de bloqueio de conta no AD FS depois que um usuário foi examinado. Nesse caso, os cmdlets que precisam ser delegadas são: 
- `Get-ADFSAccountActivity`
- `Set-ADFSAccountActivity` 
- `Reset-ADFSAccountLockout` 

Podemos usar esse exemplo no restante deste documento. No entanto, você pode personalizar isso para permitir também que a delegação definir propriedades de terceiras partes confiáveis e esse recurso para os proprietários do aplicativo dentro da organização.  


##  <a name="create-the-required-groups-necessary-to-grant-users-permissions"></a>Criar os grupos necessários necessário conceder permissões a usuários 
1. Criar uma [conta de serviço gerenciado de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview). A conta gMSA é usada para permitir que o usuário JEA acesse recursos de rede como outros computadores ou serviços da web. Ele fornece uma identidade de domínio que pode ser usada para autenticar para recursos em qualquer computador no domínio. A conta gMSA recebe os direitos administrativos necessários posteriormente na instalação. Neste exemplo, chamamos a conta **gMSAContoso**. 
2. Criar um Active Directory grupo pode ser populado com os usuários que precisam ter os direitos para os comandos de delegado. Neste exemplo, a equipe de suporte técnico é concedidas permissões para ler, atualizar e redefinir o estado de bloqueio do AD FS. Nós nos referimos a esse grupo em todo o exemplo de como **JEAContoso**. 

### <a name="install-the-gmsa-account-on-the-adfs-server"></a>Instale a conta gMSA no servidor ADFS: 
Crie uma conta de serviço que tenha direitos administrativos para os servidores do AD FS. Isso pode ser feito no controlador de domínio ou remotamente, desde que o pacote RSAT AD esteja instalado.  A conta de serviço deve ser criada na mesma floresta que o servidor ADFS. Modifique os valores de exemplo para a configuração do seu farm. 

```powershell
 # This command should only be run if this is the first time gMSA accounts are enabled in the forest 
Add-KdsRootKey -EffectiveTime ((get-date).addhours(-10))  
 
# Run this on every node that you want to have JEA configured on  
$adfsServer = Get-ADComputer server01.contoso.com  
 
# Run targeted at domain controller  
$serviceaccount = New-ADServiceAccount gMSAcontoso -DNSHostName <FQDN of the domain containing the KDS key> - PrincipalsAllowedToRetrieveManagedPassword $adfsServer –passthru 
 
# Run this on every node 
Add-ADComputerServiceAccount -Identity server01.contoso.com -ServiceAccount $ServiceAccount 
```

Instale a conta gMSA no servidor ADFS.  Isso precisa ser executado em cada nó do AD FS no farm. 
 
```powershell
Install-ADServiceAccount gMSAcontoso 
```

### <a name="grant-the-gmsa-account-admin-rights"></a>Conceder direitos de administrador da conta gMSA 
Se o farm estiver usando a administração delegada, conceda a gMSA direitos de administrador de conta ao adicioná-lo ao grupo existente que tem acesso de administrador delegado.  
 
Se o farm não estiver usando a administração delegada, conceda a gMSA direitos de administrador da conta, tornando o grupo de administração local em todos os servidores do AD FS. 
 
 
### <a name="create-the-jea-role-file"></a>Criar o arquivo de função JEA 
 
Crie a função do JEA em um arquivo do bloco de notas. Instruções para criar a função é fornecida em [recursos de função JEA](https://docs.microsoft.com/powershell/jea/role-capabilities). 
 
Os commandlets delegados neste exemplo são `Reset-AdfsAccountLockout, Get-ADFSAccountActivity, and Set-ADFSAccountActivity`. 

Função JEA exemplo Delegar acesso dos commandlets 'Redefinição ADFSAccountLockout', 'Get-ADFSAccountActivity' e 'Set-ADFSAccountActivity':

```powershell
@{
GUID = 'b35d3985-9063-4de5-81f8-241be1f56b52'
ModulesToImport = 'adfs'
VisibleCmdlets = 'Reset-AdfsAccountLockout', 'Get-ADFSAccountActivity', 'Set-ADFSAccountActivity'
}
```


### <a name="create-the-jea-session-configuration-file"></a>Criar o arquivo de configuração de sessão JEA 
Siga as instruções para criar o [configuração de sessão JEA](https://docs.microsoft.com/powershell/jea/session-configurations) arquivo. O arquivo de configuração determina quem pode usar o ponto de extremidade JEA e quais recursos eles têm acesso. 

Capacidades de função são referenciadas pelo nome simples (nome de arquivo sem a extensão) do arquivo de capacidade de função. Se vários recursos de função estão disponíveis no sistema com o mesmo nome simples, o PowerShell usará sua ordem de pesquisa implícita para selecionar o arquivo de capacidade de função efetivo. Ele não dá acesso a todos os arquivos de recurso de função com o mesmo nome. 

Para especificar um arquivo de capacidade de função com um caminho, use o `RoleCapabilityFiles` argumento. Para uma subpasta, o JEA procurará módulos válidos do Powershell que contêm uma `RoleCapabilities` subpasta, onde o `RoleCapabilityFiles` argumento deve ser modificado para ser `RoleCapabilities`. 

Arquivo de configuração de sessão de exemplo: 

```powershell
@{
SchemaVersion = '2.0.0.0'
GUID = '54c8d41b-6425-46ac-a2eb-8c0184d9c6e6'
SessionType = 'RestrictedRemoteServer'
GroupManagedServiceAccount =  'CONTOSO\gMSAcontoso'
RoleDefinitions = @{ JEAcontoso = @{ RoleCapabilityFiles = 'C:\Program Files\WindowsPowershell\Modules\AccountActivityJEA\RoleCapabilities\JEAAccountActivityResetRole.psrc' } }
}
```

Salve o arquivo de configuração de sessão. 
 
É altamente recomendável [testar seu arquivo de configuração de sessão](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Test-PSSessionConfigurationFile?view=powershell-5.1) se você editou o arquivo pssc manualmente usar um editor de texto para garantir que a sintaxe está correta. Se um arquivo de configuração de sessão não passar nesse teste, ele não está registrado com êxito no sistema.  
 
### <a name="install-the-jea-session-configuration-on-the-ad-fs-server"></a>Instalar a configuração de sessão JEA no servidor do AD FS 

Instalar a configuração de sessão JEA no servidor do AD FS 
 
```powershell
Register-PSSessionConfiguration -Path .\JEASessionConfig.pssc -name "AccountActivityAdministration" -force
``` 
## <a name="operational-instructions"></a>Instruções operacionais 
Uma vez configurado, JEA, registrando em log e auditoria pode ser usado para determinar se os usuários corretos tenham acesso ao ponto de extremidade JEA. 

Para usar os comandos de delegado: 

```powershell
Enter-pssession -ComputerName server01.contoso.com -ConfigurationName "AccountActivityAdministration" -Credential <User Using JEA> 
Get-AdfsAccountActivity <User> 
```
