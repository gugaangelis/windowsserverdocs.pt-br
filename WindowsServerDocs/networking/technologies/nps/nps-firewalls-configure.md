---
title: Configurar Firewalls de tráfego RADIUS
description: Este tópico fornece uma visão geral de como configurar firewalls para permitir o tráfego de RAIO de servidor de política de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 140111e10eabbece098ae9b7c36746cc663c9cce
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-firewalls-for-radius-traffic"></a>Configurar Firewalls de tráfego RADIUS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Firewalls podem ser configurados para permitir ou bloquear tipos de tráfego IP de e para o computador ou dispositivo no qual o firewall está sendo executado. Se firewalls não estão configuradas corretamente para permitir o tráfego RADIUS entre clientes RADIUS, proxies RADIUS e servidores RADIUS, autenticação de acesso à rede pode falhar, impedindo que os usuários acessem recursos de rede. 

Talvez seja necessário configurar dois tipos de firewall para permitir o tráfego de RAIO:

- Firewall do Windows Defender com segurança avançada no servidor local executando o servidor de política de rede (NPS).
- Firewalls em execução em outros computadores ou dispositivos de hardware.

## <a name="windows-firewall-on-the-local-nps-server"></a>Firewall do Windows no servidor NPS local

Por padrão, o NPS envia e recebe tráfego RADIUS usando User Datagram Protocol \(UDP\) portas 1812, 1813, 1645 e 1646. Firewall do Windows Defender no servidor NPS é configurado automaticamente com exceções, durante a instalação de NPS, para permitir esse tráfego RADIUS sejam enviadas e recebidas.

Portanto, se você estiver usando as portas UDP padrão, você não precisa alterar a configuração do Firewall do Windows Defender para permitir o tráfego de RAIO em servidores NPS.

Em alguns casos, convém alterar as portas NPS usa para o tráfego de RAIO. Se você definir NPS e os servidores de acesso de rede para enviar e receber tráfego RADIUS nas portas diferente dos padrões, você deve fazer o seguinte:

- Remova as exceções que permitem o tráfego RADIUS nas portas padrão.
- Crie novas exceções que permitam o tráfego RADIUS nas novas portas.

Para obter mais informações, consulte [configurar informações da porta UDP NPS](nps-udp-ports-configure.md).

## <a name="other-firewalls"></a>Outros firewalls

No passo de configuração mais comuns, o firewall está conectado à Internet e o servidor NPS é um recurso de intranet está conectado à rede do perímetro.

Para acessar o controlador de domínio dentro da intranet, o servidor NPS pode ter:

- Uma interface de rede do perímetro e uma interface na intranet (o roteamento de IP não está habilitado). 
- Uma única interface na rede de perímetro. Nesta configuração, o NPS se comunica com controladores de domínio através de outro firewall que conecta a rede de perímetro à intranet.

## <a name="configuring-the-internet-firewall"></a>Configurando o firewall da Internet

O firewall está conectado à Internet deve ser configurado com filtros de entrada e saídos na interface da Internet \ (e, opcionalmente, sua interface\ do perímetro de rede), para permitir o encaminhamento de mensagens RADIUS entre o servidor NPS e clientes RADIUS ou proxies na Internet. Filtros adicionais podem ser usados para permitir a passagem de tráfego para servidores Web, servidores VPN e outros tipos de servidores na rede de perímetro.

Pacotes de entrada e saída filtros podem ser configurados na interface da Internet e a interface de rede do perímetro separados.

### <a name="configure-input-filters-on-the-internet-interface"></a>Configure filtros de entrada na Interface da Internet

Configure os seguintes filtros de pacotes de entrada na interface da Internet do firewall para permitir que os seguintes tipos de tráfego:

- Endereço IP de destino da interface de rede do perímetro e porta UDP de destino de 1812 (0x714) do servidor NPS.  Esse filtro permite o tráfego de autenticação RADIUS dos clientes RADIUS baseados na Internet para o servidor NPS. Isso é a porta UDP padrão que é usada pelo NPS, conforme definido em RFC 2865. Se você estiver usando uma porta diferente, substitua esse número de porta por 1812.
- Endereço IP de destino da interface de rede do perímetro e porta UDP de destino de 1813 (0x715) do servidor NPS. Esse filtro permite o tráfego de contabilização de RAIO de clientes RADIUS baseados na Internet para servidor NPS. Isso é a porta UDP padrão que é usada pelo NPS, conforme definido em RFC 2866. Se você estiver usando uma porta diferente, substitua esse número de porta para 1813.
- \(Optional\) endereço IP de destino da interface de rede do perímetro e porta UDP de destino de 1645 \(0x66D\) do servidor NPS. Esse filtro permite o tráfego de autenticação RADIUS dos clientes RADIUS baseados na Internet para o servidor NPS. Esta é a porta UDP que é usada por clientes RADIUS antigos.
- \(Optional\) endereço IP de destino da interface de rede do perímetro e porta UDP de destino de 1646 \(0x66E\) do servidor NPS. Esse filtro permite o tráfego de contabilização de RAIO de clientes RADIUS baseados na Internet para servidor NPS. Esta é a porta UDP que é usada por clientes RADIUS antigos.

### <a name="configure-output-filters-on-the-internet-interface"></a>Configure filtros de saída na Interface da Internet

Configure os seguintes filtros de saída na interface da Internet do firewall para permitir que os seguintes tipos de tráfego:

- Endereço IP de origem da interface de rede do perímetro e a porta UDP origem de 1812 (0x714) do servidor NPS. Esse filtro permite o tráfego de autenticação RADIUS do servidor NPS para os clientes RADIUS baseados na Internet. Isso é a porta UDP padrão que é usada pelo NPS, conforme definido em RFC 2865. Se você estiver usando uma porta diferente, substitua esse número de porta por 1812.
- Endereço IP de origem da interface de rede do perímetro e a porta UDP origem de 1813 (0x715) do servidor NPS. Esse filtro permite o tráfego de contabilização de RAIO do servidor NPS para os clientes RADIUS baseados na Internet. Isso é a porta UDP padrão que é usada pelo NPS, conforme definido em RFC 2866. Se você estiver usando uma porta diferente, substitua esse número de porta para 1813.
- \(Optional\) endereço IP de origem da interface de rede do perímetro e porta UDP de origem de 1645 \(0x66D\) do servidor NPS. Esse filtro permite o tráfego de autenticação RADIUS do servidor NPS para os clientes RADIUS baseados na Internet. Esta é a porta UDP que é usada por clientes RADIUS antigos.
- \(Optional\) endereço IP de origem da interface de rede do perímetro e porta UDP de origem de 1646 \(0x66E\) do servidor NPS. Esse filtro permite o tráfego de contabilização de RAIO do servidor NPS para os clientes RADIUS baseados na Internet. Esta é a porta UDP que é usada por clientes RADIUS antigos.

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>Configure filtros de entrada na Interface de rede do perímetro

Configure os seguintes filtros de entrada na interface de rede de perímetro do firewall para permitir que os seguintes tipos de tráfego:

- Endereço IP de origem da interface de rede do perímetro e a porta UDP origem de 1812 (0x714) do servidor NPS. Esse filtro permite o tráfego de autenticação RADIUS do servidor NPS para os clientes RADIUS baseados na Internet. Isso é a porta UDP padrão que é usada pelo NPS, conforme definido em RFC 2865. Se você estiver usando uma porta diferente, substitua esse número de porta por 1812.
- Endereço IP de origem da interface de rede do perímetro e a porta UDP origem de 1813 (0x715) do servidor NPS. Esse filtro permite o tráfego de contabilização de RAIO do servidor NPS para os clientes RADIUS baseados na Internet. Isso é a porta UDP padrão que é usada pelo NPS, conforme definido em RFC 2866. Se você estiver usando uma porta diferente, substitua esse número de porta para 1813.
- \(Optional\) endereço IP de origem da interface de rede do perímetro e porta UDP de origem de 1645 \(0x66D\) do servidor NPS. Esse filtro permite o tráfego de autenticação RADIUS do servidor NPS para os clientes RADIUS baseados na Internet. Esta é a porta UDP que é usada por clientes RADIUS antigos.
- \(Optional\) endereço IP de origem da interface de rede do perímetro e porta UDP de origem de 1646 \(0x66E\) do servidor NPS. Esse filtro permite o tráfego de contabilização de RAIO do servidor NPS para os clientes RADIUS baseados na Internet. Esta é a porta UDP que é usada por clientes RADIUS antigos.

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>Configure filtros de saída na Interface de rede do perímetro

Configure os seguintes filtros de pacote de saída na interface de rede de perímetro do firewall para permitir que os seguintes tipos de tráfego:

- Endereço IP de destino da interface de rede do perímetro e porta UDP de destino de 1812 (0x714) do servidor NPS. Esse filtro permite o tráfego de autenticação RADIUS dos clientes RADIUS baseados na Internet para o servidor NPS. Isso é a porta UDP padrão que é usada pelo NPS, conforme definido em RFC 2865. Se você estiver usando uma porta diferente, substitua esse número de porta por 1812.
- Endereço IP de destino da interface de rede do perímetro e porta UDP de destino de 1813 (0x715) do servidor NPS. Esse filtro permite o tráfego de contabilização de RAIO de clientes RADIUS baseados na Internet para servidor NPS. Isso é a porta UDP padrão que é usada pelo NPS, conforme definido em RFC 2866. Se você estiver usando uma porta diferente, substitua esse número de porta para 1813.
- \(Optional\) endereço IP de destino da interface de rede do perímetro e porta UDP de destino de 1645 \(0x66D\) do servidor NPS. Esse filtro permite o tráfego de autenticação RADIUS dos clientes RADIUS baseados na Internet para o servidor NPS. Esta é a porta UDP que é usada por clientes RADIUS antigos.
- \(Optional\) endereço IP de destino da interface de rede do perímetro e porta UDP de destino de 1646 \(0x66E\) do servidor NPS. Esse filtro permite o tráfego de contabilização de RAIO de clientes RADIUS baseados na Internet para servidor NPS. Esta é a porta UDP que é usada por clientes RADIUS antigos.

Para maior segurança, você pode usar os endereços IP de cada cliente RADIUS que envia os pacotes através do firewall para definir filtros para o tráfego entre o cliente e o endereço IP do servidor NPS na rede de perímetro.

### <a name="filters-on-the-perimeter-network-interface"></a>Filtros na interface de rede do perímetro

Configure os seguintes filtros de pacotes de entrada na interface de rede de perímetro do firewall da intranet para permitir que os seguintes tipos de tráfego:

- Endereço IP de origem da interface de rede do perímetro do servidor NPS. Este filtro permite o tráfego do servidor NPS na rede de perímetro.

Configure os seguintes filtros de saída na interface de rede de perímetro do firewall da intranet para permitir que os seguintes tipos de tráfego:

- Endereço IP de destino da interface de rede do perímetro do servidor NPS. Este filtro permite o tráfego para o servidor NPS na rede de perímetro.

### <a name="filters-on-the-intranet-interface"></a>Filtros na interface da intranet

Configure os seguintes filtros de entrada na interface da intranet do firewall para permitir que os seguintes tipos de tráfego:

- Endereço IP de destino da interface de rede do perímetro do servidor NPS. Este filtro permite o tráfego para o servidor NPS na rede de perímetro.

Configure os seguintes filtros de pacote de saída na interface da intranet do firewall para permitir que os seguintes tipos de tráfego:

- Endereço IP de origem da interface de rede do perímetro do servidor NPS. Este filtro permite o tráfego do servidor NPS na rede de perímetro.


Para obter mais informações sobre o gerenciamento de NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).




