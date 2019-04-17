---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: "Examine a função do Proxy do servidor de federação no parceiro de recurso"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 31e285e863e4316a8e0a65f9b68c27442290927d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>Examine a função do Proxy do servidor de federação no parceiro de recurso

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Um proxy de servidor de Federação em serviços de Federação do Active Directory \(AD FS\) pode funcionar em um ou mais das funções a seguir, dependendo de como configurar o servidor para atender às necessidades da organização do parceiro de recurso:  
  
-   **Descoberta de parceiros de conta**: computador do cliente de um Internet deve identificar qual conta parceiro será autenticá-la. O cliente encontra o parceiro de conta usando uma conta parceiro descoberta Web forma \(discoverclientrealm.aspx\), que é armazenada no proxy do servidor de federação no parceiro de recurso. Se mais de um parceiro de conta é configurado no AD FS snap\-in Gerenciamento, que aparece um menu suspenso drop\ para o cliente com todos os parceiros de conta disponíveis estão visíveis para computadores de clientes de Internet que acessam a descoberta de parceiros de conta formulário da Web. Você pode alterar como a descoberta de parceiros de conta formulário da Web é apresentada para computadores cliente personalizando o arquivo discoverclientrealm.aspx.  
  
-   **Redirecionamento de token de segurança**: O proxy do servidor de federação no parceiro de conta envia os tokens de segurança para o parceiro de recurso. O proxy de servidor de Federação do recurso aceita esses tokens e passa para o servidor de federação no parceiro de recurso. O servidor de federação de recurso depois, emite um token de segurança que é associado para um servidor de Web do recurso específico. O proxy de servidor de Federação do recurso, em seguida, redireciona o token para o cliente.  
  
Para resumir, um proxy de servidor de Federação do recurso facilita o processo de logon federado redirecionando computadores cliente para um servidor de federação que os clientes possa se autenticar. Um proxy de servidor de Federação do recurso também atua como um proxy para tokens de segurança do cliente para servidores de Federação do recurso.  
  
> [!NOTE]  
> Quando é necessário ajudar a reduzir a quantidade de hardware e o número de certificados necessários, o proxy do servidor de Federação pode ser localizado no mesmo computador que o servidor Web.  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

