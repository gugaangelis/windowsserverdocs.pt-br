---
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: "Onde colocar um Proxy de servidor de Federação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4bde30f694c6490962edaa0c3fe1543e74ba7fd7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="where-to-place-a-federation-server-proxy"></a>Onde colocar um Proxy de servidor de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode colocar proxies de servidor de Federação do serviços de Federação do Active Directory \(AD FS\) em uma rede do perímetro para fornecer uma camada de proteção contra usuários mal-intencionados que podem ser provenientes da Internet. Proxies de servidor de Federação são ideais para o ambiente de rede do perímetro porque eles não têm acesso às chaves particulares que são usados para criar tokens. No entanto, proxies do servidor de federação podem rotear de forma eficiente solicitações de entrada para servidores de federação que estão autorizados a produzir esses tokens.  
  
Não é necessário colocar um proxy de servidor de Federação dentro da rede corporativa para o parceiro de conta ou o parceiro de recurso, porque os computadores cliente que estão conectados à rede corporativa podem se comunicar diretamente com o servidor de Federação. Nesse cenário, o servidor de Federação também fornece funcionalidade de proxy do servidor de federação para computadores cliente que são provenientes de rede corporativa.  
  
Como é típico com redes de perímetro, um firewall intranet\ voltados é estabelecido entre a rede do perímetro e a rede corporativa e um firewall Internet\ voltados geralmente é estabelecido entre a rede do perímetro e a Internet. Nesse cenário, o proxy do servidor de Federação fica entre os dois desses firewalls na rede de perímetro.  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>Configurar os servidores de firewall para um proxy de servidor de Federação  
Para o processo federação servidor proxy redirecionamento seja bem-sucedido, todos os servidores de firewall devem ser configurados para permitir o tráfego \(HTTPS\) Secure Hypertext Transfer Protocol. O uso de HTTPS é necessário porque os servidores de firewall devem publicar o proxy de servidor de federação, usando a porta 443, para que o proxy do servidor de federação na rede de perímetro possa acessar o servidor de federação na rede corporativa.  
  
> [!NOTE]  
> Todas as comunicações de e para os computadores cliente também ocorrerem por HTTPS.  
  
Além disso, o Internet\ voltados para o Firewall do servidor, como um computador com o Microsoft Internet Security and Acceleration \(ISA\) servidor, usa um processo conhecido como publicação no servidor para distribuir solicitações de cliente da Internet para o perímetro apropriado e os servidores de rede corporativa, como proxies de servidor de Federação ou servidores de Federação.  
  
Regras de publicação de servidor determinam como funciona a publicação de servidor — filtragem essencialmente, todas as solicitações de entrada e saídas por meio do computador ISA Server. Regras de publicação de servidor solicitações de cliente são mapeadas para os servidores apropriados atrás do computador ISA Server. Para obter informações sobre como configurar o ISA Server para publicar um servidor, consulte [criar uma regra de publicação de Web segura](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
No mundo federado do AD FS, essas solicitações de cliente normalmente são feitas para uma URL específica, por exemplo, um federação identificador URL do servidor, como http://fs.fabrikam.com. Como essas solicitações de cliente vêm da Internet, o servidor de firewall voltados para Internet\ deve ser configurado para publicar a URL de identificador do servidor de federação para cada proxy de servidor de Federação é implantado na rede de perímetro.  
  
### <a name="configuring-isa-server-to-allow-ssl"></a>Configurando o ISA Server para permitir que o SSL  
Para facilitar a comunicação segura do AD FS, você deve configurar ISA Server para permitir a comunicação Secure Sockets Layer \(SSL\) entre o seguinte:  
  
-   **Servidores de Federação e proxies de servidor de Federação.** Um canal SSL é necessário para todas as comunicações entre servidores de Federação e proxies de servidor de Federação. Portanto, você deve configurar ISA Server para permitir que uma conexão SSL entre a rede corporativa e a rede do perímetro.  
  
-   **Computadores cliente, servidores de Federação e proxies de servidor de Federação.** Para que seja feita comunicações entre computadores cliente e servidores de Federação ou entre computadores cliente e proxies de servidor de federação, você pode colocar um computador executando o ISA Server na frente do servidor de Federação ou o proxy do servidor de Federação.  
  
    Se sua organização realiza autenticação de cliente SSL no servidor de Federação ou proxy do servidor de federação, quando você colocar um computador executando o ISA Server na frente do servidor de Federação ou o proxy do servidor de federação, o servidor deve ser configurado para pass\-por meio da conexão SSL porque deve terminar a conexão SSL no servidor de Federação ou proxy do servidor de Federação.  
  
    Se sua organização não executar a autenticação de cliente SSL no servidor de Federação ou proxy do servidor de federação, uma opção adicional é encerrar a conexão SSL no computador que executa o ISA Server e, em seguida, re\-estabelecer uma conexão SSL para o servidor de Federação ou o proxy do servidor de Federação.  
  
> [!NOTE]  
> O servidor de Federação ou o proxy do servidor de Federação requer que a conexão ser protegidas por SSL para proteger o conteúdo do token de segurança.  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
