---
ms.assetid: 
title: O acesso de clientes reivindicar tipos no AD FS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1e37aded450555d293806d1ed8903a51e3df9424
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
#<a name="client-access-policy-claim-types-in-ad-fs"></a>Tipos no AD FS de declaração de política de acesso do cliente

Para fornecer informações de contexto de solicitação adicional, políticas de acesso do cliente use os seguintes tipos de declaração, AD FS gera de informações de cabeçalho de solicitação para processamento.  Para obter mais informações, consulte [a função do mecanismo requerimentos judiciais ou Extrajudiciais](../technical-reference/the-role-of-the-claims-engine.md).

##<a name="x-ms-forwarded-client-ip"></a>X-MS-encaminhado-Client-IP

Tipo de solicitação: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

Essa declaração do AD FS representa uma "melhor tentativa" na verificar o endereço IP do usuário (por exemplo, o cliente Outlook) que faz a solicitação. Essa declaração pode conter vários endereços IP, incluindo o endereço de cada proxy que encaminhado a solicitação.  Essa declaração é preenchida com um cabeçalho HTTP que está atualmente apenas definido pelo Exchange Online, que preenche o cabeçalho ao passar a solicitação de autenticação para AD FS. O valor da reivindicação pode ser um destes procedimentos:


- Um único endereço IP - o endereço IP do cliente que está conectado diretamente ao Exchange Online

    >! [Observação] O endereço IP de um cliente na rede corporativa aparecerá como o endereço IP da interface externa do proxy de saída da organização ou gateway.

- Um ou mais endereços IP
    - Se o Exchange Online não conseguir determinar o endereço IP do cliente conectado, ele definirá o valor com base no valor de cabeçalho x-encaminhado-para, um cabeçalho não padrão que pode ser incluído em HTTP com base em solicitações e é compatível com muitos clientes, balanceadores de carga e proxies do mercado.
    - Vários endereços IP indicando o endereço IP do cliente e o endereço de cada proxy que passado a solicitação serão ser separados por vírgula.

    >! [Observação] Não estará presentes na lista de endereços IP relacionada à infraestrutura Exchange Online.


>! [Aviso] Exchange Online atualmente dá suporte apenas endereços IPV4; ele não oferece suporte a endereços IPV6. 


## <a name="x-ms-client-application"></a>X-MS-aplicativo cliente

Tipo de solicitação: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

Essa declaração do AD FS representa o protocolo usado pelo cliente final, que corresponde flexível para o aplicativo está sendo usado.  Essa declaração é preenchida com um cabeçalho HTTP que está atualmente apenas definido pelo Exchange Online, que preenche o cabeçalho ao passar a solicitação de autenticação para AD FS. Dependendo do aplicativo, o valor desta declaração será um destes procedimentos:



- No caso de dispositivos que usam o Exchange Active Sync, o valor é Microsoft.Exchange.ActiveSync. 
- Uso do cliente Microsoft Outlook pode resultar em qualquer um dos seguintes valores:
    - Microsoft.Exchange.Autodiscover
    - Microsoft.Exchange.OfflineAddressBook
    - Microsoft.Exchange.RPC
    - Microsoft.Exchange.WebServices
    - Microsoft.Exchange.Mapi
- Outros valores possíveis para esse cabeçalho incluem o seguinte:
    - Microsoft.Exchange.Powershell
    - Microsoft.Exchange.SMTP
    - Microsoft.Exchange.PopImap
    - Microsoft.Exchange.Pop
    - Microsoft.Exchange.Imap

## <a name="x-ms-client-user-agent"></a>X-MS-Client-agente do usuário

Tipo de solicitação: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

Essa declaração do AD FS fornece uma cadeia de caracteres para representar o tipo de dispositivo que o cliente está usando para acessar o serviço. Isso pode ser usado quando os clientes gostaria de evitar o acesso para certos dispositivos (por exemplo, determinados tipos de Smartphones).  Essa declaração é preenchida com um cabeçalho HTTP que está atualmente apenas definido pelo Exchange Online, que preenche o cabeçalho ao passar a solicitação de autenticação para AD FS. Valores de exemplo para essa declaração incluem (mas não está limitados a) os valores abaixo.
>! [Observação] A seguir são exemplos de para um cliente cujo x-ms-aplicativo cliente é "Microsoft.Exchange.ActiveSync" pode conter o valor de x-ms-agente do usuário

- Vortex/1.0
- Apple-iPad1C1/812.1
- Apple-iPhone3C1/811.2
- Apple-iPhone/704.11
- Moto-DROID2/4.5.1
- SAMSUNGSPHD700/100.202
- Android/0.3

>! [Observação] Também é possível que esse valor está vazio.


## <a name="x-ms-proxy"></a>X-MS-Proxy

Tipo de solicitação: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

Essa declaração do AD FS indica que a solicitação passou por meio do proxy do servidor de Federação.  Essa declaração é preenchida pelo proxy do servidor de federação, que preenche o cabeçalho ao passar a solicitação de autenticação para o serviço de Federação back-end. AD FS converte em uma reivindicação. 

O valor da reivindicação é o nome DNS do proxy do servidor de Federação passado a solicitação.

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-ponto de extremidade--caminho absoluto (Active vs passivo)

Tipo de solicitação: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

Esse tipo de declaração pode ser usado para determinar as solicitações originadas clientes (avançados) "ativos" versus "passivos" (navegador-clientes baseados na web). Isso permite que solicitações externas de aplicativos baseados em navegador como o Outlook Web Access, SharePoint Online ou o portal do Office 365 para poder enquanto as solicitações originadas clientes sofisticados como o Microsoft Outlook estiverem bloqueadas.

O valor da reivindicação é o nome do serviço do AD FS que recebeu a solicitação.
