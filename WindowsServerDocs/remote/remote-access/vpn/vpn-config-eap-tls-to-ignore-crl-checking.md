---
title: Configurar o EAP-TLS para ignorar a verificação de lista de revogação de certificados (CRL)
description: Um cliente EAP-TLS não pode se conectar, a menos que o servidor NPS conclua uma verificação de revogação da cadeia de certificados (incluindo o certificado raiz) do cliente e verifique se os certificados foram revogados.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 07/13/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: db85d71ed1b7d8d5b3c14ac8ea603789422ea2cb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818879"
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

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

[Etapa 7,2. Criar certificados raiz para autenticação VPN com o Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md): nesta etapa, você configura certificados raiz de acesso condicional para autenticação VPN com o Azure AD, que cria automaticamente um aplicativo de nuvem do servidor VPN no locatário.
