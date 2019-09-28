---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: Onde colocar um servidor de federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c64b28f0e62839ff771cd50c0f09b534861cf9e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358782"
---
# <a name="where-to-place-a-federation-server"></a>Onde colocar um servidor de federação

Como prática recomendada de segurança, coloque Serviços de Federação do Active Directory (AD FS) servidores \(AD FS @ no__t-1federation na frente de um firewall e conecte-os à sua rede corporativa para evitar a exposição da Internet. Isso é importante porque os servidores de Federação têm autorização total para conceder tokens de segurança. Portanto, eles devem ter a mesma proteção que um controlador de domínio. Se um servidor de Federação estiver comprometido, um usuário mal-intencionado terá a capacidade de emitir tokens de acesso completo para todos os aplicativos Web e para servidores de Federação protegidos por Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 em todos os parceiros de recursos organizações.  
  
> [!NOTE]  
> Como prática recomendada de segurança, evite ter seus servidores de Federação diretamente acessíveis na Internet. Considere dar aos seus servidores de Federação acesso direto à Internet somente quando você estiver configurando um ambiente de laboratório de teste ou quando sua organização não tiver uma rede de perímetro.  
  
Para redes corporativas típicas, um firewall de intranet @ no__t-0facing é estabelecido entre a rede corporativa e a rede de perímetro, e um firewall de Internet @ no__t-1facing é geralmente estabelecido entre a rede de perímetro e a Internet. Nessa situação, o servidor de Federação fica dentro da rede corporativa e não pode ser acessado diretamente pelos clientes da Internet.  
  
> [!NOTE]  
> Os computadores cliente que estão conectados à rede corporativa podem se comunicar diretamente com o servidor de Federação por meio da autenticação integrada do Windows.  
  
Um proxy de servidor de Federação deve ser colocado na rede de perímetro antes de configurar os servidores de firewall para uso com o AD FS. Para obter mais informações, consulte [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>Configurando os servidores de firewall para um servidor de federação  
Para que os servidores de Federação possam se comunicar diretamente com proxies de servidor de Federação, o servidor de firewall de intranet deve ser configurado para permitir o tráfego de protocolo de transferência de hipertexto seguro \(HTTPS @ no__t-1 do proxy do servidor de Federação para o servidor de Federação. Isso é um requisito porque o servidor de firewall de intranet deve publicar o servidor de Federação usando a porta 443 para que o proxy do servidor de Federação na rede de perímetro possa acessar o servidor de Federação.  
  
Além disso, o servidor de firewall da intranet @ no__t-0facing, como um servidor executando o Internet Security and Acceleration \(ISA @ no__t-2 Server, usa um processo conhecido como publicação de servidor para distribuir solicitações de clientes da Internet para a empresa apropriada servidores de Federação. Isso significa que você deve criar manualmente uma regra de publicação de servidor no servidor de intranet que executa o ISA Server que publica a URL do servidor de Federação clusterizada, por exemplo, http: @no__t -0\/fs.fabrikam.com.  
  
Para obter mais informações sobre como configurar a publicação de servidor em uma rede de perímetro, consulte [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md). Para obter informações sobre como configurar o ISA Server para publicar um servidor, consulte [criar uma regra de publicação segura na Web](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
