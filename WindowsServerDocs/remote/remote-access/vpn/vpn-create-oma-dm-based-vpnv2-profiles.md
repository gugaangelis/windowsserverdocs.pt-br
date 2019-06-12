---
title: Criar perfis de VPNv2 baseados em OMA-DM para dispositivos Windows 10
description: 'Você pode usar um dos dois métodos para criar o OMA-DM com base em perfis de VPNv2. '
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 9051a5b4dc8055885bdc1f8f727514b6e049d74d
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749512"
---
# <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devices"></a>Etapa 7.5. Criar OMA-DM com base em perfis de VPNv2 para dispositivos Windows 10

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Etapa 7.4. Implantar certificados de raiz do acesso condicional para o local AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)
- [**Avançar:** Saiba como o acesso condicional para VPN funciona](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Nesta etapa, você pode criar o OMA-DM com base em perfis de VPNv2 usando o Intune para implantar uma política de configuração do dispositivo VPN. Se você quiser usar o SCCM ou o PowerShell script para criar perfis de VPNv2, consulte [configurações de CSP de VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) para obter mais detalhes. 

## <a name="managed-deployment-using-intune"></a>Implantação gerenciada usando o Intune

Tudo que é discutido nesta seção é o mínimo necessário para fazer com que o VPN funcionarem com acesso condicional. Ele não aborda um túnel dividido, usando o WIP, criação de perfis de configuração de dispositivo Intune personalizados para obter AutoVPN trabalhando ou SSO. Integre as configurações abaixo o perfil VPN que você criou anteriormente em [etapa 5. Configurar o cliente do Windows 10 sempre em conexões VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md).  Neste exemplo, podemos estiver integrando-los para o [configurar o cliente VPN usando o Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune) política. 

**Pré-requisito:**

Computador de cliente do Windows 10 já foi configurado com uma conexão VPN usando o Intune.   


**Procedimento:**

1. No portal do Azure, selecione **Intune** > **configuração do dispositivo** > **perfis** e selecione o perfil VPN que você criou anteriormente no [ Configurar o cliente VPN usando o Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune).
    
2. No editor de política, selecione **propriedades** > **configurações** > **VPN de Base**. Estender os existentes **Xml EAP** para incluir um filtro que permite que o cliente VPN a lógica que ele precisa recuperar o certificado de acesso condicional do AAD do repositório de certificados do usuário em vez de deixá-lo a chance de que lhe permite usar o primeiro certificado foi descoberto.

    >[!NOTE]
    >Sem isso, o cliente VPN foi possível recuperar o certificado de usuário emitido da autoridade de certificação local, resultando em uma conexão VPN com falha.

    ![Portal do Intune](../../media/Always-On-Vpn/intune-eap-xml.png)

3. Localize a seção que termina com  **\</AcceptServerName >\</EapType >** e insira a seguinte cadeia de caracteres entre esses dois valores para fornecer ao cliente VPN com a lógica para selecionar a condicional do AAD Certificado de acesso:

    ```XML
    <TLSExtensions xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><EKUName>AAD Conditional Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></ClientAuthEKUList></FilteringInfo></TLSExtensions>
    ```

4. Selecione o **acesso condicional** folha e alternância **acesso condicional para esta conexão VPN** para **habilitado**.
   
   Habilitar essa opção alterações de configuração do  **\<DeviceCompliance >\<habilitado > true\</habilitado >** definindo no XML do perfil de VPNv2.

    ![Acesso condicional para VPN - propriedades Always On](../../media/Always-On-Vpn/vpn-conditional-access-azure-ad.png)

5. Selecione **OK**.

6. Selecione **atribuições**, em incluir, selecione **selecionar grupos para incluir**.

7. Selecione o **usuários de VPN** grupo que recebe essa política e selecione **salvar**.

    ![LIMITE para os usuários VPN automático - atribuições](../../media/Always-On-Vpn/cap-for-auto-vpn-users-assignments.png)

## <a name="force-mdm-policy-sync-on-the-client"></a>Forçar a sincronização de política MDM no cliente

Se o perfil VPN não aparecer no dispositivo cliente, em configurações\\rede e Internet\\VPN, você pode forçar a política de MDM para sincronizar.

1. Entre em um computador cliente ingressado no domínio como um membro do **usuários da VPN** grupo.

2. No menu Iniciar, digite **conta**, e pressione Enter.

3. No painel de navegação à esquerda, selecione **acessar trabalho ou escola**.

4. Em acesso corporativo ou estudante, selecione **conectado a < \domain > MDM**, em seguida, selecione **informações**.

5. Selecione **Sync** e verifique se o perfil VPN é exibido nas configurações\\rede e Internet\\VPN.


## <a name="next-steps"></a>Próximas etapas

Concluir configuração do perfil VPN para usar o acesso condicional do Azure AD. 

|Se você quiser...  |Em seguida, consulte...  |
|---------|---------|
|Saiba mais sobre como o acesso condicional funciona com VPNs  |[VPN e acesso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Esta página fornece mais informações sobre como o acesso condicional funciona com VPNs.      |
|Saiba mais sobre os recursos avançados de VPN  |[Recursos VPN avançados](always-on-vpn/deploy/always-on-vpn-adv-options.md#advanced-vpn-features): Esta página fornece diretrizes sobre como habilitar filtros de tráfego de VPN, como configurar conexões VPN automáticas usando gatilhos de aplicativo e como configurar o NPS para permitir somente conexões VPN de clientes que usam certificados emitidos pelo AD do Azure.        |


## <a name="related-topics"></a>Tópicos relacionados

- [VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp):  Este tópico fornece uma visão geral de VPNv2 CSP. O provedor de serviço de configuração de VPNv2 permite que o servidor de gerenciamento (MDM) do dispositivo móvel configurar o perfil VPN do dispositivo.

- [Configurar o cliente do Windows 10 sempre em conexões VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): Este tópico fornece informações sobre as opções de ProfileXML e o esquema e como criar a VPN ProfileXML. Depois de configurar a infraestrutura de servidor, você deve configurar os computadores de cliente do Windows 10 para se comunicar com que a infraestrutura com uma conexão VPN. 

- [Configurar o cliente VPN usando o Intune](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections#configure-the-vpn-client-by-using-intune): Este tópico fornece informações sobre como implantar perfis do Windows 10 remoto acesso VPN Always On. Agora, o Intune usa grupos do Azure AD. Se o Azure AD Connect sincronizado o grupo de usuários de VPN do local para o Azure AD, não é necessário para configurar o cliente VPN usando o Intune.
