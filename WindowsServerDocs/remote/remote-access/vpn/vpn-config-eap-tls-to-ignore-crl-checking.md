---
title: Configurar o EAP-TLS para ignorar a verificação de lista de revogação de certificados (CRL)
description: Um cliente EAP-TLS não pode se conectar, a menos que o servidor NPS conclua uma verificação de revogação da cadeia de certificados (incluindo o certificado raiz) do cliente e verifique se os certificados foram revogados.
services: active-directory
ms.prod: windows-server
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
ms.openlocfilehash: f2c1de01883f2fb52faebb4abf1d0c9e61f0139b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388067"
---
# <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checking"></a>Etapa 7.1. Configurar o EAP-TLS para ignorar a verificação de lista de revogação de certificados (CRL)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Etapa 7. Adicional Acesso condicional para conectividade VPN usando o Azure AD](ad-ca-vpn-connectivity-windows10.md)
- [**Em seguida:** Etapa 7,2. Criar certificados raiz para autenticação de VPN com o Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>A falha na implementação dessa alteração do registro fará com que as conexões IKEv2 usando certificados de nuvem com PEAP falhem, mas conexões IKEv2 usando certificados de autenticação de cliente emitidos pela AC local continuarão funcionando.

Nesta etapa, você pode adicionar **IgnoreNoRevocationCheck** e defini-lo para permitir a autenticação de clientes quando o certificado não inclui pontos de distribuição de CRL. Por padrão, IgnoreNoRevocationCheck é definido como 0 (desabilitado).

>[!NOTE]
>Se um servidor de roteamento e acesso remoto do Windows (RRAS) usar o NPS para chamadas RADIUS de proxy para um segundo NPS, você deverá definir **IgnoreNoRevocationCheck = 1** em ambos os servidores.

Um cliente EAP-TLS não pode se conectar, a menos que o servidor NPS conclua uma verificação de revogação da cadeia de certificados (incluindo o certificado raiz). Os certificados de nuvem emitidos para o usuário pelo Azure AD não têm uma CRL porque eles são certificados de curta duração com um tempo de vida de uma hora. O EAP no NPS precisa ser configurado para ignorar a ausência de uma CRL. Por padrão, IgnoreNoRevocationCheck é definido como 0 (desabilitado). Adicione IgnoreNoRevocationCheck e defina-o como 1 para permitir a autenticação de clientes quando o certificado não incluir pontos de distribuição de CRL. 

Como o método de autenticação é EAP-TLS, esse valor de registro só é necessário em EAP\13. Se outros métodos de autenticação EAP forem usados, o valor do registro também deverá ser adicionado sob eles. 

**Procedure**

1. Abra o **regedit. exe** no servidor NPS.

2. Navegue até **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\rasman\ppp\eap\13**.

3. Selecione **editar > novo** e selecione o **valor DWORD (32 bits)** e insira **IgnoreNoRevocationCheck**.

4. Clique duas vezes em **IgnoreNoRevocationCheck** e defina os dados do valor como **1**.

5. Selecione **OK** e reinicialize o servidor. Reiniciar os serviços RRAS e NPS não é suficiente.

Para obter mais informações, consulte [como habilitar ou desabilitar a CRL (verificação de revogação de certificado) em clientes](https://technet.microsoft.com/library/bb680540.aspx).


|Caminho do Registro  |Extensão EAP  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP-MSCHAP v2         |

## <a name="next-steps"></a>Próximas etapas

[Etapa 7,2. Criar certificados raiz para autenticação VPN com o Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md): nesta etapa, você configura certificados raiz de acesso condicional para autenticação VPN com o Azure AD, que cria automaticamente um aplicativo de nuvem do servidor VPN no locatário.
