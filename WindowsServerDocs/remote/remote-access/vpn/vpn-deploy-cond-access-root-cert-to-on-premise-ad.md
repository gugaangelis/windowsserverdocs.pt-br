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
ms.openlocfilehash: 67d361db7a2dd3f2879e8beb924075dae68d52a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404321"
---
# <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>Etapa 7.4. Implantar certificados raiz de acesso condicional no AD local

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

Nesta etapa, você implanta o certificado raiz de acesso condicional como certificado raiz confiável para autenticação de VPN para seu AD local.

- [**Anterior** Etapa 7.3. Configurar a política de acesso condicional](vpn-config-conditional-access-policy.md)
- [**Última** Etapa 7.5. Criar perfis VPNv2 baseados em OMA-DM para dispositivos Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. Na página **conectividade VPN** , selecione **baixar certificado**.

   >[!NOTE]
   >A opção **baixar certificado Base64** está disponível para algumas configurações que exigem certificados Base64 para implantação.

2. Faça logon em um computador ingressado em domínio com direitos de administrador corporativo e execute estes comandos em um prompt de comando de administrador para adicionar os certificados raiz da nuvem ao repositório *NTauth corporativo* :

   >[!NOTE]
   >Para ambientes em que o servidor VPN não está ingressado no domínio de Active Directory, os certificados raiz de nuvem devem ser adicionados ao repositório de _autoridades de certificação raiz confiáveis_ manualmente.

   | Comando | Descrição |
   | --- | --- |
   | `certutil -dspublish -f VpnCert.cer RootCA` | Cria dois contêineres **Gen 1 da AC raiz de VPN da Microsoft** nos contêineres **CN = AIA** e **CN = autoridades de certificação** e publica cada certificado raiz como um valor no atributo _cACertificate_ da raiz de **VPN da Microsoft Contêineres da CA Gen 1** . |
   | `certutil -dspublish -f VpnCert.cer NTAuthCA` | Cria um contêiner **CN = NTAuthCertificates** nos contêineres **CN = AIA** e **CN = autoridades de certificação** e publica cada certificado raiz como um valor no atributo _cACertificate_ do **CN = Contêiner NTAuthCertificates** . |
   | `gpupdate /force` | Agiliza a adição dos certificados raiz aos computadores cliente e Windows Server. |

3. Verifique se os certificados raiz estão presentes no repositório NTauth corporativo e mostram como confiáveis:
   1. Faça logon em um servidor com direitos de administrador corporativo com as **ferramentas de gerenciamento de autoridade de certificação** instaladas.

   >[!NOTE]
   >Por padrão, as **ferramentas de gerenciamento de autoridade de certificação** são servidores de autoridade de certificação instalados. Eles podem ser instalados em outros servidores de membros como parte das **ferramentas de administração de função** no Gerenciador do servidor.

   1. No servidor VPN, no menu Iniciar, digite **PKIView. msc** para abrir a caixa de diálogo PKI corporativa.
   1. No menu Iniciar, digite **PKIView. msc** para abrir a caixa de diálogo PKI corporativa.
   1. Clique com o botão direito do mouse em **Enterprise PKI** e selecione **gerenciar contêineres do AD**.
   1. Verifique se cada certificado do Microsoft VPN root CA Gen 1 está presente em:
      - NTAuthCertificates
      - Contêiner de AIA
      - Contêiner de autoridades de certificação

## <a name="next-steps"></a>Próximas etapas

[Etapa 7.5. Criar perfis de VPNv2 baseados em OMA-DM para dispositivos Windows 10 @ no__t-0: Nesta etapa, você pode criar perfis de VPNv2 baseados em OMA-DM usando o Intune para implantar uma política de configuração de dispositivo VPN. Se você quiser que o script do SCCM ou do PowerShell crie perfis do VPNv2, consulte [configurações do CSP do VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) para obter mais detalhes.
