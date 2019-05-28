---
ms.assetid: 1b3a03c0-5558-4177-9b2f-e9d6ce3271cd
title: Examine a função do proxy do servidor de federação no parceiro de conta
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbceb19d31738bdc5b628a9a2b069e5d3022d145
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190946"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>Examine a função do proxy do servidor de federação no parceiro de conta

A principal função do proxy do servidor de federação na rede de perímetro da organização do parceiro de conta nos serviços de Federação do Active Directory \(do AD FS\) é coletar credenciais de autenticação de um computador cliente que fizer logon pela Internet e passar essas credenciais para o servidor de federação, que está localizado dentro da rede corporativa da organização do parceiro de conta. A conta do computador cliente é armazenada no repositório de atributos do parceiro de conta.  
  
Um proxy do servidor de Federação também pode funcionar em uma ou mais das funções a seguir, dependendo de como você configurá-lo para atender às necessidades da organização do parceiro de conta:  
  
-   Retransmissão de Tokens de segurança — o servidor de Federação emite um token de segurança para o proxy de servidor de federação, que, em seguida, transmite o token para o computador cliente. O token de segurança é usado para fornecer acesso para o computador cliente a uma terceira parte confiável específica.  
  
-   Coletar credenciais — o proxy do servidor de federação usa um logon padrão do cliente Web form \(Clientlogon\) para coletar senha\-com base em credenciais por meio de formulários\-autenticação baseada em. No entanto, você pode personalizar esse formulário para aceitar outros tipos com suporte de autenticação, como o Secure Sockets Layer \(SSL\) autenticação de cliente. Para obter mais informações sobre como personalizar essa página, consulte Personalizando o Logon do cliente e as páginas de descoberta do Home Realm \( [http:\/\/go.microsoft.com\/fwlink\/? LinkId\=104275](https://go.microsoft.com/fwlink/?LinkId=104275)\). Um proxy do servidor de Federação não aceita credenciais por meio da autenticação integrada do Windows.  
  
Para resumir, um proxy do servidor de federação no parceiro de conta age como um proxy para logons de cliente para um servidor de federação que está localizado na rede corporativa. O proxy do servidor de Federação também facilita a distribuição dos tokens de segurança para clientes da Internet que são destinados a partes confiáveis.  
  
> [!CAUTION]  
> Expor um proxy do servidor de federação no parceiro de conta extranet do formulário da Web acessível por qualquer pessoa com a Internet de logon do cliente acessarão. Isso pode deixar sua organização vulnerável a alguns senha\-com base em ataques, como ataques de dicionário ou ataques de força bruta que podem acionar os bloqueios de conta para contas de usuário que são armazenadas no domínio do Active Directory corporativo Os serviços \(AD DS\).  
  

## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
