---
title: Confirme se os hosts protegidos podem atestar
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7485796b-b840-4678-9b33-89e9710fbbc7
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 02/05/2019
ms.openlocfilehash: 87878eba785c0e1cc50454a74b2af4a159e88e12
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443666"
---
# <a name="confirm-guarded-hosts-can-attest"></a>Confirme se os hosts protegidos podem atestar 

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016


Um administrador de malha precisa confirmar que os hosts do Hyper-V podem ser executado como hosts protegidos. Conclua as etapas a seguir em pelo menos um host protegido:

1.  Se você ainda não instalou a função Hyper-V e o recurso de suporte de Hyper-V de guardião de Host, você deve instalá-los com o seguinte comando:

        Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart

2.  Verifique se o host Hyper-V pode resolver o nome DNS de HGS e tem conectividade de rede para alcançar a porta 80 (ou 443 se você configurar o HTTPS) no servidor do HGS.

2.  Configure a proteção de chave e URLs do Atestado do host:

    - **Por meio do Windows PowerShell**: Você pode configurar as URLs de Atestado e proteção de chave executando o seguinte comando em um console do Windows PowerShell com privilégios elevados. Para &lt;FQDN&gt;, use o domínio nome FQDN (totalmente qualificado) do seu cluster do HGS (por exemplo, hgs.bastion.local, ou peça ao administrador HGS para executar o **Get-HgsServer** cmdlet no servidor HGS para recuperar o URLs).

        `Set-HgsClientConfiguration -AttestationServerUrl 'http://<FQDN>/Attestation' -KeyProtectionServerUrl 'http://<FQDN>/KeyProtection'`

        Para configurar um servidor HGS de fallback, repita esse comando e especifique as URLs de fallback para os serviços de Atestado e proteção de chave. Para obter mais informações, consulte [configuração de Fallback](guarded-fabric-manage-branch-office.md#fallback-configuration). 

    - **Por meio do VMM**: Se você estiver usando o System Center 2016 – Virtual Machine Manager (VMM), você pode configurar as URLs do Atestado e chave de proteção no VMM. Para obter detalhes, consulte [definir as configurações globais do HGS](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts#configure-global-hgs-settings) na **provisionar hosts protegidos na VMM**.
    
    >**Notas**
    > - Se o administrador HGS [habilitar o HTTPS no servidor HGS](guarded-fabric-configure-hgs-https.md), iniciar as URLs com `https://`.
    > - Se o administrador HGS habilitar o HTTPS no servidor HGS e usado um certificado autoassinado, você precisará importar o certificado para o armazenamento de autoridades de certificação raiz confiáveis em todos os hosts. Para fazer isso, execute o seguinte comando em cada host:<br>
        `Import-Certificate -FilePath "C:\temp\HttpsCertificate.cer" -CertStoreLocation Cert:\LocalMachine\Root`
    
3.  Para iniciar uma tentativa de Atestado no host e exibir o status do Atestado, execute o seguinte comando:

        Get-HgsClientConfiguration

    A saída do comando indica se o host passado Atestado e agora é protegido. Se `IsHostGuarded` não retorna **verdadeira**, você pode executar a ferramenta de diagnóstico HGS [Get-HgsTrace](https://technet.microsoft.com/library/mt718831.aspx), para investigar. Para executar o diagnóstico, digite o seguinte comando em um prompt elevado do Windows PowerShell no host:

        Get-HgsTrace -RunDiagnostics -Detailed

    > [!IMPORTANT]
    > Se você estiver usando o Windows Server 2019 ou Windows 10, versão 1809 e estiver usando políticas de integridade de código `Get-HgsTrace` retornar uma falha para o **ativa de política de integridade código** diagnóstico.
    > Você pode ignorar com segurança esse resultado, quando se trata de apenas falhando diagnóstico.

## <a name="next-step"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Implantar VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

## <a name="see-also"></a>Consulte também

- [Implantar o serviço guardião de Host (HGS)](guarded-fabric-deploying-hgs-overview.md)
- [Implantar VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

