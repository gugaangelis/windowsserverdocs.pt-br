---
title: Configurar firewalls para tráfego RADIUS
description: Este tópico fornece uma visão geral de como configurar firewalls para permitir o tráfego RADIUS para o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: pashort
author: shortpatti
ms.openlocfilehash: eb7f5c495fa09aed584ef37d668d1fa232d7a497
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405443"
---
# <a name="configure-firewalls-for-radius-traffic"></a>Configurar firewalls para tráfego RADIUS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Os firewalls podem ser configurados para permitir ou bloquear tipos de tráfego IP de e para o computador ou dispositivo no qual o firewall está sendo executado. Se os firewalls não estiverem configurados corretamente para permitir o tráfego RADIUS entre clientes RADIUS, proxies RADIUS e servidores RADIUS, a autenticação de acesso à rede poderá falhar, impedindo que os usuários acessem recursos de rede. 

Talvez seja necessário configurar dois tipos de firewalls para permitir o tráfego RADIUS:

- Windows Defender firewall com segurança avançada no servidor local que executa o servidor de diretivas de rede (NPS).
- Firewalls em execução em outros computadores ou dispositivos de hardware.

## <a name="windows-firewall-on-the-local-nps"></a>Firewall do Windows no NPS local

Por padrão, o NPS envia e recebe o tráfego RADIUS usando o protocolo de datagrama do usuário \(portas UDP\) 1812, 1813, 1645 e 1646. O Windows Defender firewall no NPS é configurado automaticamente com exceções, durante a instalação do NPS, para permitir que esse tráfego RADIUS seja enviado e recebido.

Portanto, se você estiver usando as portas UDP padrão, não será necessário alterar a configuração do Windows Defender firewall para permitir o tráfego RADIUS de e para o NPSs.

Em alguns casos, talvez você queira alterar as portas que o NPS usa para o tráfego RADIUS. Se você configurar o NPS e seus servidores de acesso à rede para enviar e receber o tráfego RADIUS em portas que não sejam os padrões, deverá fazer o seguinte:

- Remova as exceções que permitem o tráfego RADIUS nas portas padrão.
- Crie novas exceções que permitam o tráfego RADIUS nas novas portas.

Para obter mais informações, consulte [Configure NPS UDP Port Information](nps-udp-ports-configure.md).

## <a name="other-firewalls"></a>Outros firewalls

Na configuração mais comum, o firewall está conectado à Internet e o NPS é um recurso de intranet que está conectado à rede de perímetro.

Para acessar o controlador de domínio na intranet, o NPS pode ter:

- Uma interface na rede de perímetro e uma interface na intranet (o roteamento de IP não está habilitado). 
- Uma única interface na rede de perímetro. Nessa configuração, o NPS se comunica com os controladores de domínio por meio de outro firewall que conecta a rede de perímetro à intranet.

## <a name="configuring-the-internet-firewall"></a>Configurando o firewall da Internet

O firewall que está conectado à Internet deve ser configurado com filtros de entrada e saída em sua interface de Internet \(e, opcionalmente, sua interface de perímetro de rede\), para permitir o encaminhamento de mensagens RADIUS entre os clientes NPS e RADIUS ou proxies na Internet. Filtros adicionais podem ser usados para permitir a passagem de tráfego para servidores Web, servidores VPN e outros tipos de servidores na rede de perímetro.

Filtros de pacotes de entrada e saída separados podem ser configurados na interface da Internet e na interface de rede de perímetro.

### <a name="configure-input-filters-on-the-internet-interface"></a>Configurar filtros de entrada na interface de Internet

Configure os seguintes filtros de pacote de entrada na interface de Internet do firewall para permitir os seguintes tipos de tráfego:

- Endereço IP de destino da interface de rede de perímetro e porta de destino UDP de 1812 (0x714) do NPS.  Esse filtro permite o tráfego de autenticação RADIUS de clientes RADIUS baseados na Internet para o NPS. Essa é a porta UDP padrão usada pelo NPS, conforme definido no RFC 2865. Se você estiver usando uma porta diferente, substitua esse número de porta para 1812.
- Endereço IP de destino da interface de rede de perímetro e porta de destino UDP de 1813 (0x715) do NPS. Esse filtro permite o tráfego de contabilização RADIUS de clientes RADIUS baseados na Internet para o NPS. Essa é a porta UDP padrão usada pelo NPS, conforme definido no RFC 2866. Se você estiver usando uma porta diferente, substitua esse número de porta para 1813.
- \(endereço IP de destino\) opcional da interface de rede de perímetro e a porta de destino UDP de 1645 \(0x66D\) do NPS. Esse filtro permite o tráfego de autenticação RADIUS de clientes RADIUS baseados na Internet para o NPS. Essa é a porta UDP usada por clientes RADIUS mais antigos.
- \(endereço IP de destino\) opcional da interface de rede de perímetro e a porta de destino UDP de 1646 \(0x66E\) do NPS. Esse filtro permite o tráfego de contabilização RADIUS de clientes RADIUS baseados na Internet para o NPS. Essa é a porta UDP usada por clientes RADIUS mais antigos.

### <a name="configure-output-filters-on-the-internet-interface"></a>Configurar filtros de saída na interface de Internet

Configure os seguintes filtros de saída na interface de Internet do firewall para permitir os seguintes tipos de tráfego:

- Endereço IP de origem da interface de rede de perímetro e porta de origem UDP 1812 (0x714) do NPS. Esse filtro permite o tráfego de autenticação RADIUS do NPS para clientes RADIUS baseados na Internet. Essa é a porta UDP padrão usada pelo NPS, conforme definido no RFC 2865. Se você estiver usando uma porta diferente, substitua esse número de porta para 1812.
- Endereço IP de origem da interface de rede de perímetro e porta de origem UDP 1813 (0x715) do NPS. Esse filtro permite o tráfego de contabilização RADIUS do NPS para clientes RADIUS baseados na Internet. Essa é a porta UDP padrão usada pelo NPS, conforme definido no RFC 2866. Se você estiver usando uma porta diferente, substitua esse número de porta para 1813.
- \(endereço IP de origem\) opcional da interface de rede de perímetro e a porta de origem UDP 1645 \(0x66D\) do NPS. Esse filtro permite o tráfego de autenticação RADIUS do NPS para clientes RADIUS baseados na Internet. Essa é a porta UDP usada por clientes RADIUS mais antigos.
- \(endereço IP de origem\) opcional da interface de rede de perímetro e a porta de origem UDP 1646 \(0x66E\) do NPS. Esse filtro permite o tráfego de contabilização RADIUS do NPS para clientes RADIUS baseados na Internet. Essa é a porta UDP usada por clientes RADIUS mais antigos.

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>Configurar filtros de entrada na interface de rede de perímetro

Configure os seguintes filtros de entrada na interface de rede de perímetro do firewall para permitir os seguintes tipos de tráfego:

- Endereço IP de origem da interface de rede de perímetro e porta de origem UDP 1812 (0x714) do NPS. Esse filtro permite o tráfego de autenticação RADIUS do NPS para clientes RADIUS baseados na Internet. Essa é a porta UDP padrão usada pelo NPS, conforme definido no RFC 2865. Se você estiver usando uma porta diferente, substitua esse número de porta para 1812.
- Endereço IP de origem da interface de rede de perímetro e porta de origem UDP 1813 (0x715) do NPS. Esse filtro permite o tráfego de contabilização RADIUS do NPS para clientes RADIUS baseados na Internet. Essa é a porta UDP padrão usada pelo NPS, conforme definido no RFC 2866. Se você estiver usando uma porta diferente, substitua esse número de porta para 1813.
- \(endereço IP de origem\) opcional da interface de rede de perímetro e a porta de origem UDP 1645 \(0x66D\) do NPS. Esse filtro permite o tráfego de autenticação RADIUS do NPS para clientes RADIUS baseados na Internet. Essa é a porta UDP usada por clientes RADIUS mais antigos.
- \(endereço IP de origem\) opcional da interface de rede de perímetro e a porta de origem UDP 1646 \(0x66E\) do NPS. Esse filtro permite o tráfego de contabilização RADIUS do NPS para clientes RADIUS baseados na Internet. Essa é a porta UDP usada por clientes RADIUS mais antigos.

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>Configurar filtros de saída na interface de rede de perímetro

Configure os seguintes filtros de pacotes de saída na interface de rede de perímetro do firewall para permitir os seguintes tipos de tráfego:

- Endereço IP de destino da interface de rede de perímetro e porta de destino UDP de 1812 (0x714) do NPS. Esse filtro permite o tráfego de autenticação RADIUS de clientes RADIUS baseados na Internet para o NPS. Essa é a porta UDP padrão usada pelo NPS, conforme definido no RFC 2865. Se você estiver usando uma porta diferente, substitua esse número de porta para 1812.
- Endereço IP de destino da interface de rede de perímetro e porta de destino UDP de 1813 (0x715) do NPS. Esse filtro permite o tráfego de contabilização RADIUS de clientes RADIUS baseados na Internet para o NPS. Essa é a porta UDP padrão usada pelo NPS, conforme definido no RFC 2866. Se você estiver usando uma porta diferente, substitua esse número de porta para 1813.
- \(endereço IP de destino\) opcional da interface de rede de perímetro e a porta de destino UDP de 1645 \(0x66D\) do NPS. Esse filtro permite o tráfego de autenticação RADIUS de clientes RADIUS baseados na Internet para o NPS. Essa é a porta UDP usada por clientes RADIUS mais antigos.
- \(endereço IP de destino\) opcional da interface de rede de perímetro e a porta de destino UDP de 1646 \(0x66E\) do NPS. Esse filtro permite o tráfego de contabilização RADIUS de clientes RADIUS baseados na Internet para o NPS. Essa é a porta UDP usada por clientes RADIUS mais antigos.

Para maior segurança, você pode usar os endereços IP de cada cliente RADIUS que envia os pacotes por meio do firewall para definir filtros para o tráfego entre o cliente e o endereço IP do NPS na rede de perímetro.

### <a name="filters-on-the-perimeter-network-interface"></a>Filtros na interface de rede de perímetro

Configure os seguintes filtros de pacote de entrada na interface de rede de perímetro do firewall da intranet para permitir os seguintes tipos de tráfego:

- Endereço IP de origem da interface de rede de perímetro do NPS. Esse filtro permite o tráfego do NPS na rede de perímetro.

Configure os seguintes filtros de saída na interface de rede de perímetro do firewall da intranet para permitir os seguintes tipos de tráfego:

- Endereço IP de destino da interface de rede de perímetro do NPS. Esse filtro permite o tráfego para o NPS na rede de perímetro.

### <a name="filters-on-the-intranet-interface"></a>Filtros na interface da intranet

Configure os seguintes filtros de entrada na interface de intranet do firewall para permitir os seguintes tipos de tráfego:

- Endereço IP de destino da interface de rede de perímetro do NPS. Esse filtro permite o tráfego para o NPS na rede de perímetro.

Configure os seguintes filtros de pacotes de saída na interface de intranet do firewall para permitir os seguintes tipos de tráfego:

- Endereço IP de origem da interface de rede de perímetro do NPS. Esse filtro permite o tráfego do NPS na rede de perímetro.


Para obter mais informações sobre como gerenciar o NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).




