---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: Onde colocar um servidor de federação
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 503b1f206786d36364f9f0033ec0ea88330c444f
ms.sourcegitcommit: 2cc251eb5bc3069bf09bc08e06c3478fcbe1f321
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84333857"
---
# <a name="where-to-place-a-federation-server"></a>Onde colocar um servidor de federação

Como prática recomendada de segurança, coloque Serviços de Federação do Active Directory (AD FS) \( AD FS \) servidores de Federação atrás de um firewall e conecte-os à sua rede corporativa para evitar a exposição da Internet. Isso é importante porque os servidores de Federação têm autorização total para conceder tokens de segurança. Portanto, eles devem ter a mesma proteção que um controlador de domínio. Se um servidor de Federação estiver comprometido, um usuário mal-intencionado poderá emitir tokens de acesso completo para todos os aplicativos Web e para servidores de Federação protegidos por Serviços de Federação do Active Directory (AD FS) \( AD FS \) em todas as organizações de parceiros de recursos.  
  
> [!NOTE]  
> Como prática recomendada de segurança, evite ter seus servidores de Federação diretamente acessíveis na Internet. Considere dar aos seus servidores de Federação acesso direto à Internet somente quando você estiver configurando um ambiente de laboratório de teste ou quando sua organização não tiver uma rede de perímetro.  
  
Para redes corporativas típicas, um \- firewall voltado para a intranet é estabelecido entre a rede corporativa e a rede de perímetro, e um \- firewall voltado para a Internet é geralmente estabelecido entre a rede de perímetro e a Internet. Nessa situação, o servidor de Federação fica dentro da rede corporativa e não pode ser acessado diretamente pelos clientes da Internet.  
  
> [!NOTE]  
> Os computadores cliente que estão conectados à rede corporativa podem se comunicar diretamente com o servidor de Federação por meio da autenticação integrada do Windows.  
  
Um proxy de servidor de Federação deve ser colocado na rede de perímetro antes de configurar os servidores de firewall para uso com o AD FS. Para obter mais informações, consulte [Onde colocar um Proxy do servidor de Federação](Where-to-Place-a-Federation-Server-Proxy.md).  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>Configurando os servidores de firewall para um servidor de federação  
Para que os servidores de Federação possam se comunicar diretamente com os proxies de servidor de Federação, o servidor de firewall de intranet deve ser configurado para permitir \( \) o tráfego HTTPS seguro do protocolo de transferência de hipertexto do proxy do servidor de Federação para o servidor de Federação. Isso é um requisito porque o servidor de firewall de intranet deve publicar o servidor de Federação usando a porta 443 para que o proxy do servidor de Federação na rede de perímetro possa acessar o servidor de Federação.  
  
Além disso, o \- servidor de firewall voltado para a intranet, como um servidor executando o Internet Security and Acceleration \( ISA \) Server, usa um processo conhecido como publicação de servidor para distribuir solicitações de clientes de Internet para os servidores de Federação corporativa apropriados. Isso significa que você deve criar manualmente uma regra de publicação de servidor no servidor de intranet que executa o ISA Server que publica a URL do servidor de Federação clusterizada, por exemplo, http: \/ \/ FS.fabrikam.com.  
  
Para obter mais informações sobre como configurar a publicação de servidor em uma rede de perímetro, consulte [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md). Para obter informações sobre como configurar o ISA Server para publicar um servidor, consulte [criar uma regra de publicação segura na Web](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
