---
ms.assetid: 1b3a03c0-5558-4177-9b2f-e9d6ce3271cd
title: "Examine a função do Proxy do servidor de federação no parceiro de conta"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d2b60ce593c2ca7eb902595ee6a42850cb7605d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>Examine a função do Proxy do servidor de federação no parceiro de conta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A função principal do proxy do servidor de federação na rede de perímetro da organização, parceiro conta de serviços de Federação do Active Directory \(AD FS\) é para coletar as credenciais de autenticação de um computador cliente que faz logon pela Internet e transmitir essas credenciais para o servidor de Federação está localizado dentro da rede corporativa da organização do parceiro de conta. A conta para o computador cliente é armazenada no repositório de atributo do parceiro de conta.  
  
Um proxy de servidor de Federação também pode funcionar em um ou mais das funções a seguir, dependendo de como você configurá-la para atender às necessidades da organização do parceiro de conta:  
  
-   Retransmitir os Tokens de segurança, o servidor de Federação emite um token de segurança para o proxy de servidor de federação, que, em seguida, transmite o token para o computador cliente. O token de segurança é usado para fornecer acesso para esse computador cliente para um terceiro específico.  
  
-   Coletam credenciais — o proxy do servidor de federação usa um padrão cliente logon Web forma \(clientlogon.aspx\) para coletar as credenciais baseadas em password\ por meio de autenticação baseada em forms\. No entanto, você pode personalizar este formulário para aceitar outros tipos de autenticação, como autenticação de cliente Secure Sockets Layer \(SSL\) suportados. Para obter mais informações sobre como personalizar essa página, veja Personalizando o Logon do cliente e páginas de descoberta de território Home \ ([http:///\/go.microsoft.com\/fwlink\/? LinkId\ = 104275](https://go.microsoft.com/fwlink/?LinkId=104275)\). Um proxy de servidor de Federação não aceita credenciais por meio de autenticação integrada do Windows.  
  
Para resumir, um proxy de servidor de federação no parceiro de conta age como um proxy para logons de clientes em um servidor de Federação está localizado na rede corporativa. O proxy do servidor de Federação também facilita a distribuição dos tokens de segurança para clientes da Internet que são destinados a partes confiantes.  
  
> [!CAUTION]  
> Expondo um proxy de servidor de federação na extranet de parceiro de conta do formulário da Web acessível por qualquer pessoa com a Internet de logon do cliente acessará. Isso potencialmente pode deixar sua organização vulnerável a alguns ataques baseados em password\, como ataques de força bruta que podem disparar bloqueios de conta para contas de usuário que são armazenadas no \(AD DS\) Active Directory Domain Services corporativa ou de ataques de dicionário.  
  

## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
