---
title: Tipos de declaração de acesso de cliente no AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 3c68d615bcb7fffd8bb0c91116f5e3b73e252a65
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940340"
---
# <a name="client-access-policy-claim-types-in-ad-fs"></a>Tipos de declaração de política de acesso de cliente no AD FS

Para fornecer informações adicionais de contexto de solicitação, as políticas de acesso para cliente usam os seguintes tipos de declaração, que AD FS gera a partir de informações de cabeçalho de solicitação para processamento.  Para obter mais informações, consulte [a função do mecanismo de declarações](../technical-reference/the-role-of-the-claims-engine.md).

## <a name="x-ms-forwarded-client-ip"></a>X-MS-encaminhar-Client-IP

Tipo de declaração:`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

Essa declaração de AD FS representa uma "melhor tentativa" ao garantir o endereço IP do usuário (por exemplo, o cliente Outlook) fazendo a solicitação. Essa declaração pode conter vários endereços IP, incluindo o endereço de cada proxy que encaminhou a solicitação.Essa declaração é populada a partir de um cabeçalho HTTP que atualmente só é definido pelo Exchange Online, que popula o cabeçalho ao passar a solicitação de autenticação para AD FS. O valor da declaração pode ser um dos seguintes:


- Um único endereço IP-o endereço IP do cliente que está conectado diretamente ao Exchange Online

    >! Anotações O endereço IP de um cliente na rede corporativa será exibido como o endereço IP da interface externa do proxy ou gateway de saída da organização.

- Um ou mais endereços IP
  - Se o Exchange Online não puder determinar o endereço IP do cliente que está se conectando, ele definirá o valor com base no valor do cabeçalho x-Forwarded-for, um cabeçalho não padrão que pode ser incluído em solicitações baseadas em HTTP e tem suporte de vários clientes, balanceadores de carga e proxies no mercado.
  - Vários endereços IP que indicam o endereço IP do cliente e o endereço de cada proxy que passou na solicitação serão separados por uma vírgula.

    >! Anotações Os endereços IP relacionados à infraestrutura do Exchange Online não estarão presentes na lista.


>! Alerta Atualmente, o Exchange Online dá suporte apenas a endereços IPV4; Ele não dá suporte a endereços IPV6.


## <a name="x-ms-client-application"></a>X-MS-cliente-aplicativo

Tipo de declaração:`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

Essa declaração de AD FS representa o protocolo usado pelo cliente final, que corresponde livremente ao aplicativo que está sendo usado.Essa declaração é populada a partir de um cabeçalho HTTP que atualmente só é definido pelo Exchange Online, que popula o cabeçalho ao passar a solicitação de autenticação para AD FS. Dependendo do aplicativo, o valor dessa declaração será um dos seguintes:



- No caso de dispositivos que usam Exchange Active Sync, o valor é Microsoft. Exchange. ActiveSync.
- O uso do cliente do Microsoft Outlook pode resultar em qualquer um dos seguintes valores:
    - Microsoft. Exchange. autodiscover
    - Microsoft. Exchange. OfflineAddressBook
    - Microsoft. Exchange. RPC
    - Microsoft. Exchange. WebServices
    - Microsoft. Exchange. MAPI
- Outros valores possíveis para esse cabeçalho incluem o seguinte:
    - Microsoft. Exchange. PowerShell
    - Microsoft. Exchange. SMTP
    - Microsoft. Exchange. PopImap
    - Microsoft. Exchange. pop
    - Microsoft. Exchange. IMAP

## <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent

Tipo de declaração:`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

Essa declaração de AD FS fornece uma cadeia de caracteres para representar o tipo de dispositivo que o cliente está usando para acessar o serviço. Isso pode ser usado quando os clientes desejarem impedir o acesso a determinados dispositivos (como tipos específicos de Smart Phone).Essa declaração é populada a partir de um cabeçalho HTTP que atualmente só é definido pelo Exchange Online, que popula o cabeçalho ao passar a solicitação de autenticação para AD FS. Os valores de exemplo para essa declaração incluem (mas não estão limitados a) os valores abaixo.
>! Anotações Veja a seguir exemplos de o que o valor x-MS-User-Agent pode conter para um cliente cujo x-MS-Client-Application seja "Microsoft. Exchange. ActiveSync"

- Vortex/1.0
- Apple-iPad1C1/812.1
- Apple-iPhone3C1/811.2
- Apple-iPhone/704.11
- Moto-DROID2/4.5.1
- SAMSUNGSPHD700/100.202
- Android/0.3

>! Anotações Também é possível que esse valor esteja vazio.


## <a name="x-ms-proxy"></a>X-MS-proxy

Tipo de declaração:`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

Essa declaração de AD FS indica que a solicitação passou pelo proxy do servidor de Federação.Essa declaração é preenchida pelo proxy do servidor de Federação, que popula o cabeçalho ao passar a solicitação de autenticação para o back-end Serviço de Federação. AD FS, em seguida, converte-o em uma declaração.

O valor da declaração é o nome DNS do proxy do servidor de Federação que passou na solicitação.

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-Endpoint-Absolute-Path (ativo vs passivo)

Tipo de declaração:`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

Esse tipo de declaração pode ser usado para determinar solicitações originadas de clientes "ativos" (ricos) versus clientes "passivos" (baseados em navegador da Web). Isso permite que solicitações externas de aplicativos baseados em navegador, como o Outlook Acesso via Web, o SharePoint Online ou o portal do Office 365, sejam permitidas enquanto as solicitações originadas de clientes avançados, como o Microsoft Outlook, são bloqueadas.

O valor da declaração é o nome do serviço de AD FS que recebeu a solicitação.
