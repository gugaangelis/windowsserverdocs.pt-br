---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: Onde colocar um servidor de federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 376cec7f3a4fb1f988ac5d458b05220c7b9de970
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857687"
---
# <a name="where-to-place-a-federation-server"></a>Onde colocar um servidor de federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Como uma prática recomendada de segurança, coloque os serviços de Federação do Active Directory \(do AD FS\)servidores de federação na frente de um firewall e conecte-se à sua rede corporativa para evitar a exposição da Internet. Isso é importante porque os servidores de Federação têm autorização total para conceder tokens de segurança. Portanto, eles devem ter a mesma proteção que um controlador de domínio. Se um servidor de Federação estiver comprometido, um usuário mal-intencionado tem a capacidade de emitir tokens de acesso completo para todos os aplicativos Web e servidores de federação que são protegidos pelos serviços de Federação do Active Directory \(do AD FS\) em todos os recursos organizações de parceiros.  
  
> [!NOTE]  
> Como uma segurança melhor prática, evite ter servidores de Federação diretamente acessíveis na Internet. É recomendável atribuir os servidores de Federação acesso direto à Internet somente quando você está configurando um ambiente de laboratório de teste ou quando sua organização não tiver uma rede de perímetro.  
  
Para redes corporativas típicas, uma intranet\-voltados para o firewall é estabelecido entre a rede corporativa e a rede de perímetro e um Internet\-voltados para o firewall geralmente é estabelecido entre a rede de perímetro e o Na Internet. Nessa situação, o servidor de Federação fica dentro da rede corporativa, e não é diretamente acessível pelos clientes de Internet.  
  
> [!NOTE]  
> Computadores cliente que estão conectados à rede corporativa podem se comunicar diretamente com o servidor de federação por meio da autenticação integrada do Windows.  
  
Um proxy do servidor de federação deve ser colocado na rede de perímetro antes de configurar os servidores de firewall para uso com o AD FS. Para obter mais informações, consulte [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>Configurando os servidores de firewall para um servidor de federação  
Para que os servidores de federação podem se comunicar diretamente com proxies do servidor de federação, o servidor de firewall da intranet deve ser configurado para permitir o protocolo \(HTTPS\) o tráfego de proxy do servidor de federação para o servidor de Federação. Este é um requisito porque o servidor de firewall da intranet deve publicar o servidor de Federação usando a porta 443 para que o proxy do servidor de federação na rede de perímetro possa acessar o servidor de Federação.  
  
Além disso, a intranet\-voltados para o servidor de firewall, como um servidor que executa o Internet Security and Acceleration \(ISA\) servidor, usa um processo conhecido como publicação de servidor para distribuir solicitações de cliente da Internet para o servidores de Federação corporativa adequado. Isso significa que você deve criar manualmente uma regra de publicação de servidor no servidor da intranet executando o ISA Server que publica a URL do servidor em cluster de federação, por exemplo, http:\/\/fs.fabrikam.com.  
  
Para obter mais informações sobre como configurar a publicação de servidor em uma rede de perímetro, consulte [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md). Para obter informações sobre como configurar o ISA Server para publicar um servidor, consulte [criar uma regra de publicação na Web segura](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
