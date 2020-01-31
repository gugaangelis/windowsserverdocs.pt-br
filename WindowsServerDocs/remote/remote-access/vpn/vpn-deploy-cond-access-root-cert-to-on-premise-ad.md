---
title: Implantar certificados de raiz de acesso condicional para locais AD
description: ''
services: active-directory
ms.prod: windows-server
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 60be590d0d133f00817018018af42cfc23f1bee5
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822389"
---
# <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>Etapa 7.4. Implantar certificados raiz de acesso condicional no AD local

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

Nesta etapa, você implanta o certificado raiz de acesso condicional como certificado raiz confiável para autenticação de VPN para seu AD local.

- [**Anterior:** Etapa 7,3. Configurar a política de acesso condicional](vpn-config-conditional-access-policy.md)
- [**Em seguida:** Etapa 7,5. Criar perfis de VPNv2 baseados em OMA-DM para dispositivos Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. Na página **conectividade VPN** , selecione **baixar certificado**.

   >[!NOTE]
   >A opção **baixar certificado Base64** está disponível para algumas configurações que exigem certificados Base64 para implantação.

2. Faça logon em um computador ingressado em domínio com direitos de administrador corporativo e execute estes comandos em um prompt de comando de administrador para adicionar os certificados raiz da nuvem ao repositório *NTauth corporativo* :

   >[!NOTE]
   >For environments where the VPN server is not joined to the Active Directory domain, the cloud root certificates must be added to the _Trusted Root Certification Authorities_ store manually.

   | Comando | Descrição |
   | --- | --- |
   | `certutil -dspublish -f VpnCert.cer RootCA` | Creates two **Microsoft VPN root CA gen 1** containers under the **CN=AIA** and **CN=Certification Authorities** containers, and publishes each root certificate as a value on the _cACertificate_ attribute of both **Microsoft VPN root CA gen 1** containers. |
   | `certutil -dspublish -f VpnCert.cer NTAuthCA` | Creates one **CN=NTAuthCertificates** container under the **CN=AIA** and **CN=Certification Authorities** containers, and publishes each root certificate as a value on the _cACertificate_ attribute of the **CN=NTAuthCertificates** container. |
   | `gpupdate /force` | Expedites adding the root certificates to the Windows server and client computers. |

3. Verify that the root certificates are present in the Enterprise NTauth store and show as trusted:
   1. Log on to a server with Enterprise Admin rights that has the **Certificate Authority Management Tools** installed.

   >[!NOTE]
   >By default the **Certificate Authority Management Tools** are installed Certificate Authority servers. They can be installed on other members servers as part of the **Role Administration Tools** in Server Manager.

   1. On the VPN server, in the Start menu, enter **pkiview.msc** to open the Enterprise PKI dialog.
   1. From the Start menu, enter **pkiview.msc** to open the Enterprise PKI dialog.
   1. Right-click **Enterprise PKI** and select **Manage AD Containers**.
   1. Verify that each Microsoft VPN root CA gen 1 certificate is present under:
      - NTAuthCertificates
      - AIA Container
      - Certificate Authorities Container

## <a name="next-steps"></a>Próximas etapas

[Step 7.5. Create OMA-DM based VPNv2 Profiles to Windows 10 devices](vpn-create-oma-dm-based-vpnv2-profiles.md): In this step, you can create OMA-DM based VPNv2 profiles using Intune to deploy a VPN Device Configuration policy. If you want to use Microsoft Endpoint Configuration Manager or PowerShell Script to create VPNv2 profiles, see [VPNv2 CSP settings](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) for more details.
