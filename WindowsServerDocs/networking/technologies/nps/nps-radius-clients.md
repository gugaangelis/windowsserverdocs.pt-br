---
title: Clientes RADIUS
description: Este tópico fornece uma visão geral dos clientes RADIUS para o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1dfc1bb71d2800a8a9587e54147950dfd7fb371f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395999"
---
# <a name="radius-clients"></a>Clientes RADIUS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Um servidor de acesso à rede \(NAS\) é um dispositivo que fornece algum nível de acesso a uma rede maior. Um NAS que usa uma infraestrutura RADIUS também é um cliente RADIUS, enviando solicitações de conexão e mensagens de contabilização para um servidor RADIUS para autenticação, autorização e contabilidade.

>[!NOTE]
>Computadores cliente, como computadores laptop e outros computadores que executam sistemas operacionais cliente, não são clientes RADIUS. Os clientes RADIUS são servidores de acesso à rede, como pontos de acesso sem fio, comutadores de autenticação 802.1 X, rede virtual privada \(servidores VPN\) e servidores dial-up, pois eles usam o protocolo RADIUS para se comunicar com servidores RADIUS, como servidor de diretivas de rede \(servidores\) NPS.

Para implantar o NPS como um servidor RADIUS ou um proxy RADIUS, você deve configurar clientes RADIUS no NPS.

## <a name="radius-client-examples"></a>Exemplos de clientes RADIUS

Os exemplos de servidores de acesso à rede são:

- Servidores de acesso à rede que fornecem conectividade de acesso remoto a uma rede da organização ou à Internet. Um exemplo é um computador que executa o sistema operacional Windows Server 2016 e o serviço de acesso remoto que fornece serviços de acesso remoto de VPN (rede virtual privada) ou dial-up tradicional para uma intranet da organização.
- Pontos de acesso sem fio que fornecem acesso de camada física a uma rede de organização usando tecnologias de recepção e transmissão baseadas em sem fio.
- Opções que fornecem acesso à camada física para a rede de uma organização, usando tecnologias de LAN tradicionais, como Ethernet.
- Proxies RADIUS que encaminham solicitações de conexão para servidores RADIUS que são membros de um grupo de servidores RADIUS remotos que são configurados no proxy RADIUS.

## <a name="radius-access-request-messages"></a>Acesso RADIUS-mensagens de solicitação

Os clientes RADIUS criam mensagens de solicitação de acesso RADIUS e as encaminham para um proxy RADIUS ou servidor RADIUS, ou encaminham mensagens de solicitação de acesso para um servidor RADIUS recebido de outro cliente RADIUS, mas não foram criadas por conta própria.

Os clientes RADIUS não processam mensagens de solicitação de acesso executando autenticação, autorização e contabilidade. Somente os servidores RADIUS executam essas funções.

No entanto, o NPS pode ser configurado como um proxy RADIUS e um servidor RADIUS simultaneamente, para que ele processe algumas mensagens de solicitação de acesso e encaminhe outras mensagens.

## <a name="nps-as-a-radius-client"></a>NPS como um cliente RADIUS

O NPS atua como um cliente RADIUS quando você o configura como um proxy RADIUS para encaminhar mensagens de solicitação de acesso para outros servidores RADIUS para processamento. Quando você usa o NPS como um proxy RADIUS, as seguintes etapas gerais de configuração são necessárias:

1. Os servidores de acesso à rede, como pontos de acesso sem fio e servidores VPN, são configurados com o endereço IP do proxy NPS como o servidor RADIUS designado ou servidor de autenticação. Isso permite que os servidores de acesso à rede, que criam mensagens de solicitação de acesso com base nas informações recebidas de clientes de acesso, encaminhem mensagens para o proxy NPS.

2. O proxy NPS é configurado adicionando cada servidor de acesso à rede como um cliente RADIUS. Essa etapa de configuração permite que o proxy NPS receba mensagens dos servidores de acesso à rede e se comunique com elas durante a autenticação. Além disso, as políticas de solicitação de conexão no proxy NPS são configuradas para especificar quais mensagens de solicitação de acesso devem ser encaminhadas para um ou mais servidores RADIUS. Essas políticas também são configuradas com um grupo de servidores RADIUS remoto, que informa ao NPS para onde enviar as mensagens recebidas dos servidores de acesso à rede.

3. O NPS ou outros servidores RADIUS que são membros do grupo de servidores RADIUS remotos no proxy NPS são configurados para receber mensagens do proxy NPS. Isso é feito configurando o proxy NPS como um cliente RADIUS.

## <a name="radius-client-properties"></a>Propriedades do cliente RADIUS

Quando você adiciona um cliente RADIUS à configuração do NPS por meio do console do NPS ou usando os comandos netsh para comandos do NPS ou do Windows PowerShell, está configurando o NPS para receber mensagens de solicitação de acesso RADIUS de um servidor de acesso à rede ou de um Proxy RADIUS.

Ao configurar um cliente RADIUS no NPS, você pode designar as seguintes propriedades:

### <a name="client-name"></a>Nome do cliente

 Um nome amigável para o cliente RADIUS, que torna mais fácil identificar ao usar o snap-in do NPS ou os comandos netsh para NPS.

### <a name="ip-address"></a>Endereço IP

O protocolo IP versão 4 \(endereço de\) IPv4 ou o sistema de nomes de domínio \(DNS\) nome do cliente RADIUS.

### <a name="client-vendor"></a>Cliente-fornecedor

O fornecedor do cliente RADIUS. Caso contrário, você pode usar o valor padrão RADIUS para o fornecedor do cliente.

### <a name="shared-secret"></a>Segredo compartilhado

Uma cadeia de texto que é usada como uma senha entre clientes RADIUS, servidores RADIUS e proxies RADIUS. Quando o atributo de autenticador de mensagem é usado, o segredo compartilhado também é usado como a chave para criptografar mensagens RADIUS. Essa cadeia de caracteres deve ser configurada no cliente RADIUS e no snap-in do NPS.

### <a name="message-authenticator-attribute"></a>Atributo de autenticador de mensagem

Descrito na RFC 2869, "extensões RADIUS", uma mensagem Digest 5 \(MD5\) hash de toda a mensagem RADIUS. Se o atributo autenticador de mensagem RADIUS estiver presente, ele será verificado. Se a verificação falhar, a mensagem RADIUS será descartada. Se as configurações do cliente exigirem o atributo autenticador de mensagem e ele não estiver presente, a mensagem RADIUS será descartada. O uso do atributo de autenticador de mensagem é recomendado.

>[!NOTE]
>O atributo de autenticador de mensagem é necessário e habilitado por padrão quando você usa o protocolo de autenticação extensível \(autenticação de\) EAP. 

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).

