---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: Analisar a função do proxy do servidor de federação no parceiro de recurso
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: df9e291d85ea8899d7f546956276c60582893fe8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359037"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>Analisar a função do proxy do servidor de federação no parceiro de recurso

Um proxy de servidor de Federação no Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 pode funcionar em uma ou mais das seguintes funções, dependendo de como você configura o servidor para atender às necessidades da organização do parceiro de recurso:  
  
-   **Descoberta de parceiro de conta**: Um computador cliente de Internet deve identificar qual parceiro de conta o autenticará. O cliente encontra o parceiro de conta usando um formulário da Web de descoberta de parceiro de conta @no__t -0discoverclientrealm. aspx @ no__t-1, que é armazenado no proxy do servidor de Federação no parceiro de recurso. Se mais de um parceiro de conta estiver configurado na AD FS snap @ no__t-0in, um menu drop @ no__t-1down aparecerá para o cliente com todos os parceiros de conta disponíveis que são visíveis para os computadores cliente da Internet que acessam a conta da Web de descoberta de parceiro de contas isolada. Você pode alterar a forma pela qual o formulário Web de descoberta de parceiro de conta. é apresentado aos computadores cliente personalizando o arquivo discoverclientrealm.aspx.  
  
-   **Redirecionamento de token de segurança**: O proxy do servidor de Federação no parceiro de conta envia os tokens de segurança para o parceiro de recurso. O proxy do servidor de Federação de recursos aceita esses tokens e os passa para o servidor de Federação no parceiro de recurso. O servidor de Federação de recurso emite, então, um token de segurança que está associado a um servidor Web de recursos específico. O proxy do servidor de Federação de recurso redireciona o token para o cliente.  
  
Para resumir, um proxy de servidor de Federação de recursos facilita o processo de logon federado redirecionando os computadores cliente para um servidor de Federação que pode autenticar os clientes. Um proxy de servidor de Federação de recursos também atua como um proxy para tokens do Client Security para servidores de Federação de recursos.  
  
> [!NOTE]  
> Quando for necessário ajudar a reduzir a quantidade de hardware e o número de certificados necessários, o proxy do servidor de Federação poderá estar localizado no mesmo computador que o servidor Web.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

