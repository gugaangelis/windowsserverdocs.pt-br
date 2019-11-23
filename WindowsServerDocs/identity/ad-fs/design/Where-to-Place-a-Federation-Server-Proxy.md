---
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: Onde colocar um proxy do servidor de federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 73e68d03e4f2f76dbaf4a497da551640476d0438
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407821"
---
# <a name="where-to-place-a-federation-server-proxy"></a>Onde colocar um proxy do servidor de federação

Você pode posicionar Serviços de Federação do Active Directory (AD FS) \(AD FS\)proxies de servidor de Federação em uma rede de perímetro para fornecer uma camada de proteção contra usuários mal-intencionados que podem ser provenientes da Internet. Proxies do servidor de federação são ideais para o ambiente de rede de perímetro, porque eles não têm acesso às chaves particulares usadas para criar tokens. No entanto, os proxies do servidor de Federação podem rotear de forma eficiente as solicitações de entrada para os servidores de Federação autorizados a produzir esses tokens.  
  
Não é necessário posicionar um proxy de servidor de Federação dentro da rede corporativa para o parceiro de conta ou o parceiro de recurso, pois os computadores cliente conectados à rede corporativa podem se comunicar diretamente com o servidor de Federação. Nesse cenário, o servidor de Federação também fornece a funcionalidade de proxy do servidor de Federação para computadores cliente provenientes da rede corporativa.  
  
Como é comum com redes de perímetro, um firewall voltado para a intranet\-é estabelecido entre a rede de perímetro e a rede corporativa, e um firewall para a Internet\-é estabelecido com freqüência entre a rede de perímetro e a Internet. Nesse cenário, o proxy do servidor de Federação fica entre ambos os firewalls na rede de perímetro.  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>Configurando os servidores de firewall para um proxy do servidor de federação  
Para que o processo de redirecionamento de proxy do servidor de Federação seja bem-sucedido, todos os servidores de firewall devem ser configurados para permitir que o protocolo de transferência de hipertexto seguro \(HTTPS\) tráfego. O uso de HTTPS é necessário porque os servidores de firewall devem publicar o proxy do servidor de Federação, usando a porta 443, para que o proxy do servidor de Federação na rede de perímetro possa acessar o servidor de Federação na rede corporativa.  
  
> [!NOTE]  
> Todas as comunicações e para computadores cliente também ocorre por HTTPS.  
  
Além disso, o servidor de firewall\-voltado para a Internet, como um computador executando o Microsoft Internet Security and Acceleration \(ISA\) Server, usa um processo conhecido como publicação de servidor para distribuir solicitações de clientes de Internet para os servidores de rede corporativa e de perímetro apropriados, como proxies de servidor de Federação ou servidores de Federação.  
  
Regras de publicação de servidor determinam como funciona a publicação de servidor – essencialmente, filtrando todas as solicitações de entrada e saída pelo computador do ISA Server. Regras de publicação de servidor mapeiam solicitações de cliente recebidas para os servidores apropriados atrás do computador do ISA Server. Para obter informações sobre como configurar o ISA Server para publicar um servidor, consulte [criar uma regra de publicação segura na Web](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
No mundo federado da AD FS, essas solicitações de cliente geralmente são feitas em uma URL específica, por exemplo, uma URL de identificador de servidor de Federação, como http:\//fs.fabrikam.com. Como essas solicitações de cliente vêm da Internet, o servidor de firewall\-voltado para a Internet deve ser configurado para publicar a URL do identificador do servidor de Federação para cada proxy de servidor de Federação implantado na rede de perímetro.  
  
### <a name="configuring-isa-server-to-allow-ssl"></a>Configurando o ISA Server para permitir SSL  
Para facilitar as comunicações AD FS seguras, você deve configurar o ISA Server para permitir protocolo SSL \(SSL\) comunicações entre os seguintes:  
  
-   **Servidores de Federação e proxies de servidor de Federação.** Um canal SSL é necessário para todas as comunicações entre os servidores de Federação e os proxies do servidor de Federação. Portanto, você deve configurar o ISA Server para permitir uma conexão SSL entre a rede corporativa e a rede de perímetro.  
  
-   **Computadores cliente, servidores de Federação e proxies de servidor de Federação.** Para que as comunicações possam ocorrer entre computadores cliente e servidores de Federação ou entre computadores cliente e proxies de servidor de Federação, você pode posicionar um computador executando o ISA Server na frente do servidor de Federação ou proxy de servidor de Federação.  
  
    Se sua organização executa a autenticação de cliente SSL no servidor de Federação ou proxy de servidor de Federação, quando você coloca um computador executando o ISA Server na frente do servidor de Federação ou proxy de servidor de Federação, o servidor deve ser configurado para passar\-por meio da conexão SSL, pois a conexão SSL deve ser encerrada no servidor de Federação ou proxy de servidor de Federação.  
  
    Se a sua organização não executar a autenticação de cliente SSL no servidor de Federação ou no proxy do servidor de Federação, uma opção adicional será encerrar a conexão SSL no computador que está executando o ISA Server e, em seguida,\-estabelecer uma conexão SSL com o servidor de Federação ou com o proxy do servidor de Federação.  
  
> [!NOTE]  
> O servidor de Federação ou proxy de servidor de Federação requer que a conexão seja protegida por SSL para proteger o conteúdo do token de segurança.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
