---
title: Configurar o EAP-TLS para ignorar a verificação de lista de revogação de certificados (CRL)
description: Não é possível conectar a um cliente EAP-TLS, a menos que o servidor NPS conclui uma verificação de revogação da cadeia de certificados (incluindo o certificado raiz) do cliente e verifica se os certificados foram revogados.
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
ms.openlocfilehash: ac59c554c69a6138a106a648c3fab3ed4fe05b7b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836417"
---
# <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checking"></a>Etapa 7.1. Configurar o EAP-TLS para ignorar a verificação de lista de revogação de certificados (CRL)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Anterior:** Etapa 7. (Opcional) Acesso condicional para conectividade VPN usando o Azure AD](ad-ca-vpn-connectivity-windows10.md)<br>
&#187; [ **Next:** Etapa 7.2. Criar certificados raiz para autenticação de VPN com o Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>Falha ao implementar essa alteração no registro fará com que conexões IKEv2 usando certificados de nuvem com o PEAP falhe, mas conexões IKEv2 usando certificados de autenticação de cliente emitidos da autoridade de certificação local continuará a funcionar.

Nesta etapa, você pode adicionar **IgnoreNoRevocationCheck** e defini-lo para permitir a autenticação de clientes quando o certificado não inclui os pontos de distribuição da CRL. Por padrão, IgnoreNoRevocationCheck é definido como 0 (desabilitado).

>[!NOTE]
>Se um servidor de acesso remoto (RRAS) e de roteamento do Windows usa o NPS para proxy RADIUS chamadas para um segundo NPS, você deve definir **IgnoreNoRevocationCheck = 1** em ambos os servidores.

Não é possível conectar a um cliente EAP-TLS, a menos que o servidor NPS conclui uma verificação de revogação da cadeia de certificados (incluindo o certificado raiz). Certificados de nuvem emitidos para o usuário pelo AD do Azure não tem uma CRL, porque eles são certificados de curta duração com um tempo de vida de uma hora. EAP no NPS deve ser configurado para ignorar a ausência de uma CRL. Por padrão, IgnoreNoRevocationCheck é definido como 0 (desabilitado). Adicione IgnoreNoRevocationCheck e defini-lo como 1 para permitir a autenticação de clientes quando o certificado não inclui os pontos de distribuição da CRL. 

Como o método de autenticação EAP-TLS, esse valor de registro é necessário apenas em EAP\13. Se outros métodos de autenticação EAP são usados, em seguida, o valor do registro deve ser adicionado abaixo deles também. 

**Procedimento**

1. Abra **regedit.exe** no servidor NPS.

2. Navegue até **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13**.

3. Clique em **Editar > New** e selecione **valor DWORD (32 bits)** e digite **IgnoreNoRevocationCheck**.

4. Clique duas vezes em **IgnoreNoRevocationCheck** e defina os dados do valor como **1**.

5. Clique em **Okey** e reinicialize o servidor. Reiniciar os serviços NPS e RRAS não é suficiente.

Para obter mais informações, consulte [como habilitar ou desabilitar a verificação de revogação de certificados (CRL) em clientes](https://technet.microsoft.com/library/bb680540.aspx).


|Caminho do Registro  |Extensão EAP  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP-MSCHAP v2         |

## <a name="next-step"></a>Próximas etapas

[Etapa 7.2. Criar certificados raiz para autenticação de VPN com o Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md): Nesta etapa, você pode configurar certificados de raiz do acesso condicional para autenticação de VPN com o Azure AD, que cria automaticamente um aplicativo de nuvem do servidor VPN no locatário. 

---