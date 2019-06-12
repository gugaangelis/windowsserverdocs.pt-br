---
title: Implantar certificados de raiz de acesso condicional para locais AD
description: ''
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 4aaad98cd04c9b07bdea848294e10d9bcb602064
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749545"
---
# <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>Etapa 7.4. Implantar certificados de raiz do acesso condicional para o local AD

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

Nesta etapa, você implanta o certificado de raiz do acesso condicional como certificado de raiz confiável para autenticação de VPN em suas instalações AD.

- [**Anterior:** Etapa 7.3. Configurar a política de acesso condicional](vpn-config-conditional-access-policy.md)
- [**Avançar:** Etapa 7.5. Criar perfis VPNv2 baseados em OMA-DM para dispositivos Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. Sobre o **conectividade VPN** página, selecione **baixar certificado**. 
   
    ![Baixe o certificado para acesso condicional](../../media/Always-On-Vpn/06.png)

    >[!NOTE]
    >O **baixar o certificado de base64** opção está disponível para algumas configurações que requerem certificados de base64 para implantação. 

2. Faça logon em um computador ingressado no domínio com direitos de administrador corporativo e execute estes comandos em um prompt de comando de administrador para adicionar a nuvem raiz do certificado (s) para o *NTauth corporativo* armazenar:

    >[!NOTE]
    >Para ambientes em que o servidor VPN não ingressou no domínio do Active Directory, os certificados de raiz de nuvem devem ser adicionados para o _autoridades de certificação raiz confiáveis_ armazenar manualmente.

    |Comando  |Descrição  |  
    |---------|-------------| 
    |`certutil -dspublish -f VpnCert.cer RootCA`     |Cria dois **gen 1 do CA de raiz de VPN do Microsoft** contêineres sob o **CN = AIA** e **CN = autoridades de certificação** contêineres e os publica cada certificado de raiz como um valor em o _cACertificate_ atributo de ambos **gen 1 do CA de raiz de VPN do Microsoft** contêineres.|  
    |`certutil -dspublish -f VpnCert.cer NTAuthCA`   |Cria um **CN = NTAuthCertificates** contêiner sob o **CN = AIA** e **CN = autoridades de certificação** contêineres e os publica cada certificado de raiz como um valor em o _cACertificate_ atributo da **CN = NTAuthCertificates** contêiner. |  
    |`gpupdate /force`     |Acelera a adicionar os certificados raiz para os computadores cliente e servidor Windows.  |

3.  Verifique se os certificados raiz estão presentes no repositório NTauth corporativo e mostrar como confiável:

    a.  Faça logon em um servidor com direitos de administrador corporativo que tem o **as ferramentas de gerenciamento de autoridade de certificado** instalado.

    >[!NOTE]
    >Por padrão o **as ferramentas de gerenciamento de autoridade de certificado** são servidores de autoridade de certificação instalados. Eles podem ser instalados em outros servidores membros como parte dos **ferramentas de administração de função** no Gerenciador do servidor.

    b.  No servidor VPN, no menu Iniciar, digite **PKIView** para abrir a caixa de diálogo de Enterprise PKI.

    c.  No menu Iniciar, digite **PKIView** para abrir a caixa de diálogo de Enterprise PKI.

    d.  Clique com botão direito **Enterprise PKI** e selecione **contêineres de gerenciar o AD**.

    d.  Verifique se cada certificado de Ger 1 VPN Microsoft autoridade de certificação raiz está presente em:
      - NTAuthCertificates
      - Contêiner AIA
      - Contêiner de autoridades de certificado

## <a name="next-steps"></a>Próximas etapas

[Etapa 7.5. Criar perfis de VPNv2 baseados em OMA-DM para dispositivos Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md): Nesta etapa, você pode criar o OMA-DM com base em perfis de VPNv2 usando o Intune para implantar uma política de configuração do dispositivo VPN. Se você quiser SCCM ou Script do PowerShell para criar perfis de VPNv2, consulte [configurações de CSP de VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) para obter mais detalhes.
