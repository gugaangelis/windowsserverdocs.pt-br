---
title: Clientes RADIUS
description: Este tópico fornece uma visão geral dos clientes RADIUS para o servidor de política de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a970dabcc6f3f4fc4d8ed5a0dedd01b5a7dee71a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="radius-clients"></a>Clientes RADIUS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Um servidor de acesso de rede \(NAS\) é um dispositivo que oferece algum nível de acesso a uma rede maior. NAS usando uma infraestrutura RADIUS também é um cliente RADIUS, enviando solicitações de conexão e mensagens de contabilidade para um servidor RADIUS para autenticação, autorização e estatísticas.

>[!NOTE]
>Computadores cliente, como laptops e outros computadores que executam sistemas operacionais cliente, não são clientes RADIUS. Os clientes RADIUS são servidores de acesso à rede - como pontos de acesso sem fio, comutadores de autenticação 802.1 X, servidores \(VPN\) de rede virtual privada e servidores dial-up - porque eles usam o protocolo RADIUS para se comunicar com servidores RADIUS, como servidores NPS \(NPS\).

Para implantar o NPS como um servidor RADIUS ou um proxy RADIUS, você deve configurar clientes RADIUS de NPS.

## <a name="radius-client-examples"></a>Exemplos de cliente RADIUS

Exemplos de servidores de acesso à rede são:

- Servidores de acesso de rede que fornecem conectividade de acesso remoto a uma rede corporativa ou à Internet. Um exemplo é um computador que executa o sistema operacional Windows Server 2016 e o serviço de acesso remoto que fornece a tradicional dial-up ou serviços de acesso remoto de rede virtual privada (VPN) em uma intranet de organização.
- Pontos de acesso sem fio que fornecem acesso de camada física a uma rede corporativa usando tecnologias de transmissão e recepção sem fio.
- Alterna que fornecem acesso de camada física a rede de uma organização, usando tecnologias de LAN tradicionais, como Ethernet.
- Proxies RADIUS que encaminham solicitações de conexão para servidores RADIUS que são membros de um grupo de servidores remotos RADIUS configurado no proxy RADIUS.

## <a name="radius-access-request-messages"></a>Mensagens de solicitação de acesso RADIUS

Clientes RADIUS criar mensagens de solicitação de acesso de RAIO e encaminhá-los para um proxy RADIUS ou servidor RADIUS, ou eles encaminham mensagens de solicitação de acesso a um servidor RADIUS que recebeu de outro cliente RADIUS, mas não tiver criado propriamente ditos.

Clientes RADIUS não processam mensagens de solicitação de acesso executando a autenticação, autorização e estatísticas. Somente servidores RADIUS executam essas funções.

NPS, no entanto, pode ser configurado como um proxy RADIUS e um servidor RADIUS simultaneamente, para que ele processa algumas mensagens de solicitação de acesso e encaminhe outras mensagens.

## <a name="nps-as-a-radius-client"></a>NPS como um cliente RADIUS

NPS atua como um cliente RADIUS quando você configurá-lo como um proxy RADIUS para encaminhar mensagens de solicitação de acesso a outros servidores RADIUS para processamento. Quando você usa o NPS como um proxy RADIUS, as seguintes etapas de configuração gerais são necessárias:

1. Servidores de acesso à rede, como pontos de acesso sem fio e servidores VPN, são configurados com o endereço IP do proxy NPS como o servidor RADIUS designado ou o servidor de autenticação. Isso permite que os servidores de acesso à rede, que criam mensagens de solicitação de acesso com base nas informações que recebem de clientes de acesso, para encaminhar mensagens para o proxy NPS.

2. O proxy NPS é configurado adicionando cada servidor de acesso de rede como um cliente RADIUS. Esta etapa de configuração permite o proxy NPS para receber mensagens dos servidores de acesso à rede e se comunicar com eles em toda a autenticação. Além disso, as políticas de solicitação de conexão no proxy NPS são configuradas para especificar as mensagens de solicitação de acesso para encaminhar mensagens para um ou mais servidores RADIUS. Essas políticas também são configuradas com um grupo de servidores remotos RADIUS, que informa ao NPS onde enviar as mensagens recebidas dos servidores de acesso à rede.

3. O NPS ou outros servidores RADIUS que são membros do grupo de servidores RADIUS remotos no proxy NPS são configurados para receber mensagens do proxy NPS. Isso é feito por meio da configuração do proxy NPS como um cliente RADIUS.

## <a name="radius-client-properties"></a>Propriedades do cliente RADIUS

Quando você adiciona um cliente RADIUS a configuração do NPS por meio do console NPS ou usando os comandos netsh para NPS ou os comandos do Windows PowerShell, você está configurando NPS para receber mensagens de solicitação de acesso RADIUS de um servidor de acesso de rede ou um proxy RADIUS.

Quando você configura um cliente RADIUS de NPS, você pode designar as seguintes propriedades:

### <a name="client-name"></a>Nome do cliente

 Um nome amigável para o cliente RADIUS, o que torna mais fácil identificar ao usar os NPS snap-in ou os comandos netsh para NPS.

### <a name="ip-address"></a>Endereço IP

O endereço do protocolo IP versão 4 \(IPv4\) ou o nome do sistema de nomes de domínio \(DNS\) do cliente RADIUS.

### <a name="client-vendor"></a>Fornecedor do cliente

O fornecedor do cliente RADIUS. Caso contrário, você pode usar o valor padrão do RAIO fornecedor do cliente.

### <a name="shared-secret"></a>Segredo compartilhado

Uma cadeia de caracteres de texto que é usada como uma senha entre clientes RADIUS, servidores RADIUS e proxies RADIUS. Quando o atributo autenticador de mensagem é usado, o segredo compartilhado também é usado como a chave para criptografar as mensagens de RAIO. Essa cadeia de caracteres deve ser configurada no cliente RADIUS e o snap-in NPS.

### <a name="message-authenticator-attribute"></a>Atributo autenticador de mensagem

Descrito em RFC 2869, "RADIUS extensões", um hash de 5 de síntese de mensagem \(MD5\) da mensagem RADIUS inteira. Se o atributo autenticador de mensagem RADIUS estiver presente, ele é verificado. Se a verificação falhar, a mensagem RADIUS é descartada. Se as configurações do cliente exigirem o atributo autenticador de mensagem e ele não estiver presente, a mensagem RADIUS é descartada. É recomendável usar o atributo autenticador de mensagem.

>[!NOTE]
>O atributo autenticador de mensagem é necessário e habilitado por padrão quando você usa Extensible Authentication Protocol \(EAP\) autenticação. 

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).

