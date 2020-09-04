---
title: Confirmar que os hosts protegidos podem atestar
ms.topic: article
ms.assetid: 7485796b-b840-4678-9b33-89e9710fbbc7
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 09/25/2019
ms.openlocfilehash: cda60c65772a41322a20b7277a7d7c80a4daf9e1
ms.sourcegitcommit: a640c2d7f2d21d7cd10a9be4496e1574e5e955f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89446769"
---
# <a name="confirm-guarded-hosts-can-attest"></a>Confirmar que os hosts protegidos podem atestar

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Um administrador de malha precisa confirmar que os hosts Hyper-V podem ser executados como hosts protegidos. Conclua as seguintes etapas em pelo menos um host protegido:

1. Se você ainda não instalou a função Hyper-V e o recurso de suporte do Hyper-V do guardião de host, instale-os com o seguinte comando:

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2. Verifique se o host Hyper-V pode resolver o nome DNS do HGS e tem conectividade de rede para alcançar a porta 80 (ou 443 se você configurar o HTTPS) no servidor HGS.

3. Configure as URLs de proteção e atestado de chave do host:

    - **Por meio do Windows PowerShell**: você pode configurar a proteção de chave e as URLs de atestado executando o comando a seguir em um console do Windows PowerShell elevado. Para &lt; FQDN &gt; , use o FQDN (nome de domínio totalmente qualificado) do seu cluster HgS (por exemplo, HgS. bastiões. local ou peça ao administrador do HgS para executar o cmdlet **Get-HGSSERVER** no servidor HgS para recuperar as URLs).

        ```PowerShell
        Set-HgsClientConfiguration -AttestationServerUrl 'http://<FQDN>/Attestation' -KeyProtectionServerUrl 'http://<FQDN>/KeyProtection'
         ```

        Para configurar um servidor HGS de fallback, repita esse comando e especifique as URLs de fallback para a proteção de chave e os serviços de atestado. Para obter mais informações, consulte [configuração de fallback](guarded-fabric-manage-branch-office.md#fallback-configuration).

    - **Por meio do VMM**: se você estiver usando System Center Virtual Machine Manager (VMM), poderá configurar o atestado e as URLs de proteção de chave no VMM. Para obter detalhes, consulte [definir configurações globais do HgS](/system-center/vmm/guarded-deploy-host?view=sc-vmm-2019#configure-global-hgs-settings) em **provisionar hosts protegidos no VMM**.

    >**Observações**
    > - Se o administrador HGS [habilitou o HTTPS no servidor HgS](guarded-fabric-configure-hgs-https.md), inicie as URLs com `https://` .
    > - Se o administrador HGS habilitou o HTTPS no servidor HGS e usou um certificado autoassinado, você precisará importar o certificado para o repositório de autoridades de certificação raiz confiáveis em cada host. Para fazer isso, execute o seguinte comando em cada host:
       ```PowerShell
       Import-Certificate -FilePath "C:\temp\HttpsCertificate.cer" -CertStoreLocation Cert:\LocalMachine\Root
       ```
    > - Se você configurou o cliente HGS para usar HTTPS e tiver desabilitado o TLS 1,0 em todo o seu caso, consulte nossas [diretrizes de TLS modernas](guarded-fabric-troubleshoot-hosts.md#modern-tls)

4. Para iniciar uma tentativa de atestado no host e exibir o status do atestado, execute o seguinte comando:

    ```powershell
    Get-HgsClientConfiguration
    ```

    A saída do comando indica se o host passou pelo atestado e agora está protegido. Se não `IsHostGuarded` retornar **true**, você poderá executar a ferramenta de diagnóstico HgS, [Get-HgsTrace](https://technet.microsoft.com/library/mt718831.aspx), para investigar. Para executar o diagnóstico, digite o seguinte comando em um prompt elevado do Windows PowerShell no host:

    ```powershell
    Get-HgsTrace -RunDiagnostics -Detailed
    ```

    > [!IMPORTANT]
    > Se você estiver usando o Windows Server 2019 ou o Windows 10, versão 1809 ou posterior, e estiver usando políticas de integridade de código, `Get-HgsTrace` retorne uma falha para o diagnóstico **ativo da política de integridade de código** .
    > Você pode ignorar esse resultado com segurança quando ele for o único diagnóstico com falha.

## <a name="next-step"></a>Próxima etapa

> [!div class="nextstepaction"]
> [Implantar VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

## <a name="additional-references"></a>Referências adicionais

- [Implantar o Serviço Guardião de Host (HGS)](guarded-fabric-deploying-hgs-overview.md)
- [Implantar VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
