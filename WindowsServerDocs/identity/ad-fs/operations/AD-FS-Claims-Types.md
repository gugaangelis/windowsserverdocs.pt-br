---
ms.assetid: ''
title: Acesso para cliente tipos de declarações no AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1e37aded450555d293806d1ed8903a51e3df9424
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839137"
---
#<a name="client-access-policy-claim-types-in-ad-fs"></a>Política de acesso de cliente tipos de declarações no AD FS

Para fornecer informações de contexto de solicitação adicionais, as políticas de acesso do cliente use os seguintes tipos de declaração, o AD FS gera de informações de cabeçalho de solicitação de processamento.  Para obter mais informações, consulte [a função do mecanismo de declarações](../technical-reference/the-role-of-the-claims-engine.md).

##<a name="x-ms-forwarded-client-ip"></a>X-MS-Forwarded-Client-IP

Tipo de declaração: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

Essa declaração do AD FS representa uma "melhor tentativa" atestar o endereço IP do usuário (por exemplo, o cliente Outlook) que está fazendo a solicitação. Essa declaração pode conter vários endereços IP, incluindo o endereço de cada proxy que encaminhou a solicitação.  Essa declaração é preenchida com um cabeçalho HTTP que está atualmente só definidas pelo Exchange Online, que preenche o cabeçalho ao passar a solicitação de autenticação para AD FS. O valor da declaração pode ser um dos seguintes:


- Um único endereço IP – endereço IP do cliente que está conectado diretamente ao Exchange Online

    >! [Observação] O endereço IP de um cliente na rede corporativa será exibido como o endereço IP de interface externa do gateway ou proxy de saída da organização.

- Um ou mais endereços IP
    - Se o Exchange Online não puder determinar o endereço IP do cliente conectado, ele definirá o valor com base no valor do cabeçalho x-forwarded-for, um cabeçalho não padrão que pode ser incluído em HTTP com base em solicitações e é compatível com muitos clientes, balanceadores de carga, e proxies do mercado.
    - Vários endereços IP que indica o endereço IP do cliente e o endereço de cada proxy que passou a solicitação serão ser separados por uma vírgula.

    >! [Observação] Endereços IP relacionados à infraestrutura do Exchange Online não estarão presentes na lista.


>! [Aviso] O Exchange Online atualmente suporta apenas a endereços IPV4; ele não oferece suporte a endereços IPV6. 


## <a name="x-ms-client-application"></a>X-MS-aplicativo de cliente

Tipo de declaração: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

Essa declaração do AD FS representa o protocolo usado pelo cliente final, que corresponde livremente ao aplicativo que está sendo usado.  Essa declaração é preenchida com um cabeçalho HTTP que está atualmente só definidas pelo Exchange Online, que preenche o cabeçalho ao passar a solicitação de autenticação para AD FS. Dependendo do aplicativo, o valor da declaração será um dos seguintes:



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

## <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent

Tipo de declaração: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

Essa declaração do AD FS fornece uma cadeia de caracteres para representar o tipo de dispositivo que o cliente está usando para acessar o serviço. Isso pode ser usado quando os clientes desejarem impedir o acesso para determinados dispositivos (como tipos específicos de Smartphones).  Essa declaração é preenchida com um cabeçalho HTTP que está atualmente só definidas pelo Exchange Online, que preenche o cabeçalho ao passar a solicitação de autenticação para AD FS. Valores de exemplo para essa declaração incluem (mas não estão limitados a) os valores a seguir.
>! [Observação] A seguir estão exemplos de um cliente cujo x-ms-client-aplicativo é "Microsoft.Exchange.ActiveSync" pode conter o valor de x-ms-user-agent

- Vortex 1.0
- Apple-iPad1C1/812.1
- Apple-iPhone3C1/811.2
- Apple-iPhone/704.11
- Moto-DROID2/4.5.1
- SAMSUNGSPHD700/100.202
- Android/0.3

>! [Observação] Também é possível que esse valor está vazio.


## <a name="x-ms-proxy"></a>X-MS-Proxy

Tipo de declaração: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

Essa declaração do AD FS indica que a solicitação tiver passado por meio do proxy de servidor de Federação.  Essa declaração é preenchida com o proxy de servidor de federação, que preenche o cabeçalho ao passar a solicitação de autenticação para o serviço de federação de back-end. O AD FS, em seguida, o converte em uma declaração. 

O valor da declaração é o nome DNS do proxy do servidor de federação que passou a solicitação.

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-ponto de extremidade-Absolute-Path (ativo vs passivo)

Tipo de declaração: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

Esse tipo de declaração pode ser usado para determinar as solicitações provenientes de clientes (avançados) "ativos" versus "passivos" (browser-clientes baseados na web). Isso permite que as solicitações externas de aplicativos baseados em navegador, como o Outlook Web Access, o SharePoint Online ou o portal do Office 365 a serem permitidos enquanto as solicitações provenientes de clientes avançados, como o Microsoft Outlook são bloqueadas.

O valor da declaração é o nome do serviço do AD FS que recebeu a solicitação.
