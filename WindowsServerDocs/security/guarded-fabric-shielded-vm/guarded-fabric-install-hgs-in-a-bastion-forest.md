---
title: Instalar o HGS em uma floresta de bastiões existente
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 4e4bdf9c33d4511c470da50462469fadbd0641ce
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996233"
---
# <a name="install-hgs-in-an-existing-bastion-forest"></a>Instalar o HGS em uma floresta de bastiões existente

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016


## <a name="join-the-hgs-server-to-the-existing-domain"></a>Ingressar o servidor HGS no domínio existente

Em uma floresta de bastiões existente, o HGS deve ser adicionado ao domínio raiz. Use Gerenciador do Servidor ou [Add-Computer](https://go.microsoft.com/fwlink/?LinkId=821564) para ingressar o servidor HgS no domínio raiz.

## <a name="add-the-hgs-server-role"></a>Adicionar a função de servidor HGS

Execute todos os comandos neste tópico em uma sessão do PowerShell com privilégios elevados.

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)]

Se o seu datacenter tiver uma floresta de bastiões segura em que você deseja ingressar nos nós HGS, siga estas etapas.
Você também pode usar estas etapas para configurar dois ou mais clusters HGS independentes que ingressaram no mesmo domínio.

## <a name="join-the-hgs-server-to-the-existing-domain"></a>Ingressar o servidor HGS no domínio existente

Use Gerenciador do Servidor ou [Add-Computer](https://go.microsoft.com/fwlink/?LinkId=821564) para unir os servidores HgS ao domínio desejado.

## <a name="prepare-active-directory-objects"></a>Preparar objetos Active Directory

Crie uma conta de serviço gerenciado de grupo e 2 grupos de segurança.
Você também pode pré-testar os objetos de cluster se a conta para a qual você está inicializando o HGS não tiver permissão para criar objetos de computador no domínio.

## <a name="group-managed-service-account"></a>Conta de serviço gerenciado de grupo

A conta de serviço gerenciado de grupo (gMSA) é a identidade usada pelo HGS para recuperar e usar seus certificados. Use [New-ADServiceAccount](/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps) para criar um gMSA.
Se esse for o primeiro gMSA no domínio, será necessário adicionar uma chave raiz do serviço de distribuição de chaves.

Cada nó HGS precisará receber permissão para acessar a senha gMSA.
A maneira mais fácil de configurar isso é criar um grupo de segurança que contenha todos os nós HGS e conceder a esse grupo de segurança acesso para recuperar a senha gMSA.

Você deve reinicializar o servidor HGS depois de adicioná-lo a um grupo de segurança para garantir que ele obtenha sua nova associação de grupo.

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

O gMSA precisará do direito de gerar eventos no log de segurança em cada servidor HGS.
Se você usar Política de Grupo para configurar a atribuição de direitos de usuário, verifique se a conta gMSA recebeu o [privilégio gerar eventos de auditoria](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn221956%28v=ws.11%29) nos servidores HgS.

> [!NOTE]
> Contas de serviço gerenciado de grupo estão disponíveis a partir do esquema de Active Directory do Windows Server 2012.
> Para obter mais informações, consulte [requisitos de conta de serviço gerenciado de grupo](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj128431(v=ws.11)).

## <a name="jea-security-groups"></a>Grupos de segurança do JEA

Quando você configura o HGS, um ponto de extremidade do PowerShell de [Administração Jea (apenas suficiente)](https://aka.ms/JEAdocs) é configurado para permitir que os administradores gerenciem o HgS sem a necessidade de privilégios totais de administrador local.
Não é necessário usar o JEA para gerenciar o HGS, mas ele ainda deve ser configurado durante a execução de Initialize-HgsServer.
A configuração do ponto de extremidade JEA consiste em designar 2 grupos de segurança que contêm seus administradores HGS e os revisores HGS.
Os usuários que pertencem ao grupo de administradores podem adicionar, alterar ou remover políticas no HGS; os revisores só podem exibir a configuração atual.

Crie dois grupos de segurança para esses grupos de JEA usando as ferramentas de administração Active Directory ou [New-ADGroup](/powershell/module/addsadministration/new-adgroup?view=win10-ps).

```powershell
New-ADGroup -Name 'HgsJeaReviewers' -GroupScope DomainLocal
New-ADGroup -Name 'HgsJeaAdmins' -GroupScope DomainLocal
```

## <a name="cluster-objects"></a>Objetos de cluster

Se a conta que você está usando para configurar o HGS não tiver permissão para criar novos objetos de computador no domínio, será necessário pré-testar os objetos de cluster.
Essas etapas são explicadas em pré-configurar [objetos de computador de cluster no Active Directory Domain Services](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn466519(v=ws.11)).

Para configurar seu primeiro nó HGS, será necessário criar um objeto de nome de cluster (CNO) e um objeto de computador virtual (VCO).
O CNO representa o nome do cluster e é usado principalmente internamente pelo clustering de failover.
O VCO representa o serviço HGS que reside na parte superior do cluster e será o nome registrado no servidor DNS.

> [!IMPORTANT]
> O usuário que executará o `Initialize-HgsServer` precisará de **controle total** sobre os objetos CNO e VCO no Active Directory.

Para pré-configurar rapidamente o CNO e o VCO, faça com que um administrador de Active Directory execute os seguintes comandos do PowerShell:

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

## <a name="security-baseline-exceptions"></a>Exceções de linha de base de segurança

Se você estiver implantando o HGS em um ambiente altamente bloqueado, determinadas configurações de Política de Grupo poderão impedir que o HGS opere normalmente.
Verifique os objetos do Política de Grupo para as seguintes configurações e siga as orientações se você for afetado:

### <a name="network-logon"></a>Logon de rede

**Caminho da política:** Computador \ \ \ Configurações de direitos de \ \ \ \ tarefas

**Nome da política:** Negar acesso a este computador pela rede

**Valor obrigatório:** Verifique se o valor não bloqueia os logons de rede para todas as contas locais. No entanto, é possível bloquear com segurança as contas de administrador local.

**Motivo:** O clustering de failover depende de uma conta local que não seja de administrador chamada CLIUSR para gerenciar nós de cluster. Bloquear o logon de rede para este usuário impedirá que o cluster opere corretamente.

### <a name="kerberos-encryption"></a>Criptografia Kerberos

**Caminho da política:** Opções de \ \ Configurações do computador \ \ \ Configurações

**Nome da política:** Segurança de rede: configurar tipos de criptografia permitidos para Kerberos

**Ação**: se essa política estiver configurada, você deverá atualizar a conta GMSA com [set-ADServiceAccount](/powershell/module/addsadministration/set-adserviceaccount?view=win10-ps) para usar apenas os tipos de criptografia com suporte nesta política. Por exemplo, se sua política permitir somente AES128 \_ HMAC \_ SHA1 e aes256 \_ HMAC \_ SHA1, você deverá executar `Set-ADServiceAccount -Identity HGSgMSA -KerberosEncryptionType AES128,AES256` .



## <a name="next-steps"></a>Próximas etapas

- Para as próximas etapas para configurar o atestado baseado em TPM, consulte [inicializar o cluster HgS usando o modo TPM em uma floresta de bastiões existente](guarded-fabric-initialize-hgs-tpm-mode-bastion.md).
- Para as próximas etapas para configurar o atestado de chave do host, consulte [inicializar o cluster HgS usando o modo de chave em uma floresta de bastiões existente](guarded-fabric-initialize-hgs-key-mode-bastion.md).
- Para as próximas etapas para configurar o atestado baseado em administrador (preterido no Windows Server 2019), consulte [inicializar o cluster HgS usando o modo ad em uma floresta de bastiões existente](guarded-fabric-initialize-hgs-ad-mode-bastion.md).