---
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: Onde colocar um proxy do servidor de federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4bde30f694c6490962edaa0c3fe1543e74ba7fd7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842977"
---
# <a name="where-to-place-a-federation-server-proxy"></a>Onde colocar um proxy do servidor de federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode colocar os serviços de Federação do Active Directory \(do AD FS\)proxies do servidor de Federação em uma rede de perímetro para fornecer uma camada de proteção contra usuários mal-intencionados que podem vir da Internet. Proxies do servidor de federação são ideais para o ambiente de rede de perímetro, porque eles não têm acesso às chaves particulares usadas para criar tokens. No entanto, proxies de servidor de federação podem encaminhar eficientemente as solicitações de entrada para servidores de federação que estão autorizados a produzir esses tokens.  
  
Não é necessário para colocar um proxy do servidor de Federação dentro da rede corporativa para o parceiro de conta ou parceiro de recurso, porque os computadores cliente que estão conectados à rede corporativa podem se comunicar diretamente com o servidor de Federação. Nesse cenário, o servidor de Federação também fornece funcionalidade de proxy do servidor de federação para computadores cliente que são provenientes da rede corporativa.  
  
Como é típico com redes de perímetro, uma intranet\-voltados para o firewall é estabelecido entre a rede de perímetro e a rede corporativa e um Internet\-voltados para o firewall geralmente é estabelecido entre a rede de perímetro e o Na Internet. Nesse cenário, o proxy do servidor de Federação fica entre ambos esses firewalls na rede de perímetro.  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>Configurando os servidores de firewall para um proxy do servidor de federação  
Para o federation server proxy processo de redirecionamento seja bem-sucedida, todos os servidores de firewall devem ser configurados para permitir o protocolo \(HTTPS\) tráfego. O uso de HTTPS é necessário porque os servidores de firewall devem publicar o proxy do servidor de federação, usando a porta 443, para que o proxy do servidor de federação na rede de perímetro possa acessar o servidor de federação na rede corporativa.  
  
> [!NOTE]  
> Todas as comunicações e para computadores cliente também ocorre por HTTPS.  
  
Além disso, a Internet\-voltados para o servidor de firewall, como um computador executando o Microsoft Internet Security and Acceleration \(ISA\) servidor, usa um processo conhecido como publicação de servidor para distribuir o cliente da Internet solicitações para o perímetro apropriado e servidores de rede corporativa, como proxies de servidor de Federação ou servidores de Federação.  
  
Regras de publicação de servidor determinam como funciona a publicação de servidor – essencialmente, filtrando todas as solicitações de entrada e saída pelo computador do ISA Server. Regras de publicação de servidor mapeiam solicitações de cliente recebidas para os servidores apropriados atrás do computador do ISA Server. Para obter informações sobre como configurar o ISA Server para publicar um servidor, consulte [criar uma regra de publicação de Web seguro](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
No mundo federado do AD FS, essas solicitações do cliente geralmente são feitas para uma URL específica, por exemplo, uma URL de identificador do servidor de federação, como http://fs.fabrikam.com. Como essas solicitações do cliente são fornecidos da Internet, a Internet\-voltados para o servidor de firewall devem ser configurado para publicar a URL de identificador do servidor de federação para cada proxy do servidor de federação que é implantado na rede de perímetro.  
  
### <a name="configuring-isa-server-to-allow-ssl"></a>Configurando o ISA Server para permitir SSL  
Para facilitar a comunicação segura do AD FS, você deve configurar o ISA Server para permitir o Secure Sockets Layer \(SSL\) as comunicações entre o seguinte:  
  
-   **Servidores de Federação e proxies de servidor de Federação.** Um canal SSL é necessário para todas as comunicações entre servidores de Federação e proxies de servidor de Federação. Portanto, você deve configurar o ISA Server para permitir uma conexão SSL entre a rede corporativa e a rede de perímetro.  
  
-   **Computadores cliente, servidores de Federação e proxies de servidor de Federação.** Para que possa ocorrer a comunicação entre computadores cliente e servidores de Federação ou entre computadores cliente e proxies de servidor de federação, você pode colocar um computador executando o ISA Server na frente do servidor de Federação ou proxy do servidor de Federação.  
  
    Se sua organização executar autenticação de cliente SSL no servidor de Federação ou proxy do servidor de federação, quando você coloca um computador executando o ISA Server na frente do servidor de Federação ou proxy do servidor de federação, o servidor deve ser configurado para passagem\-por meio da conexão SSL porque a conexão SSL deve terminar no servidor de Federação ou proxy do servidor de Federação.  
  
    Se sua organização executar autenticação de cliente SSL no servidor de Federação ou proxy do servidor de federação, uma opção adicional é encerrar a conexão SSL no computador que executa o ISA Server e, em seguida, o re\-estabelecer uma conexão SSL para o servidor de Federação ou proxy do servidor de Federação.  
  
> [!NOTE]  
> O servidor de Federação ou proxy do servidor de Federação requer que a conexão seja protegidas por SSL para proteger o conteúdo do token de segurança.  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
