---
title: Configurar o EAP-TLS para ignorar a verificação de lista de revogação de certificados (CRL)
description: Um cliente de EAP-TLS não pode se conectar, a menos que o servidor NPS conclui a verificação de revogação de cadeia de certificados (incluindo o certificado raiz) do cliente e verifica que os certificados foram revogados.
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
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066982"
---
# Etapa 7.1. Configurar o EAP-TLS para ignorar a verificação de lista de revogação de certificados (CRL)

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** etapa 7. (Opcional) Acesso condicional para conectividade VPN usando o Azure AD](ad-ca-vpn-connectivity-windows10.md)<br>
& #187; [ **Próximo:** etapa 7.2. Criar certificados raiz para autenticação de VPN com o Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>Falha ao implementar essa alteração do registro causará conexões IKEv2 usando certificados de nuvem com o PEAP falhem, mas conexões IKEv2 usando certificados de autenticação de cliente emitidos da CA no local continuará a funcionar.

Nesta etapa, você pode adicionar **IgnoreNoRevocationCheck** e defini-lo para permitir a autenticação de clientes quando o certificado não inclui pontos de distribuição de CRL. Por padrão, IgnoreNoRevocationCheck é definida como 0 (desabilitado).

>[!NOTE]
>Se um servidor de acesso remoto (RRAS) e de roteamento do Windows usa o NPS para proxy RADIUS chamadas para um segundo NPS, então você deve configurar **IgnoreNoRevocationCheck = 1** em ambos os servidores.

Um cliente de EAP-TLS não pode se conectar, a menos que o servidor NPS conclui a verificação de revogação de cadeia de certificados (incluindo o certificado raiz). Certificados de nuvem emitidos para o usuário pelo Azure AD não têm uma CRL porque eles são certificados de curta duração com uma vida útil de uma hora. EAP no NPS precisa ser configurado para ignorar a ausência de uma lista de certificados Revogados. Por padrão, IgnoreNoRevocationCheck é definida como 0 (desabilitado). Adicione IgnoreNoRevocationCheck e defina-o como 1 para permitir que a autenticação de clientes quando o certificado não inclui pontos de distribuição de CRL. 

Como o método de autenticação EAP-TLS, esse valor do registro é necessário somente em EAP\13. Se outros métodos de autenticação EAP forem usados, em seguida, o valor do registro deve ser adicionado em aqueles também. 

**Procedimento**

1. Abra **regedit.exe** no servidor NPS.

2. Navegue até **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13**.

3. Clique em **Editar > New** e selecione o **valor DWORD (32 bits)** e digite **IgnoreNoRevocationCheck**.

4. Clique duas vezes em **IgnoreNoRevocationCheck** e defina os dados de valor como **1**.

5. Clique em **Okey** e reinicialize o servidor. Reiniciando os serviços RRAS e NPS não é suficiente.

Para obter mais informações, veja [como habilitar ou desabilitar a verificação de revogação de certificados (CRL) nos clientes](https://technet.microsoft.com/library/bb680540.aspx).


|Caminho do Registro  |Extensão EAP  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP-MSCHAP v2         |

## Próximas etapas

Etapa [7.2. Criar certificados raiz para autenticação de VPN com o Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md): nesta etapa, você deve configurar certificados raiz de acesso condicional para autenticação de VPN com o Azure AD, que automaticamente cria um aplicativo de nuvem do servidor VPN no locatário. 

---