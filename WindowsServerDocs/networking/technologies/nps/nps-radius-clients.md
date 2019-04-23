---
title: Clientes RADIUS
description: Este tópico fornece uma visão geral dos clientes RADIUS para o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fdca45237d971c1b2e08443112b63d3ce77e4a2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874307"
---
# <a name="radius-clients"></a>Clientes RADIUS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Um servidor de acesso de rede \(NAS\) é um dispositivo que fornece algum nível de acesso a uma rede maior. NAS usando uma infraestrutura RADIUS também é um cliente RADIUS, enviando solicitações de conexão e mensagens de estatísticas para um servidor RADIUS para autenticação, autorização e contabilidade.

>[!NOTE]
>Computadores cliente, como laptops e outros computadores que executam sistemas operacionais cliente, não são clientes RADIUS. Os clientes RADIUS são servidores de acesso de rede – como pontos de acesso sem fio, comutadores de autenticação 802.1X, rede virtual privada \(VPN\) servidores e servidores dial-up - porque eles usam o protocolo RADIUS para se comunicar com RADIUS servidores como servidor de políticas de rede \(NPS\) servidores.

Para implantar o NPS como um servidor RADIUS ou um proxy RADIUS, você deve configurar clientes RADIUS no NPS.

## <a name="radius-client-examples"></a>Exemplos de cliente RADIUS

Exemplos de servidores de acesso de rede são:

- Servidores de acesso de rede que fornecem conectividade de acesso remoto a uma rede da organização ou na Internet. Um exemplo é um computador executando o sistema operacional Windows Server 2016 e o serviço de acesso remoto que fornece a tradicional dial-up ou serviços de acesso remoto de rede virtual privada (VPN) para uma intranet da organização.
- Pontos de acesso sem fio que fornecem acesso de camada física a uma rede da organização usando tecnologias de transmissão e recepção sem fio.
- Opções que fornecem acesso à camada física à rede da organização, usando tecnologias de LAN tradicionais, como Ethernet.
- Proxies RADIUS que encaminhe as solicitações de conexão para os servidores RADIUS que são membros de um grupo de servidores remotos RADIUS configurado no proxy de RADIUS.

## <a name="radius-access-request-messages"></a>Mensagens de solicitação de acesso RADIUS

Clientes RADIUS criar mensagens de solicitação de acesso RADIUS e encaminhá-los para um proxy RADIUS ou um servidor RADIUS, ou eles encaminharem mensagens de solicitação de acesso a um servidor RADIUS que tenha recebido de outro cliente RADIUS, mas não tiver criado a mesmos.

Clientes RADIUS não processam mensagens de solicitação de acesso por executar autenticação, autorização e contabilização. Somente servidores RADIUS executam essas funções.

NPS, no entanto, pode ser configurado como um proxy RADIUS e um servidor RADIUS simultaneamente, para que ele processa algumas mensagens de solicitação de acesso e encaminha as outras mensagens.

## <a name="nps-as-a-radius-client"></a>NPS como um cliente RADIUS

O NPS atua como um cliente RADIUS quando você o configura como um proxy RADIUS para encaminhar mensagens de solicitação de acesso a outros servidores RADIUS para processamento. Quando você usa o NPS como um proxy RADIUS, as etapas de configuração geral a seguir são necessárias:

1. Servidores de acesso de rede, como pontos de acesso sem fio e servidores VPN, são configurados com o endereço IP do proxy com o NPS como servidor RADIUS de designado ou do servidor de autenticação. Isso permite que os servidores de acesso de rede, que criar mensagens de solicitação de acesso com base nas informações recebidas dos clientes de acesso, para encaminhar mensagens para o proxy do NPS.

2. O proxy NPS está configurado com a adição de cada servidor de acesso de rede como um cliente RADIUS. Essa etapa de configuração permite que o proxy do NPS para receber mensagens dos servidores de acesso à rede e se comunicar com eles em toda a autenticação. Além disso, as diretivas de solicitação de conexão no proxy NPS são configuradas para especificar quais mensagens de solicitação de acesso para encaminhar para um ou mais servidores RADIUS. Essas políticas também são configuradas com um grupo de servidores RADIUS remotos, que informa ao NPS para onde enviar as mensagens recebidas dos servidores de acesso à rede.

3. O NPS ou outros servidores RADIUS que são membros do grupo de servidores RADIUS remotos no proxy NPS são configurados para receber mensagens do proxy NPS. Isso é feito por meio da configuração de proxy NPS como um cliente RADIUS.

## <a name="radius-client-properties"></a>Propriedades do cliente RADIUS

Quando você adiciona um cliente RADIUS para a configuração do NPS por meio do console do NPS ou por meio do uso de comandos do Windows PowerShell ou comandos netsh para NPS, você está configurando o NPS para receber mensagens de solicitação de acesso RADIUS de um servidor de acesso de rede ou um Proxy RADIUS.

Quando você configura um cliente RADIUS no NPS, você pode designar as propriedades a seguir:

### <a name="client-name"></a>Nome do cliente

 Um nome amigável para o cliente RADIUS, o que torna mais fácil identificar ao usar os NPS snap-in ou os comandos netsh para NPS.

### <a name="ip-address"></a>Endereço IP

O protocolo IP versão 4 \(IPv4\) endereço ou o sistema de nomes de domínio \(DNS\) nome do cliente RADIUS.

### <a name="client-vendor"></a>Client-Vendor

O fornecedor do cliente RADIUS. Caso contrário, você pode usar o valor padrão RADIUS para o fornecedor do cliente.

### <a name="shared-secret"></a>Segredo compartilhado

Uma cadeia de caracteres de texto que é usada como uma senha entre os clientes RADIUS, servidores RADIUS e proxies RADIUS. Quando esse atributo é usado, o segredo compartilhado também é usado como a chave para criptografar mensagens RADIUS. Essa cadeia de caracteres deve ser configurada no cliente RADIUS e o snap-in NPS.

### <a name="message-authenticator-attribute"></a>Atributo autenticador de mensagem

Descrito na RFC 2869, "RADIUS Extensions", uma Message Digest 5 \(MD5\) hash de toda a mensagem RADIUS. Se o atributo de autenticador de mensagem RADIUS estiver presente, ele é verificado. Se a verificação falhar, a mensagem RADIUS é descartada. Se as configurações do cliente requerem o atributo de autenticador de mensagem e não estiver presente, a mensagem RADIUS será descartada. É recomendável o uso do atributo autenticador de mensagem.

>[!NOTE]
>Esse atributo é necessária e habilitado por padrão, quando você usa o protocolo de autenticação extensível \(EAP\) autenticação. 

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).

