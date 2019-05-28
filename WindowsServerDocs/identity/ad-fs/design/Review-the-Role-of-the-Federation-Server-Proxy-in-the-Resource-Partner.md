---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: Analisar a função do proxy do servidor de federação no parceiro de recurso
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 377baa8f282f3886284a53b686944fe145b1b15e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190892"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>Analisar a função do proxy do servidor de federação no parceiro de recurso

Um proxy do servidor de federação nos serviços de Federação do Active Directory \(do AD FS\) pode funcionar em uma ou mais das funções a seguir, dependendo de como você configura o servidor para atender às necessidades da organização do parceiro de recurso:  
  
-   **Descoberta de parceiro de conta**: Um computador cliente de Internet deve identificar qual parceiro de conta o autenticará. O cliente encontra o parceiro de conta por meio de um parceiro de conta formulário Web de descoberta \(discoverclientrealm\), que é armazenado no proxy de servidor de federação no parceiro de recurso. Se mais de um parceiro de conta é configurado no snap do gerenciamento do AD FS\-em uma queda\-menu suspenso é exibido para o cliente com todos os parceiros de conta disponíveis que são visíveis aos computadores cliente da Internet que acessam o parceiro de conta descoberta de formulário da Web. Você pode alterar a forma pela qual o formulário Web de descoberta de parceiro de conta. é apresentado aos computadores cliente personalizando o arquivo discoverclientrealm.aspx.  
  
-   **Redirecionamento de token de segurança**: O proxy do servidor de federação no parceiro de conta envia os tokens de segurança para o parceiro de recurso. O proxy de servidor de federação de recurso aceita esses tokens e os passa para o servidor de federação no parceiro de recurso. O servidor de Federação do recurso, em seguida, emite um token de segurança associado a um servidor Web de recurso específico. O proxy de servidor de federação de recurso então redireciona o token para o cliente.  
  
Para resumir, um proxy de servidor de federação de recurso facilita o processo de logon federado redirecionando computadores cliente para um servidor de federação que pode autenticar os clientes. Um proxy de servidor de federação de recurso também atua como um proxy para tokens de segurança de cliente para servidores de Federação do recurso.  
  
> [!NOTE]  
> Quando for necessário ajudar a reduzir a quantidade de hardware e o número de certificados necessários, o proxy do servidor de Federação pode estar localizado no mesmo computador como o servidor Web.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

