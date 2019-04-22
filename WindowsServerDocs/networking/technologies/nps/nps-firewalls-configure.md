---
title: Configurar firewalls para tráfego RADIUS
description: Este tópico fornece uma visão geral de como configurar firewalls para permitir o tráfego RADIUS para o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 94b03349f21a40f9bf42508d5a2878a5cf2946cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819447"
---
# <a name="configure-firewalls-for-radius-traffic"></a>Configurar firewalls para tráfego RADIUS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Firewalls podem ser configurados para permitir ou bloquear os tipos de tráfego IP de e para o computador ou dispositivo no qual o firewall está em execução. Se os firewalls não estão configurados corretamente para permitir o tráfego do RADIUS entre clientes RADIUS, proxies RADIUS e servidores RADIUS, autenticação de acesso de rede pode falhar, impedindo que os usuários acessem recursos da rede. 

Talvez você precise configurar dois tipos de firewalls para permitir o tráfego RADIUS:

- Windows Defender Firewall com segurança avançada no servidor local executando o servidor de diretivas de rede (NPS).
- Firewalls em execução em outros computadores ou dispositivos de hardware.

## <a name="windows-firewall-on-the-local-nps"></a>Firewall do Windows no NPS local

Por padrão, o NPS envia e recebe o tráfego RADIUS por meio do protocolo de datagrama de usuário \(UDP\) portas 1812, 1813, 1645 e 1646. Firewall do Windows Defender o NPS é automaticamente configurado com exceções, durante a instalação do NPS, para permitir esse tráfego RADIUS sejam enviadas e recebidas.

Portanto, se você estiver usando portas UDP padrão, você não precisará alterar a configuração do Windows Defender Firewall para permitir o tráfego RADIUS em NPSs.

Em alguns casos, talvez você queira alterar as portas que o NPS usa para o tráfego RADIUS. Se você configurar o NPS e seus servidores de acesso de rede para enviar e receber o tráfego RADIUS em portas diferentes dos padrões, você deve fazer o seguinte:

- Remova as exceções que permitem o tráfego RADIUS nas portas padrão.
- Crie novas exceções que permitem o tráfego RADIUS nas novas portas.

Para obter mais informações, consulte [configurar informações da porta UDP NPS](nps-udp-ports-configure.md).

## <a name="other-firewalls"></a>Outros firewalls

A configuração mais comum, o firewall é conectado à Internet e o NPS é um recurso da intranet que está conectado à rede de perímetro.

Para acessar o controlador de domínio na intranet, o NPS pode ter:

- Uma interface na rede de perímetro e uma interface da intranet (o roteamento IP não está habilitado). 
- Uma única interface da rede de perímetro. Nessa configuração, o NPS se comunica com os controladores de domínio por meio de outro firewall que se conecta a rede de perímetro para a intranet.

## <a name="configuring-the-internet-firewall"></a>Configurando o firewall da Internet

O firewall que está conectado à Internet deve ser configurado com filtros de entrada e saída na interface da Internet \(e, opcionalmente, sua interface do perímetro de rede\), para permitir o encaminhamento de mensagens RADIUS entre o NPS e clientes RADIUS ou proxies na Internet. Filtros adicionais podem ser usados para permitir a passagem do tráfego para servidores Web, servidores VPN e outros tipos de servidores na rede de perímetro.

Filtros podem ser configurados na interface da Internet e a interface de rede de perímetro de pacotes de entrada e saída separados.

### <a name="configure-input-filters-on-the-internet-interface"></a>Configurar filtros de entrada na Interface da Internet

Configure os seguintes filtros de pacotes de entrada na interface da Internet do firewall para permitir os seguintes tipos de tráfego:

- Endereço IP de destino do adaptador de rede de perímetro e a porta de destino UDP de 1812 (0x714) do NPS.  Esse filtro permite o tráfego de autenticação RADIUS dos clientes RADIUS baseados na Internet para o NPS. Isso é a porta UDP padrão que é usada pelo NPS, conforme definido na RFC 2865. Se você estiver usando uma porta diferente, substitua o número da porta por 1812.
- Endereço IP de destino do adaptador de rede de perímetro e a porta de destino UDP 1813 (0x715) do NPS. Esse filtro permite o tráfego de contabilização RADIUS dos clientes RADIUS baseados na Internet para o NPS. Isso é a porta UDP padrão que é usada pelo NPS, conforme definido na RFC 2866. Se você estiver usando uma porta diferente, substitua o número da porta para 1813.
- \(Opcional\) endereço IP de destino do adaptador de rede de perímetro e a porta de destino UDP de 1645 \(0x66D\) do NPS. Esse filtro permite o tráfego de autenticação RADIUS dos clientes RADIUS baseados na Internet para o NPS. Isso é a porta UDP que é usada por clientes RADIUS antigos.
- \(Opcional\) endereço IP de destino do adaptador de rede de perímetro e a porta de destino UDP de 1646 \(0x66E\) do NPS. Esse filtro permite o tráfego de contabilização RADIUS dos clientes RADIUS baseados na Internet para o NPS. Isso é a porta UDP que é usada por clientes RADIUS antigos.

### <a name="configure-output-filters-on-the-internet-interface"></a>Configurar filtros de saída na Interface da Internet

Configure os seguintes filtros de saída na interface da Internet do firewall para permitir os seguintes tipos de tráfego:

- Endereço IP de origem do adaptador de rede de perímetro e a porta de origem UDP de 1812 (0x714) do NPS. Esse filtro permite o tráfego de autenticação RADIUS do NPS para os clientes RADIUS baseados na Internet. Isso é a porta UDP padrão que é usada pelo NPS, conforme definido na RFC 2865. Se você estiver usando uma porta diferente, substitua o número da porta por 1812.
- Endereço IP de origem do adaptador de rede de perímetro e a porta de origem UDP 1813 (0x715) do NPS. Esse filtro permite o tráfego de contabilização RADIUS do NPS para os clientes RADIUS baseados na Internet. Isso é a porta UDP padrão que é usada pelo NPS, conforme definido na RFC 2866. Se você estiver usando uma porta diferente, substitua o número da porta para 1813.
- \(Opcional\) endereço IP de origem do adaptador de rede de perímetro e a porta de origem UDP de 1645 \(0x66D\) do NPS. Esse filtro permite o tráfego de autenticação RADIUS do NPS para os clientes RADIUS baseados na Internet. Isso é a porta UDP que é usada por clientes RADIUS antigos.
- \(Opcional\) endereço IP de origem do adaptador de rede de perímetro e a porta de origem UDP de 1646 \(0x66E\) do NPS. Esse filtro permite o tráfego de contabilização RADIUS do NPS para os clientes RADIUS baseados na Internet. Isso é a porta UDP que é usada por clientes RADIUS antigos.

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>Configurar filtros de entrada na Interface de rede de perímetro

Configure os seguintes filtros de entrada na interface de rede de perímetro do firewall para permitir os seguintes tipos de tráfego:

- Endereço IP de origem do adaptador de rede de perímetro e a porta de origem UDP de 1812 (0x714) do NPS. Esse filtro permite o tráfego de autenticação RADIUS do NPS para os clientes RADIUS baseados na Internet. Isso é a porta UDP padrão que é usada pelo NPS, conforme definido na RFC 2865. Se você estiver usando uma porta diferente, substitua o número da porta por 1812.
- Endereço IP de origem do adaptador de rede de perímetro e a porta de origem UDP 1813 (0x715) do NPS. Esse filtro permite o tráfego de contabilização RADIUS do NPS para os clientes RADIUS baseados na Internet. Isso é a porta UDP padrão que é usada pelo NPS, conforme definido na RFC 2866. Se você estiver usando uma porta diferente, substitua o número da porta para 1813.
- \(Opcional\) endereço IP de origem do adaptador de rede de perímetro e a porta de origem UDP de 1645 \(0x66D\) do NPS. Esse filtro permite o tráfego de autenticação RADIUS do NPS para os clientes RADIUS baseados na Internet. Isso é a porta UDP que é usada por clientes RADIUS antigos.
- \(Opcional\) endereço IP de origem do adaptador de rede de perímetro e a porta de origem UDP de 1646 \(0x66E\) do NPS. Esse filtro permite o tráfego de contabilização RADIUS do NPS para os clientes RADIUS baseados na Internet. Isso é a porta UDP que é usada por clientes RADIUS antigos.

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>Configurar filtros de saída na Interface de rede de perímetro

Configure os seguintes filtros de pacotes de saída na interface de rede de perímetro do firewall para permitir os seguintes tipos de tráfego:

- Endereço IP de destino do adaptador de rede de perímetro e a porta de destino UDP de 1812 (0x714) do NPS. Esse filtro permite o tráfego de autenticação RADIUS dos clientes RADIUS baseados na Internet para o NPS. Isso é a porta UDP padrão que é usada pelo NPS, conforme definido na RFC 2865. Se você estiver usando uma porta diferente, substitua o número da porta por 1812.
- Endereço IP de destino do adaptador de rede de perímetro e a porta de destino UDP 1813 (0x715) do NPS. Esse filtro permite o tráfego de contabilização RADIUS dos clientes RADIUS baseados na Internet para o NPS. Isso é a porta UDP padrão que é usada pelo NPS, conforme definido na RFC 2866. Se você estiver usando uma porta diferente, substitua o número da porta para 1813.
- \(Opcional\) endereço IP de destino do adaptador de rede de perímetro e a porta de destino UDP de 1645 \(0x66D\) do NPS. Esse filtro permite o tráfego de autenticação RADIUS dos clientes RADIUS baseados na Internet para o NPS. Isso é a porta UDP que é usada por clientes RADIUS antigos.
- \(Opcional\) endereço IP de destino do adaptador de rede de perímetro e a porta de destino UDP de 1646 \(0x66E\) do NPS. Esse filtro permite o tráfego de contabilização RADIUS dos clientes RADIUS baseados na Internet para o NPS. Isso é a porta UDP que é usada por clientes RADIUS antigos.

Para aumentar a segurança, você pode usar os endereços IP de cada cliente RADIUS que envia os pacotes por meio do firewall para definir filtros para o tráfego entre o cliente e o endereço IP do NPS na rede de perímetro.

### <a name="filters-on-the-perimeter-network-interface"></a>Filtros na interface de rede de perímetro

Configure os seguintes filtros de pacotes de entrada na interface de rede de perímetro do firewall da intranet para permitir que os seguintes tipos de tráfego:

- Endereço IP de origem da interface de rede de perímetro do NPS. Esse filtro permite o tráfego do NPS na rede de perímetro.

Configure os seguintes filtros de saída na interface de rede de perímetro do firewall da intranet para permitir que os seguintes tipos de tráfego:

- Endereço IP de destino da interface de rede de perímetro do NPS. Esse filtro permite o tráfego para o NPS na rede de perímetro.

### <a name="filters-on-the-intranet-interface"></a>Filtros na interface da intranet

Configure os seguintes filtros de entrada na interface de intranet do firewall para permitir os seguintes tipos de tráfego:

- Endereço IP de destino da interface de rede de perímetro do NPS. Esse filtro permite o tráfego para o NPS na rede de perímetro.

Configure os seguintes filtros de pacotes de saída na interface da intranet do firewall para permitir os seguintes tipos de tráfego:

- Endereço IP de origem da interface de rede de perímetro do NPS. Esse filtro permite o tráfego do NPS na rede de perímetro.


Para obter mais informações sobre como gerenciar o NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).




