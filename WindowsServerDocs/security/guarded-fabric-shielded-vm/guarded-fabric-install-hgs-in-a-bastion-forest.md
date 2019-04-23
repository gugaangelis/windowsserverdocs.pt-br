---
title: Instalar o HGS em uma floresta de bastiões existente
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 147610d9dcb36dfedab3aca11ee1a64731715519
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842217"
---
# <a name="install-hgs-in-an-existing-bastion-forest"></a>Instalar o HGS em uma floresta de bastiões existente 

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016


## <a name="join-the-hgs-server-to-the-existing-domain"></a>Ingressar o servidor HGS no domínio existente

Em uma floresta de bastiões existente, HGS deve ser adicionado ao domínio raiz. Use o Gerenciador do servidor ou [Add-Computer](https://go.microsoft.com/fwlink/?LinkId=821564) para ingressar o servidor HGS para o domínio raiz.

## <a name="add-the-hgs-server-role"></a>Adicionar a função de servidor HGS

Execute todos os comandos neste tópico em uma sessão do PowerShell com privilégios elevados.

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

Se seu data center tiver uma floresta de bastião seguro no qual você deseja ingressar nós HGS, siga estas etapas.
Você também pode usar estas etapas para configurar clusters HGS 2 ou mais independentes que ingressaram no mesmo domínio.

## <a name="join-the-hgs-server-to-the-existing-domain"></a>Ingressar o servidor HGS no domínio existente

Use o Gerenciador do servidor ou [Add-Computer](https://go.microsoft.com/fwlink/?LinkId=821564) para ingressar nos servidores HGS para o domínio desejado.

## <a name="prepare-active-directory-objects"></a>Prepare os objetos do Active Directory

Crie uma conta de serviço gerenciado de grupo e 2 grupos de segurança.
Você pode também pré-testar os objetos de cluster se a conta que você estiver inicializando o HGS com não tem permissão para criar objetos de computador no domínio.

## <a name="group-managed-service-account"></a>Conta de serviço gerenciado de grupo

A conta de serviço gerenciado de grupo (gMSA) é a identidade usada pelo HGS para recuperar e usar seus certificados. Use [New-ADServiceAccount](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adserviceaccount) para criar uma gMSA.
Se esta for a primeira gMSA no domínio, você precisará adicionar uma chave de raiz do serviço de distribuição de chaves.

Cada nó HGS será necessário ter permissão para acessar a senha de gMSA.
A maneira mais fácil de configurar isso é criar um grupo de segurança que contém todos os nós de HGS e conceder acesso a esse grupo de segurança para recuperar a senha de gMSA.

Você deve reinicializar o servidor HGS após adicioná-lo a um grupo de segurança para garantir que ele obtém sua nova associação de grupo.

```powershell
# Check if the KDS root key has been set up
if (-not (Get-KdsRootKey)) {
    # Adds a KDS root key effective immediately (ignores normal 10 hour waiting period)
    Add-KdsRootKey -EffectiveTime ((Get-Date).AddHours(-10))
}

# Create a security group for HGS nodes
$hgsNodes = New-ADGroup -Name 'HgsServers' -GroupScope DomainLocal -PassThru

# Add your HGS nodes to this group
# If your HGS server object is under an organizational unit, provide the full distinguished name instead of "HGS01"
Add-ADGroupMember -Identity $hgsNodes -Members "HGS01"

# Create the gMSA
New-ADServiceAccount -Name 'HGSgMSA' -DnsHostName 'HGSgMSA.yourdomain.com' -PrincipalsAllowedToRetrieveManagedPassword $hgsNodes
```

A gMSA exigirá o direito de gerar eventos no log de segurança em cada servidor HGS.
Se você usar a diretiva de grupo para configurar a atribuição de direitos de usuário, certifique-se de que a conta gMSA é concedida a [gerar o privilégio de eventos de auditoria](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn221956%28v=ws.11%29) nos seus servidores HGS.

> [!NOTE]
> Contas de serviço gerenciado de grupo estão disponíveis com o esquema do Active Directory do Windows Server 2012.
> Para obter mais informações, consulte [requisitos de conta de serviço gerenciado de grupo](https://technet.microsoft.com/library/jj128431.aspx).

## <a name="jea-security-groups"></a>Grupos de segurança JEA

Ao configurar o HGS, uma [administração JEA (Just Enough)](https://aka.ms/JEAdocs) ponto de extremidade do PowerShell está configurado para permitir que os administradores a gerenciar HGS sem a necessidade de privilégios totais de administrador local.
Não é necessário para usar o JEA para gerenciar HGS, mas ele ainda deve ser configurado durante a execução HgsServer Initialize.
A configuração do ponto de extremidade JEA consiste designando 2 grupos de segurança que contêm seus administradores HGS e os revisores HGS.
Os usuários que pertencem ao grupo de administrador podem adicionar, alterar ou remover as políticas de HGS; os revisores podem apenas exibir a configuração atual.

Criar grupos de segurança 2 para esses grupos JEA usando ferramentas de administração do Active Directory ou [New-ADGroup](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adgroup).

```powershell
New-ADGroup -Name 'HgsJeaReviewers' -GroupScope DomainLocal
New-ADGroup -Name 'HgsJeaAdmins' -GroupScope DomainLocal
```

## <a name="cluster-objects"></a>Objetos de cluster

Se a conta que você está usando para configurar o HGS não tem permissão para criar novos objetos de computador no domínio, você precisará preparar antecipadamente os objetos de cluster.
Essas etapas são explicadas em [Prestage Cluster Computer Objects in Active Directory Domain Services](https://technet.microsoft.com/library/dn466519(v=ws.11).aspx).

Para configurar o primeiro nó HGS, você precisará criar um Cluster de nome do CNO (objeto) e um Virtual computador objeto VCO ().
O CNO representa o nome do cluster e é usado principalmente internamente pelo Clustering de Failover.
O VCO representa o serviço HGS que reside na parte superior do cluster e será o nome registrado com o servidor DNS.

> [!IMPORTANT]
> O usuário que executará `Initialize-HgsServer` requer **controle total** sobre os objetos CNO e VCO no Active Directory.

Para pré-configurar o CNO e VCO o rapidamente, têm um administrador do Active Directory execute os seguintes comandos do PowerShell:

```powershell
# Create the CNO
$cno = New-ADComputer -Name 'HgsCluster' -Description 'HGS CNO' -Enabled $false -Passthru

# Create the VCO
$vco = New-ADComputer -Name 'HgsService' -Description 'HGS VCO' -Passthru

# Give the CNO full control over the VCO
$vcoPath = Join-Path "AD:\" $vco.DistinguishedName
$acl = Get-Acl $vcoPath
$ace = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $cno.SID, "GenericAll", "Allow"
$acl.AddAccessRule($ace)
Set-Acl -Path $vcoPath -AclObject $acl

# Allow time for your new CNO and VCO to replicate to your other Domain Controllers before continuing
```

## <a name="security-baseline-exceptions"></a>Exceções de segurança de linha de base

Se você estiver implantando o HGS em um ambiente altamente bloqueado, determinadas configurações de diretiva de grupo podem impedir que os HGS funcionando normalmente.
Verifique seus objetos de diretiva de grupo para as seguintes configurações e siga as orientações se você for afetado:

### <a name="network-logon"></a>Logon de rede

**Caminho da política:** Atribuições de direitos do computador Configuration\Windows Settings\Security Settings\Local atribuição

**Nome da política:** Negar o acesso a este computador a partir da rede

**Valor obrigatório:** Verifique se que o valor não bloqueia a logons de rede para todas as contas locais. Você pode bloquear com segurança contas de administrador local, no entanto.

**Motivo:** Clustering de failover depende de uma conta de não-administrador local chamada CLIUSR para gerenciar nós de cluster. Bloqueio de logon de rede para este usuário impedirá que o cluster operando corretamente.

### <a name="kerberos-encryption"></a>Criptografia Kerberos

**Caminho da política:** Configuração do Computador\Configurações do Windows\Configurações de Segurança\Políticas Locais\Opções de Segurança

**Nome da política:** Segurança de rede: Configurar tipos de criptografia permitidos para Kerberos

**Ação**: Se essa política está configurada, você deve atualizar a conta gMSA com [Set-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/set-adserviceaccount?view=win10-ps) usar somente os tipos de criptografia com suporte nesta política. Por exemplo, se sua política permite apenas AES128\_HMAC\_SHA1 e AES256\_HMAC\_SHA1, você deve executar `Set-ADServiceAccount -Identity HGSgMSA -KerberosEncryptionType AES128,AES256`.



## <a name="next-steps"></a>Próximas etapas

- Para as próximas etapas para configurar o atestado de TPM, consulte [inicializar o cluster HGS usando o modo TPM em uma floresta de bastiões existente](guarded-fabric-initialize-hgs-tpm-mode-bastion.md).
- Para as próximas etapas para configurar o atestado de chaves do host, consulte [inicializar o cluster HGS usando o modo de chave em uma floresta de bastiões existente](guarded-fabric-initialize-hgs-key-mode-bastion.md).
- Para as próximas etapas para configurar o Atestado baseado em Admin (substituído no Windows Server 2019), consulte [inicializar o cluster HGS usando modo AD em uma floresta de bastiões existente](guarded-fabric-initialize-hgs-ad-mode-bastion.md).

