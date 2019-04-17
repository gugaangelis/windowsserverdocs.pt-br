---
title: Criar perfis de VPNv2 baseados em OMA-DM para dispositivos Windows 10
description: 'Você pode usar um dos dois métodos para criar OMA DM com base em perfis de VPNv2. '
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
ms.openlocfilehash: 1ce20d09c304b26e3708429cc45da06d020e5809
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031290"
---
# Etapa 7.5. Criar perfis de VPNv2 para dispositivos Windows 10 com base OMA DM

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** etapa 7.4. Implantar certificados de raiz de acesso condicional para locais AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)<br>
& #187; [ **Próximo:** Saiba como condicional acesso para VPN funciona](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Nesta etapa, você pode criar OMA DM com base em perfis de VPNv2 usando o Intune para implantar uma política de configuração de dispositivo da VPN. Se você quiser usar script SCCM ou o PowerShell para criar perfis de VPNv2, consulte [as configurações do CSP VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) para obter mais detalhes. 

## Implantação gerenciadas usando o Intune

Tudo discutidas nesta seção é o mínimo necessário para tornar VPN trabalhar com acesso condicional. Ele não abrange a divisão de encapsulamento, usando WIP, criação de perfis de configuração personalizados do Intune dispositivo para obter AutoVPN trabalhando ou SSO. Integre o perfil VPN que você criou anteriormente na etapa 5 [as configurações abaixo. Configurar o cliente do Windows 10 sempre em conexões VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md).Neste exemplo, nós são integrá-los com a política de [Configurar o cliente VPN usando o Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune) . 

**Pré-requisito:**<p>
Computador cliente do Windows 10 já tiver sido configurado com uma conexão de VPN usando o Intune.   


**Procedimento:**

1. No portal do Azure, clique em **Intune** > **Configuração do dispositivo** > **perfis** e selecione o perfil VPN que você criou anteriormente na [Configurar o cliente VPN usando o Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune).
    
2. No editor de política, selecione **Propriedades** > **configurações** > **Base VPN**. Estender o **Xml do EAP** existente para incluir um filtro que oferece o cliente VPN a lógica que ele precisa recuperar o certificado de acesso condicional AAD do repositório de certificados do usuário em vez de deixá-la para chance permitindo que ele use o primeiro certificado descobertos.

    >[!NOTE]
    >Sem isso, o cliente VPN pode recuperar o certificado de usuário emitido da autoridade de certificação no local, resultando em uma conexão VPN com falha.

    ![Portal do Intune](../../media/Always-On-Vpn/intune-eap-xml.png)

3. Localize a seção termina com **\</AcceptServerName>\</EapType>** e insira a seguinte cadeia de caracteres entre esses dois valores para fornecer o cliente VPN com a lógica para selecionar o certificado de acesso condicional AAD:

    ```XML
    <TLSExtensions xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><EKUName>AAD Conditional Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></ClientAuthEKUList></FilteringInfo></TLSExtensions>
    ```

4. Selecione a folha de **Acesso condicional** e alternar **acesso condicional para essa conexão VPN** como **ativado**.<p>Habilitar essa alterações de configuração **\<DeviceCompliance>\<Enabled>true\</Enabled>** configuração no XML de perfil VPNv2.

    ![Acesso condicional para sempre em VPN - propriedades](../../media/Always-On-Vpn/vpn-conditional-access-azure-ad.png)

6. Clique em **OK**.

6. Selecione **atribuições**, em incluir, clique em **Selecionar grupos a serem incluídos**.

7. Selecione o grupo de **Usuários de VPN** que recebe essa política e clique em **Salvar**.

    ![LIMITE para usuários VPN automática - atribuições](../../media/Always-On-Vpn/cap-for-auto-vpn-users-assignments.png)

## Forçar sincronização de política do MDM no cliente
Se o perfil VPN não aparecer no dispositivo do cliente, em Settings\\Network & Internet\\VPN, você pode forçar a política de MDM para sincronizar.

1. Entrar um computador cliente ingressado no domínio como um membro do grupo de **Usuários da VPN** .

2. No menu Iniciar, digite a **conta**e pressione Enter.

3.  No painel de navegação esquerdo, clique em **acessar trabalho ou escola**.

5.  Em acessar trabalho ou escola, clique **conectadas ao <\domain> MDM** e **informações**.

6.  Clique em **sincronização** e verifique se que o perfil VPN aparece em Settings\\Network & Internet\\VPN.


## Próximas etapas
Terminar de configurar o perfil VPN para usar o acesso condicional do Azure AD. 

|Se você quiser...  |Em seguida, consulte...  |
|---------|---------|
|Saiba mais sobre como funciona o acesso condicional como com VPNs  |[VPN e acesso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): esta página fornece mais informações sobre acesso condicional como funciona com VPNs.      |
|Saiba mais sobre os recursos avançados de VPN  |[Recursos avançados de VPN](always-on-vpn/deploy/always-on-vpn-adv-options.md#advanced-vpn-features): esta página fornece orientações sobre como habilitar filtros de tráfego de VPN, como configurar conexões VPN automática usando o aplicativo gatilhos e como configurar o NPS para permitir somente conexões VPN de clientes usando certificados emitidos pelo Azure AD.        |


---

## Tópicos relacionados
- [CSP VPNv2](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp): Este tópico fornece uma visão geral do CSP VPNv2. O provedor de serviços de configuração VPNv2 permite que o servidor de gerenciamento (MDM) de dispositivo móvel configurar o perfil VPN do dispositivo.

- [Configurar o Windows 10 sempre em conexões VPN de cliente](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): Este tópico fornece informações sobre as opções de ProfileXML e esquema e como criar o ProfileXML VPN. Depois de configurar a infraestrutura de servidor, você deve configurar os computadores de cliente do Windows 10 para se comunicar com essa infraestrutura com uma conexão VPN. 

- [Configurar o cliente VPN usando o Intune](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections#configure-the-vpn-client-by-using-intune): Este tópico fornece informações sobre como implantar o Windows 10 remoto acesso sempre em perfis de VPN. Agora, o Intune usa grupos do Azure AD. Se o Azure AD Connect sincronizados o grupo de usuários de VPN de local ao Azure AD, não é necessário para configurar o cliente VPN usando o Intune.

---
