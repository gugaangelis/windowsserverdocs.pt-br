---
title: Fazer upgrade de uma malha protegida para o Windows Server 2019
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 11/21/2018
ms.openlocfilehash: aefff380a1320898ff342f813498b8f45dfa6122
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943996"
---
# <a name="upgrade-a-guarded-fabric-to-windows-server-2019"></a>Fazer upgrade de uma malha protegida para o Windows Server 2019

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Este artigo descreve as etapas necessárias para atualizar uma malha protegida existente do Windows Server 2016, do Windows Server versão 1709 ou do Windows Server versão 1803 para o Windows Server 2019.

## <a name="whats-new-in-windows-server-2019"></a>Novidades no Windows Server 2019

Ao executar uma malha protegida no Windows Server 2019, você pode tirar proveito de vários novos recursos:

O **atestado de chave de host** é nosso modo de atestado mais recente, projetado para facilitar a execução de VMs blindadas quando seus hosts Hyper-V não têm dispositivos TPM 2,0 disponíveis para atestado de TPM. O atestado de chave de host usa pares de chaves para autenticar hosts com HGS, removendo o requisito para que os hosts sejam ingressados em um domínio de Active Directory, eliminando a confiança do AD entre o HGS e a floresta corporativa e reduzindo o número de portas de firewall abertas. O atestado de chave de host substitui Active Directory atestado, que é preterido no Windows Server 2019.

**V2 atestado versão** -para dar suporte ao atestado de chave de host e novos recursos no futuro, apresentamos o controle de versão para HgS. Uma nova instalação do HGS no Windows Server 2019 resultará no servidor usando o atestado v2, o que significa que ele pode dar suporte ao atestado de chave de host para hosts do Windows Server 2019 e ainda oferecer suporte a hosts v1 no Windows Server 2016. As atualizações in-loco para 2019 permanecerão na versão v1 até que você habilite a v2 manualmente. A maioria dos cmdlets agora tem um parâmetro-HgsVersion que permite especificar se você deseja trabalhar com políticas de atestado herdadas ou modernas.

**Suporte para VMs blindadas Linux** -hosts Hyper-V que executam o Windows Server 2019 podem executar VMs blindadas Linux. Embora as VMs blindadas do Linux tenham sido feitas desde o Windows Server versão 1709, o Windows Server 2019 é a primeira versão de canal de manutenção de longo prazo para dar suporte a elas.

**Aprimoramentos na filial** – facilitamos a execução de VMs blindadas em filiais com suporte para VMs blindadas offline e configurações de fallback em hosts Hyper-V.

**Associação de host TPM** -para obter as cargas de trabalho mais seguras, em que você deseja que uma VM blindada seja executada somente no primeiro host em que foi criada, mas nenhuma outra, agora você pode associar a VM a esse host usando o TPM do host. Isso é melhor usado para estações de trabalho de acesso privilegiado e filiais, em vez de cargas de trabalho de datacenter gerais que precisam migrar entre hosts.

## <a name="compatibility-matrix"></a>Matriz de compatibilidade

Antes de atualizar sua malha protegida para o Windows Server 2019, examine a matriz de compatibilidade a seguir para ver se há suporte para sua configuração.

|  | HGS WS2016 | HGS WS2019|
|---|---|---|
|**Host do Hyper-V WS2016** | Com suporte | Com suporte<sup>1</sup>|
|**Host do Hyper-V WS2019** | Sem suporte<sup>2</sup> | Com suporte|

<sup>1</sup> os hosts do windows Server 2016 só podem atestar com servidores HgS do windows Server 2019 usando o protocolo de atestado v1. Os novos recursos que estão disponíveis exclusivamente no protocolo de atestado v2, incluindo o atestado de chave de host, não têm suporte para hosts do Windows Server 2016.

<sup>2</sup> a Microsoft está ciente de um problema que impede que hosts do windows Server 2019 usando atestado de TPM atestados com êxito em um servidor HgS do windows Server 2016. Essa limitação será abordada em uma atualização futura do Windows Server 2016.

## <a name="upgrade-hgs-to-windows-server-2019"></a>Atualizar o HGS para o Windows Server 2019

É recomendável atualizar seu cluster HGS para o Windows Server 2019 antes de atualizar seus hosts Hyper-V para garantir que todos os hosts, se estiverem executando o Windows Server 2016 ou 2019, possam continuar a atestar com êxito.

Atualizar o cluster HGS exigirá que você remova temporariamente um nó do cluster de cada vez enquanto ele é atualizado. Isso reduzirá a capacidade do cluster de responder a solicitações de seus hosts Hyper-V e poderá resultar em tempos de resposta lentos ou interrupções de serviço para seus locatários. Verifique se você tem capacidade suficiente para lidar com suas solicitações de atestado e de liberação de chave antes de atualizar um servidor HGS.

Para atualizar o cluster HGS, execute as seguintes etapas em cada nó do cluster, um nó por vez:

1.  Remova o servidor HGS do cluster executando `Clear-HgsServer` em um prompt do PowerShell com privilégios elevados. Esse cmdlet removerá o armazenamento replicado HGS, os sites HGS e o nó do cluster de failover.
2.  Se o seu servidor HGS for um controlador de domínio (configuração padrão), você precisará executar `adprep /forestprep` e `adprep /domainprep` no primeiro nó que está sendo atualizado para preparar o domínio para uma atualização do sistema operacional. Consulte a [documentação de atualização do Active Directory Domain Services](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/upgrade-domain-controllers#supported-in-place-upgrade-paths) para obter mais informações.
3.  Execute uma [atualização in-loco](../../get-started-19/install-upgrade-migrate-19.md) para o Windows Server 2019.
4.  Execute [Initialize-HgsServer](guarded-fabric-configure-additional-hgs-nodes.md) para unir o nó de volta ao cluster.

Depois que todos os nós tiverem sido atualizados para o Windows Server 2019, você poderá, opcionalmente, atualizar a versão do HGS para v2 para dar suporte a novos recursos, como o atestado de chave de host.

```powershell
Set-HgsServerVersion  v2
```

## <a name="upgrade-hyper-v-hosts-to-windows-server-2019"></a>Atualizar hosts do Hyper-V para o Windows Server 2019

Antes de atualizar seus hosts Hyper-V para o Windows Server 2019, verifique se o cluster HGS já está atualizado para o Windows Server 2019 e se você moveu todas as VMs para fora do servidor Hyper-V.

1.  Se você estiver usando as políticas de integridade de código do controle de aplicativos do Windows Defender no servidor (sempre que estiver usando atestado de TPM), verifique se a política está no modo de auditoria ou está desabilitada antes de tentar atualizar o servidor. [Saiba como desabilitar uma política WDAC](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/disable-windows-defender-application-control-policies)
2.  Siga as orientações no [conteúdo de atualização do Windows Server](../../upgrade/upgrade-overview.md) para atualizar seu host para o Windows Server 2019. Se o host do Hyper-V fizer parte de um cluster de failover, considere o uso de uma [atualização sem interrupção do sistema operacional do cluster](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md).
3.  [Teste e reabilite](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/audit-windows-defender-application-control-policies) sua política de controle de aplicativos do Windows Defender, se você tiver uma habilitada antes da atualização.
4.  Execute `Get-HgsClientConfiguration` para verificar se **IsHostGuarded = true**, significando que o host está passando com êxito o atestado com o servidor HgS.
5.  Se você estiver usando o atestado de TPM, talvez seja necessário [recapturar a política de linha de base ou integridade de código do TPM](guarded-fabric-add-host-information-for-tpm-trusted-attestation.md) após a atualização para passar o atestado.
6.  Inicie a execução de VMs blindadas no host novamente!

## <a name="switch-to-host-key-attestation"></a>Alternar para atestado de chave de host

Siga as etapas abaixo se você estiver executando o atestado baseado em Active Directory e quiser atualizar para o atestado de chave de host. Observe que o atestado baseado em Active Directory foi preterido no Windows Server 2019 e pode ser removido em uma versão futura.

1.  Verifique se o servidor HGS está operando no modo de atestado v2 executando o comando a seguir. Os hosts v1 existentes continuarão a atestar mesmo quando o servidor HGS for atualizado para v2.

    ```powershell
    Set-HgsServerVersion v2
    ```

2.  [Gere chaves de host](guarded-fabric-create-host-key.md) de cada um dos seus hosts Hyper-V e registre-os com o HgS. Como o HGS ainda está operando no modo de Active Directory, você receberá um aviso informando que as novas chaves de host não são efetivas imediatamente. Isso é intencional, pois você não deseja alterar para o modo de chave do host até que todos os seus hosts possam atestar com êxito as chaves de host.

3.  Depois que as chaves de host tiverem sido registradas para cada host, você poderá configurar o HGS para usar o modo de atestado de chave de host:

    ```powershell
    Set-HgsServer -TrustHostKey
    ```

    Se você tiver problemas com o modo de chave de host e precisar reverter para o atestado baseado em Active Directory, execute o seguinte comando no HGS:

    ```powershell
    Set-HgsServer -TrustActiveDirectory
    ```