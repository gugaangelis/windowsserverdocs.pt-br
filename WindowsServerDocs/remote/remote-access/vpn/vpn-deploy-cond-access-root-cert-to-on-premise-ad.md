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
ms.openlocfilehash: 210540846f5d62dfc74a2e629a6b7675ccf9894d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066965"
---
# Etapa 7.4. Implantar certificados de raiz de acesso condicional para locais AD

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

Nesta etapa, você implantar o certificado raiz de acesso condicional como certificado raiz confiável para autenticação de VPN para seus locais AD.

& #171;  [ **Anterior:** etapa 7.3. Configurar a política de acesso condicional](vpn-config-conditional-access-policy.md)<br>
& #187; [ **Próximo:** etapa 7.5. Criar perfis de VPNv2 baseados em OMA DM para dispositivos Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. Na página **conectividade VPN** , clique em **baixar o certificado**. 
   
    ![Baixe o certificado de acesso condicional](../../media/Always-On-Vpn/06.png)

    >[!NOTE]
    >A opção de **Baixar base64 certificado** está disponível para algumas configurações que exigem certificados em base64 para implantação. 

2. Faça logon em um computador ingressado no domínio com direitos de administrador da empresa e execute estes comandos em um prompt de comando de administrador para adicionar os certificados raiz de nuvem ao armazenamento *NTauth corporativo* :

    >[!NOTE]
    >Para ambientes em que o servidor VPN não está associado ao domínio do Active Directory, os certificados raiz de nuvem devem ser adicionados ao armazenamento de _Autoridades de certificação raiz confiáveis_ manualmente.

    |Comando  |Descrição  |  
    |---------|-------------| 
    |`certutil -dspublish -f VpnCert.cer RootCA`     |Cria dois contêineres de **raiz de VPN da Microsoft geração de autoridade de certificação 1** sob o **CN = AIA** e **CN = autoridades de certificação** contêineres e publica cada certificado raiz como um valor no atributo _cACertificate_ da raiz ambos **VPN da Microsoft Geração de autoridade de certificação 1** contêineres.|  
    |`certutil -dspublish -f VpnCert.cer NTAuthCA`   |Cria um **CN = CertificadosAutentNT** recipiente sob o **CN = AIA** e **CN = autoridades de certificação** contêineres e publica cada certificado raiz como um valor no atributo _cACertificate_ do **CN = NTAuthCertificates** contêiner. |  
    |`gpupdate /force`     |Acelera a adicionar os certificados raiz para o Windows server e computadores cliente.  |

3.  Verificar se os certificados raiz estão presentes no armazenamento NTauth corporativo e mostrar como confiável:

    a.  Faça logon em um servidor com direitos de administrador da empresa que possui as **Ferramentas de gerenciamento de autoridade de certificado** instalado.

    >[!NOTE]
    >Por padrão as **Ferramentas de gerenciamento de autoridade de certificado** são instalados servidores de autoridade de certificação. Eles podem ser instalados em outros servidores membros como parte das **Ferramentas de administração de função** no Gerenciador do servidor.

    b.  No servidor VPN, no menu Iniciar, digite **PKIView** para abrir a caixa de diálogo Enterprise PKI.

    c.  No menu Iniciar, digite **PKIView** para abrir a caixa de diálogo Enterprise PKI.

    d.  Clique com botão direito **Enterprise PKI** e selecione **Gerenciar contêineres do AD**.

    d.  Verifique se que cada certificado de geração 1 de autoridade de certificação raiz de VPN da Microsoft está presente em:<ul><li>NTAuthCertificates</li><li>Contêiner AIA</li><li>Contêiner de autoridades de certificação</li></ul>

    
## Próximas etapas
Etapa [7.5. Criar perfis de VPNv2 baseados em OMA DM para dispositivos Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md): nesta etapa, você pode criar OMA DM com base em perfis de VPNv2 usando o Intune para implantar uma política de configuração de dispositivo da VPN. Se você quiser SCCM ou Script do PowerShell para criar perfis de VPNv2, consulte [configurações do CSP VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) para obter mais detalhes.

---
