---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: Analisar a função do proxy do servidor de federação no parceiro de recurso
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: b431d14abfa66ceb70e5885b4301648936c4c4ff
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969683"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>Analisar a função do proxy do servidor de federação no parceiro de recurso

Um proxy de servidor de Federação no Serviços de Federação do Active Directory (AD FS) \( AD FS \) pode funcionar em uma ou mais das seguintes funções, dependendo de como você configura o servidor para atender às necessidades da organização do parceiro de recurso:

-   **Descoberta de parceiro de conta**: um computador cliente de Internet deve identificar qual parceiro de conta o autenticará. O cliente encontra o parceiro de conta usando um Web Form de descoberta de parceiro de conta \( discoverclientrealm. aspx \) , que é armazenado no proxy do servidor de Federação no parceiro de recurso. Se mais de um parceiro de conta estiver configurado no snap-in de gerenciamento de AD FS \- , um \- menu suspenso aparecerá para o cliente com todos os parceiros de conta disponíveis que são visíveis para os computadores cliente da Internet que acessam o formulário da Web de descoberta de parceiro de conta. Você pode alterar a forma pela qual o formulário Web de descoberta de parceiro de conta. é apresentado aos computadores cliente personalizando o arquivo discoverclientrealm.aspx.

-   **Redirecionamento de token de segurança**: o proxy do servidor de Federação no parceiro de conta envia os tokens de segurança para o parceiro de recurso. O proxy do servidor de Federação de recursos aceita esses tokens e os passa para o servidor de Federação no parceiro de recurso. O servidor de Federação de recurso emite, então, um token de segurança que está associado a um servidor Web de recursos específico. O proxy do servidor de Federação de recurso redireciona o token para o cliente.

Para resumir, um proxy de servidor de Federação de recursos facilita o processo de logon federado redirecionando os computadores cliente para um servidor de Federação que pode autenticar os clientes. Um proxy de servidor de Federação de recursos também atua como um proxy para tokens do Client Security para servidores de Federação de recursos.

> [!NOTE]
> Quando for necessário ajudar a reduzir a quantidade de hardware e o número de certificados necessários, o proxy do servidor de Federação poderá estar localizado no mesmo computador que o servidor Web.

## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

