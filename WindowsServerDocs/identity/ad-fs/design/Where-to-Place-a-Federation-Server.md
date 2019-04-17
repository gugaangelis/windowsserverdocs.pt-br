---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: "Onde colocar um servidor de Federação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 376cec7f3a4fb1f988ac5d458b05220c7b9de970
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="where-to-place-a-federation-server"></a>Onde colocar um servidor de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Como uma segurança práticas recomendadas, coloque os serviços de Federação do Active Directory \(AD FS\) federação servidores na frente de um firewall e conectá-las à sua rede corporativa para evitar a exposição da Internet. Isso é importante porque os servidores de federação tem autorização completa para conceder tokens de segurança. Portanto, eles devem ter a mesma proteção que um controlador de domínio. Se um servidor de Federação está comprometido, um usuário mal-intencionado tem a capacidade de emitir tokens de acesso completo a todos os aplicativos da Web e servidores de federação que são protegidos por serviços de Federação do Active Directory \(AD FS\) em todas as organizações de parceiro de recurso.  
  
> [!NOTE]  
> Como uma segurança prática recomendada, evite ter seus servidores de Federação diretamente acessíveis na Internet. Talvez seja conveniente dar acesso direto à Internet de seus servidores de Federação somente quando você está configurando um ambiente de laboratório de teste ou quando sua organização não tem uma rede do perímetro.  
  
Para redes corporativas típicas, um firewall intranet\ voltados é estabelecido entre a rede corporativa e a rede do perímetro e um firewall Internet\ voltados geralmente é estabelecido entre a rede do perímetro e a Internet. Nessa situação, o servidor de Federação fica dentro da rede corporativa e não está diretamente acessível por clientes na Internet.  
  
> [!NOTE]  
> Computadores cliente que estão conectados à rede corporativa podem se comunicar diretamente com o servidor de federação por meio de autenticação integrada do Windows.  
  
Um proxy de servidor de federação deve ser colocado na rede de perímetro antes de você configurar os servidores de firewall para uso com o AD FS. Para obter mais informações, consulte [onde colocar um Proxy de servidor de Federação](Where-to-Place-a-Federation-Server-Proxy.md).  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>Configurar os servidores de firewall para um servidor de Federação  
Para que os servidores de federação podem se comunicar diretamente com proxies de servidor de federação, o servidor de firewall da intranet deve ser configurado para permitir que o Secure Hypertext Transfer Protocol \(HTTPS\) tráfego de proxy do servidor de federação para o servidor de Federação. Isso é uma exigência porque o servidor de firewall da intranet deve publicar o servidor de Federação usando a porta 443 para que o proxy do servidor de federação na rede de perímetro possa acessar o servidor de Federação.  
  
Além disso, o intranet\ voltados para o Firewall do servidor, como um servidor executando Internet Security and Acceleration \(ISA\) servidor, usa um processo conhecido como publicação no servidor para distribuir as solicitações de cliente de Internet para os servidores de Federação corporativa apropriado. Isso significa que você deve criar uma regra de publicação do servidor manualmente no servidor da intranet executando o ISA Server que publica a URL de servidor de Federação clusterizado, por exemplo, http:///\/fs.fabrikam.com.  
  
Para obter mais informações sobre como configurar a publicação de servidor em uma rede do perímetro, consulte [onde colocar um Proxy de servidor de Federação](Where-to-Place-a-Federation-Server-Proxy.md). Para obter informações sobre como configurar o ISA Server para publicar um servidor, consulte [criar uma regra de publicação da Web segura](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
