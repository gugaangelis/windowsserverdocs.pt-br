---
title: Configurar o NPS em um computador multihomed
description: Este tópico fornece instruções sobre como configurar um servidor com vários adaptadores de rede que está executando o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1292accf4d19afa7f6a050281b7af373b3c867c3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952081"
---
# <a name="configure-nps-on-a-multihomed-computer"></a>Configurar o NPS em um computador multihomed

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para configurar um NPS com vários adaptadores de rede.

Ao usar vários adaptadores de rede em um servidor que executa o NPS (servidor de diretivas de rede), você pode configurar o seguinte:

- Os adaptadores de rede que fazem e não enviam e recebem serviço RADIUS \( \) tráfego RADIUS.
- Em uma base de adaptador por rede, se o NPS monitora o tráfego RADIUS no protocolo IP versão 4 \( IPv4 \) , IPv6 ou IPv4 e IPv6.
- Os números de porta UDP sobre os quais o tráfego RADIUS é enviado e recebido por protocolo \( IPv4 ou IPv6 \) , por adaptador de rede.

Por padrão, o NPS escuta o tráfego RADIUS nas portas 1812, 1813, 1645 e 1646 para IPv6 e IPv4 para todos os adaptadores de rede instalados. Como o NPS usa automaticamente todos os adaptadores de rede para o tráfego RADIUS, você só precisa especificar os adaptadores de rede que deseja que o NPS use para o tráfego RADIUS quando desejar impedir que o NPS utilize um adaptador de rede específico.

>[!NOTE]
>Se você desinstalar o IPv4 ou IPv6 em um adaptador de rede, o NPS não monitorará o tráfego RADIUS para o protocolo desinstalado.

Em um NPS com vários adaptadores de rede instalados, talvez você queira configurar o NPS para enviar e receber o tráfego RADIUS somente nos adaptadores que você especificar.

Por exemplo, um adaptador de rede instalado no NPS pode levar a um segmento de rede que não contém clientes RADIUS, enquanto um segundo adaptador de rede fornece ao NPS um caminho de rede para seus clientes RADIUS configurados. Nesse cenário, é importante direcionar o NPS a usar o segundo adaptador de rede para todo o tráfego RADIUS.

Em outro exemplo, se o NPS tiver três adaptadores de rede instalados, mas você quiser que o NPS use dois dos adaptadores para o tráfego RADIUS, você poderá configurar informações de porta somente para os dois adaptadores. Ao excluir a configuração de porta para o terceiro adaptador, você impede que o NPS use o adaptador para o tráfego RADIUS.

## <a name="using-a-network-adapter"></a>Usando um adaptador de rede

Para configurar o NPS para escutar e enviar o tráfego RADIUS em um adaptador de rede, use a seguinte sintaxe na caixa de diálogo Propriedades do servidor de políticas de rede no console do NPS:

- Sintaxe de tráfego IPv4: IPAddress: UDPport, em que IPAddress é o endereço IPv4 configurado no adaptador de rede sobre o qual você deseja enviar o tráfego RADIUS e UDPport é o número da porta RADIUS que você deseja usar para autenticação RADIUS ou tráfego de contabilização.
- Sintaxe de tráfego IPv6: [IPv6Address]: UDPport, em que os colchetes em aproximadamente IPv6Address são necessários, IPv6Address é o endereço IPv6 configurado no adaptador de rede sobre o qual você deseja enviar o tráfego RADIUS, e UDPport é o número da porta RADIUS que você deseja usar para autenticação RADIUS ou tráfego de contabilização.

Os caracteres a seguir podem ser usados como delimitadores para configurar as informações de endereço IP e porta UDP:

- Delimitador de endereço/porta: dois-pontos (:)
- Delimitador de porta: vírgula (,)
- Delimitador de interface: ponto e vírgula (;)

## <a name="configuring-network-access-servers"></a>Configurando servidores de acesso à rede

Verifique se os servidores de acesso à rede estão configurados com os mesmos números de porta UDP RADIUS que você configura em seu NPSs. As portas UDP padrão RADIUS definidas nas RFCs 2865 e 2866 são 1812 para autenticação e 1813 para contabilidade; no entanto, alguns servidores de acesso são configurados por padrão para usar a porta UDP 1645 para solicitações de autenticação e a porta UDP 1646 para solicitações de contabilização.

>[!IMPORTANT]
>Se você não usar os números de porta RADIUS padrão, deverá configurar exceções no firewall para o computador local para permitir o tráfego RADIUS nas novas portas. Para obter mais informações, consulte [Configurar firewalls para tráfego RADIUS](nps-firewalls-configure.md).

## <a name="configure-the-multihomed-nps"></a>Configurar o NPS de hospedagem múltipla

Você pode usar o procedimento a seguir para configurar o NPS de hospedagem múltipla.

A associação no **Admins. do Domínio** ou equivalente é o requisito mínimo exigido para concluir este procedimento.

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>Para especificar o adaptador de rede e as portas UDP que o NPS usa para o tráfego RADIUS

1. No Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **servidor de políticas de rede** para abrir o console do NPS.

2. Clique com o botão direito do mouse em **servidor de políticas de rede**e clique em **Propriedades**.

3. Clique na guia **portas** e preceda o endereço IP do adaptador de rede que você deseja usar para o tráfego RADIUS para os números de porta existentes. Por exemplo, se você quiser usar o endereço IP 192.168.1.2 e as portas RADIUS 1812 e 1645 para solicitações de autenticação, altere a configuração de porta de **1812, 1645** para **192.168.1.2:1812, 1645**. Se a autenticação RADIUS e as portas UDP de contabilização RADIUS forem diferentes dos valores padrão, altere as configurações de porta adequadamente.

4. Para usar várias configurações de porta para solicitações de autenticação ou de estatísticas, separe os números de porta com vírgulas.

Para obter mais informações sobre as portas UDP do NPS, consulte [configurar informações de porta do NPS UDP](nps-udp-ports-configure.md)


Para obter mais informações sobre o NPS, consulte [servidor de políticas de rede](nps-top.md)

