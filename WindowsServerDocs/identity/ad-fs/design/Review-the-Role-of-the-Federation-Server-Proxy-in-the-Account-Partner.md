---
ms.assetid: 1b3a03c0-5558-4177-9b2f-e9d6ce3271cd
title: Examine a função do proxy do servidor de federação no parceiro de conta
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cd04c8e73cb2b8da69d6ab0cf0e8117f51536abf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858559"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>Examine a função do proxy do servidor de federação no parceiro de conta

A função principal do proxy do servidor de Federação na rede de perímetro da organização do parceiro de conta no Serviços de Federação do Active Directory (AD FS) \(AD FS\) é coletar credenciais de autenticação de um computador cliente que faz logon pela Internet e passar essas credenciais para o servidor de Federação, que está localizado dentro da rede corporativa da organização do parceiro de conta. A conta do computador cliente é armazenada no repositório de atributos do parceiro de conta.  
  
Um proxy de servidor de Federação também pode funcionar em uma ou mais das seguintes funções, dependendo de como você configurá-lo para atender às necessidades da organização do parceiro de conta:  
  
-   Tokens de segurança de retransmissão – o servidor de Federação emite um token de segurança para o proxy do servidor de Federação, que então retransmite o token para o computador cliente. O token de segurança é usado para fornecer acesso para o computador cliente a uma terceira parte confiável específica.  
  
-   Coletar credenciais – o proxy do servidor de Federação usa um formulário da Web de logon do cliente padrão \(Clientlogon. aspx\) para coletar credenciais baseadas em\-de senha por meio de autenticação baseada em\-de formulários. No entanto, você pode personalizar esse formulário para aceitar outros tipos de autenticação com suporte, como protocolo SSL \(SSL\) autenticação de cliente. Para obter mais informações sobre como personalizar essa página, consulte Personalizando páginas de descoberta de cliente e Home Realm \([http:\/\/go.microsoft.com\/fwlink\/? LinkId\=\)104275](https://go.microsoft.com/fwlink/?LinkId=104275) . Um proxy de servidor de Federação não aceita credenciais por meio da autenticação integrada do Windows.  
  
Para resumir, um proxy de servidor de Federação no parceiro de conta atua como um proxy para logons de cliente para um servidor de Federação localizado na rede corporativa. O proxy do servidor de Federação também facilita a distribuição de tokens de segurança para clientes da Internet destinados a terceiras partes confiáveis.  
  
> [!CAUTION]  
> Expor um proxy de servidor de Federação na extranet do parceiro de conta será o formulário da Web de logon do cliente acessível por qualquer pessoa com acesso à Internet. Isso pode deixar sua organização vulnerável a alguns ataques baseados em\-de senha, como ataques de dicionário ou ataques de força bruta que podem disparar bloqueios de conta para contas de usuário armazenadas no Active Directory Domain Services corporativo \(AD DS\).  
  

## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
