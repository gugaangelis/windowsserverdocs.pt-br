---
title: Delegar acesso AD FS do commandlet do PowerShell a usuários não administradores
description: Este documento descirbes como delegar permissões a não-administradores para AD FS PowerShell cmdlts.
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: zhvolosh
ms.date: 01/31/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 86bbb562e223fdf61dac3ce5646d97a57b2eba4c
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546309"
---
# <a name="delegate-ad-fs-powershell-commandlet-access-to-non-admin-users"></a>Delegar acesso AD FS do commandlet do PowerShell a usuários não administradores 
Por padrão, a administração do AD FS por meio do PowerShell só pode ser realizada por administradores do AD FS. Para muitas organizações de grande porte, isso pode não ser um modelo operacional viável ao lidar com outras pessoas, como uma equipe de suporte técnico.  

Com a JEA (administração suficiente), os clientes agora podem delegar commandlets específicas a grupos de pessoal diferentes.  
Um bom exemplo desse caso de uso é permitir que a equipe de suporte técnico consulte AD FS status de bloqueio de conta e redefinir o estado de bloqueio de conta em AD FS depois que um usuário tiver sido verificadosdo. Nesse caso, o commandlets que precisaria ser delegado é: 
- `Get-ADFSAccountActivity`
- `Set-ADFSAccountActivity` 
- `Reset-ADFSAccountLockout` 

Usamos este exemplo no restante deste documento. No entanto, é possível personalizá-lo para também permitir que a delegação defina propriedades de terceiras partes confiáveis e entregue a ela os proprietários dos aplicativos na organização.  


##  <a name="create-the-required-groups-necessary-to-grant-users-permissions"></a>Criar os grupos necessários necessários para conceder permissões aos usuários 
1. Crie uma [conta de serviço gerenciado de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview). A conta gMSA é usada para permitir que o usuário do JEA acesse recursos de rede como outros computadores ou serviços Web. Ele fornece uma identidade de domínio que pode ser usada para autenticar os recursos em qualquer computador dentro do domínio. A conta gMSA recebe os direitos administrativos necessários mais tarde na instalação. Para este exemplo, chamamos a conta **gMSAContoso**. 
2. Criar um grupo de Active Directory pode ser preenchido com usuários que precisam receber os direitos aos comandos delegados. Neste exemplo, a equipe de suporte técnico recebe permissões para ler, atualizar e redefinir o estado de bloqueio do ADFS. Nós nos referimos a esse grupo em todo o exemplo como **JEAContoso**. 

### <a name="install-the-gmsa-account-on-the-adfs-server"></a>Instale a conta gMSA no servidor ADFS: 
Crie uma conta de serviço que tenha direitos administrativos para os servidores ADFS. Isso pode ser realizado no controlador de domínio ou remotamente, desde que o pacote do AD RSAT esteja instalado.  A conta de serviço deve ser criada na mesma floresta que o servidor ADFS. Modifique os valores de exemplo para a configuração do seu farm. 

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

Instale a conta gMSA no servidor ADFS.  Isso precisa ser executado em todos os nós do ADFS no farm. 
 
```powershell
Install-ADServiceAccount gMSAcontoso 
```

### <a name="grant-the-gmsa-account-admin-rights"></a>Conceder os direitos de administrador da conta do gMSA 
Se o farm estiver usando a administração delegada, conceda os direitos de administrador da conta do gMSA adicionando-o ao grupo existente que tem acesso de administrador delegado.  
 
Se o farm não estiver usando a administração delegada, conceda os direitos de administrador da conta do gMSA, tornando-o o grupo de administração local em todos os servidores ADFS. 
 
 
### <a name="create-the-jea-role-file"></a>Criar o arquivo de função JEA 
 
No AD FS Server, crie a função JEA em um arquivo do bloco de notas. As instruções para criar a função são fornecidas em [recursos de função Jea](https://docs.microsoft.com/powershell/jea/role-capabilities). 
 
O commandlets delegado neste exemplo é `Reset-AdfsAccountLockout, Get-ADFSAccountActivity, and Set-ADFSAccountActivity`. 

Função JEA de exemplo delegando o acesso de "Reset-ADFSAccountLockout", "Get-ADFSAccountActivity" e "Set-ADFSAccountActivity" commandlets:

```powershell
@{
GUID = 'b35d3985-9063-4de5-81f8-241be1f56b52'
ModulesToImport = 'adfs'
VisibleCmdlets = 'Reset-AdfsAccountLockout', 'Get-ADFSAccountActivity', 'Set-ADFSAccountActivity'
}
```


### <a name="create-the-jea-session-configuration-file"></a>Criar o arquivo de configuração de sessão JEA 
Siga as instruções para criar o arquivo de [configuração de sessão Jea](https://docs.microsoft.com/powershell/jea/session-configurations) . O arquivo de configuração determina quem pode usar o ponto de extremidade JEA e quais recursos eles têm acesso. 

Os recursos de função são referenciados pelo nome simples (nome de arquivo sem a extensão) do arquivo de capacidade de função. Se vários recursos de função estiverem disponíveis no sistema com o mesmo nome simples, o PowerShell usará sua ordem de pesquisa implícita para selecionar o arquivo de capacidade de função efetivo. Ele não dá acesso a todos os arquivos de capacidade de função com o mesmo nome. 

Para especificar um arquivo de capacidade de função com um caminho, `RoleCapabilityFiles` use o argumento. Para uma subpasta, Jea procura módulos válidos do PowerShell que contenham uma `RoleCapabilities` subpasta, em que o `RoleCapabilityFiles` argumento `RoleCapabilities`deve ser modificado para ser. 

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
 
É altamente recomendável [testar o arquivo de configuração de sessão](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Test-PSSessionConfigurationFile?view=powershell-5.1) se você editou o arquivo PSSC manualmente usando um editor de texto para garantir que a sintaxe esteja correta. Se um arquivo de configuração de sessão não passar nesse teste, ele não será registrado com êxito no sistema.  
 
### <a name="install-the-jea-session-configuration-on-the-ad-fs-server"></a>Instalar a configuração de sessão JEA no servidor de AD FS 

Instalar a configuração de sessão JEA no servidor de AD FS 
 
```powershell
Register-PSSessionConfiguration -Path .\JEASessionConfig.pssc -name "AccountActivityAdministration" -force
``` 
## <a name="operational-instructions"></a>Instruções operacionais 
Uma vez configurado, o registro em log JEA e a auditoria podem ser usados para determinar se os usuários corretos têm acesso ao ponto de extremidade JEA. 

Para usar os comandos delegados: 

```powershell
Enter-pssession -ComputerName server01.contoso.com -ConfigurationName "AccountActivityAdministration" -Credential <User Using JEA> 
Get-AdfsAccountActivity <User> 


```
