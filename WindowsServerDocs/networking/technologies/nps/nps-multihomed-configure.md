---
title: Configurar NPS em um computador de hospedagem múltipla
description: Este tópico fornece instruções sobre como configurar um servidor com vários adaptadores de rede que está executando o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f80e83a4d79036729b6b442e6362d52fbda12edd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-nps-on-a-multihomed-computer"></a>Configurar NPS em um computador de hospedagem múltipla

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para configurar um servidor NPS com vários adaptadores de rede.

Quando você usa vários adaptadores de rede em um servidor que executa o servidor de política de rede (NPS), você pode configurar o seguinte:

- Os adaptadores de rede que não e enviar e receber tráfego de autenticação remota Dial-In User Service \(RADIUS\).
- Em uma base de adaptador por rede, se o NPS monitora o tráfego RADIUS em protocolo IP versão 4 \(IPv4\), IPv6, ou IPv4 e IPv6.
- Os números de porta UDP sobre quais RADIUS tráfego é enviado e recebido por protocolo \ (IPv4 ou IPv6\), base de adaptador por rede.

Por padrão, NPS escuta tráfego RADIUS nas portas 1812, 1813, 1645 e 1646 IPv6 e IPv4 para todos os adaptadores de rede instalados. Como NPS usa automaticamente todos os adaptadores de rede para tráfego RADIUS, você só precisa especificar os adaptadores de rede que você deseja NPS deverá usar no RAIO de tráfego quando você deseja impedir que o NPS usando um adaptador de rede específica.

>[!NOTE]
>Se você desinstalar o IPv4 ou IPv6 em um adaptador de rede, NPS não monitora o tráfego de RAIO para o protocolo desinstalado.

Em um servidor NPS que tem vários adaptadores de rede instalados, convém configurar o NPS para enviar e receber tráfego RADIUS somente em adaptadores que você especificar.

Por exemplo, um adaptador de rede instalado no servidor NPS pode levar a um segmento de rede que não contém clientes RADIUS, enquanto um segundo adaptador de rede oferece NPS com um caminho de rede para ele tiver configurado clientes RADIUS. Nesse cenário, é importante direcionar o NPS a usar o segundo adaptador de rede para todo o tráfego de RAIO.

Em outro exemplo, se o servidor NPS tem três adaptadores de rede instalados, mas você deseja apenas NPS usar dois dos adaptadores de tráfego RADIUS, você pode configurar informações de porta para os dois adaptadores. Excluindo a configuração de porta para o adaptador de terceiro, você evita que NPS usando o adaptador para o tráfego de RAIO.

## <a name="using-a-network-adapter"></a>Usando um adaptador de rede

Para configurar o NPS para ouvir e envie tráfego RADIUS em um adaptador de rede, use a seguinte sintaxe na caixa de diálogo Propriedades do servidor de política de rede no console do NPS:

- Sintaxe de tráfego IPv4: IPAddress: UDPport, onde IPAddress é o endereço IPv4 configurado no adaptador de rede por meio da qual você deseja enviar tráfego RADIUS e UDPport é o número da porta RADIUS que você deseja usar na autenticação RADIUS ou contabilização do tráfego.
- Sintaxe de tráfego IPv6: [IPv6Address]: UDPport, onde os colchetes ao redor IPv6Address são necessários, IPv6Address é o endereço IPv6 que está configurado no adaptador de rede por meio da qual você deseja enviar tráfego RADIUS e UDPport é o número da porta RADIUS que você deseja usar na autenticação RADIUS ou contabilização do tráfego.

Os seguintes caracteres podem ser usados como delimitadores para configurar o endereço IP e informações de porta UDP:

- Endereço/porta delimitador: pontos (:)
- Delimitador de porta: vírgula (,)
- Interface delimitador:-e-vírgula (;)

## <a name="configuring-network-access-servers"></a>Configurando os servidores de acesso à rede

Certifique-se de que os servidores de acesso de rede são configurados com os mesmos números de porta UDP RADIUS que você configurar em seus servidores NPS. As portas UDP RADIUS padrão definidas nas RFCs 2865 e 2866 são 1812 para autenticação e 1813 para contabilização; No entanto, alguns servidores de acesso são configurados por padrão para usar a porta UDP 1645 para solicitações de autenticação e a porta UDP 1646 para solicitações de contabilidade.

>[!IMPORTANT]
>Se você não usar os números de porta padrão RADIUS, você deve configurar exceções no firewall para o computador local permitir o tráfego RADIUS nas novas portas. Para obter mais informações, consulte [configurar Firewalls para o tráfego de RAIO](nps-firewalls-configure.md).

## <a name="configure-the-multihomed-nps-server"></a>Configurar o servidor NPS de hospedagem múltipla

Você pode usar o procedimento a seguir para configurar o servidor NPS de hospedagem múltipla.

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para concluir este procedimento.

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>Para especificar o adaptador de rede e as portas UDP que NPS usa para o tráfego de RAIO

1. No Gerenciador do servidor, clique em **ferramentas**e clique em **NPS** para abrir o console NPS.

2. Clique com botão direito **NPS**e clique em **propriedades**.

3. Clique no **portas** guia e preceda com o endereço IP para o adaptador de rede que você deseja usar para o tráfego de RAIO aos números de porta existente. Por exemplo, se você quiser usar o endereço IP 192.168.1.2 e portas RADIUS 1812 e 1645 para solicitações de autenticação, altere a configuração de porta de **1812,1645** para **192.168.1.2: 1812,1645**. Se seu RAIO portas autenticação e RADIUS contabilidade UDP são diferentes dos valores padrão, alterar as configurações de porta adequadamente.

4. Para usar várias configurações de porta para autenticação ou solicitações de estatísticas, separe os números de porta com vírgulas.

Para obter mais informações sobre portas de NPS UDP, consulte [configurar informações da porta UDP NPS](nps-udp-ports-configure.md)


Para obter mais informações sobre o NPS, consulte [servidor de políticas de rede](nps-top.md)

