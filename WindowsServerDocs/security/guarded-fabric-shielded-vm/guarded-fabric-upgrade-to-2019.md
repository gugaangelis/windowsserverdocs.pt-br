---
title: Fazer upgrade de uma malha protegida para o Windows Server 2019
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/21/2018
ms.openlocfilehash: 274bdf027947ffb6fe807d4acd0a3b2174c20e28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867447"
---
# <a name="upgrade-a-guarded-fabric-to-windows-server-2019"></a>Fazer upgrade de uma malha protegida para o Windows Server 2019

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Este artigo descreve as etapas necessárias para atualizar uma malha protegida existente do Windows Server 2016, versão 1709 do Windows Server ou Windows Server versão 1803 para Windows Server 2019.

## <a name="whats-new-in-windows-server-2019"></a>Novidades no Windows Server 2019

Quando você executar uma malha protegida no Windows Server 2019, você pode tirar proveito dos diversos recursos novos:

**Atestado de chaves de host** é nossa mais recente modo de Atestado, projetado para tornar mais fácil de executar VMs blindadas quando seus hosts Hyper-V não tiver TPM 2.0 dispositivos disponíveis para o atestado de TPM. Atestado de chaves de host usa pares de chaves para autenticar hosts com o HGS, removendo a necessidade de hosts a ser associado a um domínio do Active Directory, eliminando a relação de confiança do AD entre HGS e a floresta corporativa e reduzindo o número de portas de firewall aberta. Atestado de chaves de host substitui Atestado do Active Directory, que foi preterido no Windows Server 2019.

**Versão de Atestado v2** - para dar suporte a atestado de chaves de Host e novos recursos no futuro, apresentamos o controle de versão para HGS. Uma instalação nova do HGS no Windows Server 2019 resultará no servidor usando o Atestado v2, o que significa que ele pode dar suporte a atestado de chaves de Host para hosts Windows Server 2019 e ainda oferecem suporte a hosts de v1 no Windows Server 2016. Atualizações in-loco para 2019 permanecerá na versão v1 até que você habilite manualmente o v2. A maioria dos cmdlets agora têm um parâmetro de - HgsVersion que permite que você especifique se deseja trabalhar com as políticas de Atestado modernas ou herdados.

**Suporte para Linux de VMs blindadas** – hosts do Hyper-V executando o Windows Server 2019 pode executar o Linux VMs blindadas. Embora do Linux blindada VMs existem desde a versão 1709 do Windows Server, Windows Server 2019 é a primeira versão de canal de manutenção longo prazo para dar suporte a eles.

**Aprimoramentos do office de branch** -tornamos mais fácil de executar VMs blindadas em filiais com suporte para VMs blindadas off-line e configurações de fallback em hosts Hyper-V.

**Ligação de host do TPM** -para a mais segura cargas de trabalho, onde você deseja que uma VM blindada para ser executado somente no primeiro host em que ele foi criado, mas nenhum outro, agora você pode associar a VM a esse host usando o TPM do host. Isso é mais adequado para estações de trabalho de acesso privilegiado e filiais, em vez de cargas de trabalho do datacenter geral que precisam migrar entre hosts.

## <a name="compatibility-matrix"></a>Matriz de compatibilidade

Antes de atualizar sua malha protegida para Windows Server 2019, examine a seguinte matriz de compatibilidade para ver se há suporte para a sua configuração.

|  | WS2016 HGS | WS2019 HGS|
|---|---|---|
|**WS2016 Host do Hyper-V** | Com suporte | Suporte para<sup>1</sup>|
|**WS2019 Hyper-V Host** | Não há suporte para<sup>2</sup> | Com suporte|

<sup>1</sup> hosts Windows Server 2016 podem atestar apenas em servidores Windows Server 2019 HGS usando o protocolo de Atestado v1. Não há suporte para os novos recursos que estão disponíveis exclusivamente no protocolo de Atestado v2, incluindo atestado de chaves de Host para hosts Windows Server 2016.

<sup>2</sup> a Microsoft está ciente de um problema, impedindo que os hosts Windows Server 2019 usando o atestado de TPM de Atestado com êxito em um servidor Windows Server 2016 HGS. Essa limitação será corrigida em uma atualização futura do Windows Server 2016.

## <a name="upgrade-hgs-to-windows-server-2019"></a>Atualizar o HGS para o Windows Server de 2019

É recomendável atualizar seu cluster HGS para Windows Server 2019 antes de atualizar seus hosts do Hyper-V para garantir que todos os hosts, se eles estiverem executando o Windows Server 2016 ou 2019, podem continuar a atestar com êxito.

Atualizar o cluster HGS exigirá a remover temporariamente um nó do cluster por vez enquanto ele está atualizado. Isso reduzirá a capacidade do seu cluster responda às solicitações de seus hosts do Hyper-V e pode resultar em tempos de resposta lentos ou interrupções de serviço para seus locatários. Certifique-se de que você tem capacidade suficiente para lidar com o Atestado e solicitações de chave de versão antes de atualizar um servidor HGS.

Para atualizar seu cluster HGS, execute as seguintes etapas em cada nó do cluster, um nó no momento:

1.  Remover o servidor HGS de seu cluster executando `Clear-HgsServer` em um prompt elevado do PowerShell. Este cmdlet removerá o repositório HGS replicado, sites HGS e nó do cluster de failover.
2.  Se seu servidor HGS for um controlador de domínio (configuração padrão), você precisará executar `adprep /forestprep` e `adprep /domainprep` no primeiro nó que está sendo atualizado para preparar o domínio para uma atualização do sistema operacional. Consulte a [documentação de atualização do Active Directory Domain Services](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/upgrade-domain-controllers#supported-in-place-upgrade-paths) para obter mais informações.
3.  Executar uma [atualização in loco](../../get-started-19/install-upgrade-migrate-19.md) para Windows Server 2019.
4.  Execute [HgsServer Initialize](guarded-fabric-configure-additional-hgs-nodes.md) para ingressar o nó do cluster.

Depois que todos os nós foram atualizados para o Windows Server 2019, opcionalmente, você pode atualizar a versão HGS para v2 para dar suporte a novos recursos, como o atestado de chaves de Host.

```powershell
Set-HgsServerVersion  v2
```

## <a name="upgrade-hyper-v-hosts-to-windows-server-2019"></a>Atualizar hosts do Hyper-V para Windows Server 2019

Antes de atualizar seus hosts do Hyper-V para Windows Server 2019, certifique-se de que seu cluster HGS já foi atualizado para Windows Server 2019 e que você moveu todas as VMs do servidor do Hyper-V.

1.  Se você estiver usando políticas de integridade de código do Windows Defender Application Control no seu servidor (sempre o caso ao usar o atestado de TPM), certifique-se de que a política está no modo de auditoria ou desabilitado antes de tentar atualizar o servidor. [Saiba como desabilitar uma política do WDAC](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/disable-windows-defender-application-control-policies)
2.  Siga as orientações na [Centro de atualização do Windows Server](http://aka.ms/upgradecenter) para atualizar o host para o Windows Server 2019. Se seu host do Hyper-V é parte de um Cluster de Failover, considere o uso de um [atualização sem interrupção do Cluster de sistema operacional](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md).
3.  [Testar e habilitar novamente](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/audit-windows-defender-application-control-policies) sua política de controle de aplicativo do Windows Defender, se você tivesse um habilitado antes da atualização.
4.  Execute `Get-HgsClientConfiguration` para verificar se **IsHostGuarded = True**, que significa que o host está passando com êxito Atestado com seu servidor HGS.
5.  Se você estiver usando o atestado de TPM, você talvez precise [capturar novamente a política de integridade de código ou de linha de base TPM](guarded-fabric-add-host-information-for-tpm-trusted-attestation.md) após a atualização para passar o Atestado.
6.  Iniciar a execução de VMs blindadas no host novamente!

## <a name="switch-to-host-key-attestation"></a>Alternar para hospedar o atestado de chaves

Siga as etapas abaixo se você atualmente em execução baseado no Active Directory e quiser atualizar para o atestado de chaves de Host. Observe que o Atestado baseado no Active Directory foi preterido no Windows Server 2019 e pode ser removido em uma versão futura.

1.  Certifique-se de que seu servidor HGS está operando no modo de Atestado v2, executando o comando a seguir. Hosts de v1 existentes continuarão atestar, mesmo quando o servidor HGS é atualizado para a v2.

    ```powershell
    Set-HgsServerVersion v2
    ```

2.  [Gerar chaves de host](guarded-fabric-create-host-key.md) de cada um dos seu Hyper-V hospeda e registrá-los com o HGS. Porque o HGS ainda está operando no modo do Active Directory, você receberá um aviso de que as novas chaves de host não estão em vigor imediatamente. Isso é intencional, pois você não deseja alterar para modo de chave do host, até que todos os seus hosts podem atestar com chaves de host com êxito.

3.  Depois que as chaves de host tenham sido registradas para cada host, você pode configurar o HGS para usar o modo de atestado de chaves de host:

    ```powershell
    Set-HgsServer -TrustHostKey
    ```

    Se você tiver problemas com o modo de chave de host e precisar reverter para Atestado baseada no Active Directory, execute o seguinte comando no HGS:

    ```powershell
    Set-HgsServer -TrustActiveDirectory
    ```