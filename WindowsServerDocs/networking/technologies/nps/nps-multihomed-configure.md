---
title: Configurar o NPS em um computador multihomed
description: Este tópico fornece instruções sobre como configurar um servidor com vários adaptadores de rede que está executando o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 55eccf3afc649e84c5b6f5ce7932ed97617ddca9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856797"
---
# <a name="configure-nps-on-a-multihomed-computer"></a>Configurar o NPS em um computador multihomed

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para configurar um NPS com vários adaptadores de rede.

Quando você usa vários adaptadores de rede em um servidor que executa o servidor de diretivas de rede (NPS), você pode configurar o seguinte:

- Os adaptadores de rede que não e enviar e receber Remote Authentication Dial-In User Service \(RADIUS\) tráfego.
- Em uma base de adaptador por rede, se o NPS monitora o tráfego RADIUS em protocolo IP versão 4 \(IPv4\), IPv6, ou IPv4 e IPv6.
- Os números de porta UDP por quais RADIUS o tráfego é enviado e recebido por protocolo \(IPv4 ou IPv6\), base de adaptador por rede.

Por padrão, o NPS escuta tráfego RADIUS nas portas 1812, 1813, 1645 e 1646 para IPv6 e IPv4 para todos os adaptadores de rede instalados. Como o NPS usa automaticamente todos os adaptadores de rede para o tráfego RADIUS, você só precisará especificar os adaptadores de rede que o NPS a ser usado para RADIUS quando você deseja impedir que o NPS usando um adaptador de rede específico de tráfego.

>[!NOTE]
>Se você desinstalar o IPv4 ou IPv6 em um adaptador de rede, o NPS não monitora o tráfego RADIUS para o protocolo desinstalado.

Em um NPS que tem vários adaptadores de rede instalados, você talvez queira configurar o NPS para enviar e receber o tráfego RADIUS somente em adaptadores que você especificar.

Por exemplo, um adaptador de rede instalado no NPS pode levar a um segmento de rede que não contém clientes RADIUS, enquanto um segundo adaptador de rede fornece o NPS com um caminho de rede para os clientes RADIUS configurados. Nesse cenário, é importante direcionar o NPS para usar o segundo adaptador de rede para todo o tráfego RADIUS.

Em outro exemplo, se o NPS tem três adaptadores de rede instalados, mas você só deseja NPS usar dois adaptadores de tráfego RADIUS, você pode configurar informações de porta para os dois adaptadores. Excluindo configuração de porta para o terceiro adaptador, você impede que o NPS usando o adaptador para o tráfego RADIUS.

## <a name="using-a-network-adapter"></a>Usando um adaptador de rede

Para configurar o NPS para escutar e enviar tráfego RADIUS em um adaptador de rede, use a sintaxe a seguir na caixa de diálogo Propriedades do servidor de políticas de rede no console do NPS:

- Sintaxe de tráfego IPv4: IPAddress: UDPport, onde Endereço_ip é o endereço IPv4 configurado no adaptador de rede pela qual você deseja enviar tráfego RADIUS e UDPport é o número da porta RADIUS que você deseja usar para autenticação RADIUS ou contabilização do tráfego.
- Sintaxe de tráfego IPv6: [IPv6Address]: UDPport, onde os IPv6Address entre colchetes são necessários, IPv6Address é o endereço de IPv6 configurada no adaptador de rede pela qual você deseja enviar tráfego RADIUS e UDPport é o número da porta RADIUS que você deseja usar para autenticação RADIUS ou tráfego de contabilidade.

Os seguintes caracteres podem ser usados como delimitadores para configurar o endereço IP e informações da porta UDP:

- Delimitador de endereço/porta: dois-pontos (:)
- Delimitador de porta: vírgula (,)
- Interface delimitador: ponto e vírgula (;)

## <a name="configuring-network-access-servers"></a>Configurando servidores de acesso de rede

Certifique-se de que seus servidores de acesso de rede são configurados com os mesmos números de porta UDP RADIUS que você configura no seu NPSs. As portas UDP RADIUS padrão definidas nos RFCs 2865 e 2866 são 1812 para autenticação e 1813 para contabilização; No entanto, alguns servidores de acesso são configuradas por padrão para usar a porta UDP 1645 para solicitações de autenticação e a porta UDP 1646 para solicitações de estatísticas.

>[!IMPORTANT]
>Se você não usar números de porta padrão RADIUS, você deve configurar exceções no firewall para o computador local permitir o tráfego RADIUS nas novas portas. Para obter mais informações, consulte [configurar Firewalls para o tráfego RADIUS](nps-firewalls-configure.md).

## <a name="configure-the-multihomed-nps"></a>Configurar o NPS multihomed

Você pode usar o procedimento a seguir para configurar a NPS hospedagem múltipla.

Ser membro do grupo **Admins. do Domínio**, ou equivalente, é o mínimo necessário para concluir este procedimento.

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>Para especificar o adaptador de rede e as portas UDP que o NPS usa para o tráfego RADIUS

1. No Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Network Policy Server** para abrir o console do NPS.

2. Clique com botão direito **servidor de políticas de rede**e, em seguida, clique em **propriedades**.

3. Clique o **portas** guia e, em seguida, preceda o endereço IP para o adaptador de rede que você deseja usar para o tráfego RADIUS para os números de porta existente. Por exemplo, se você quiser usar o endereço IP 192.168.1.2 e portas RADIUS 1812 e 1645 para solicitações de autenticação, alterar a configuração da porta do **1812,1645** à **192.168.1.2: 1812,1645**. Se sua autenticação RADIUS e as portas UDP de contabilização RADIUS são diferentes dos valores padrão, altere as configurações de porta adequadamente.

4. Para usar várias configurações de portas para autenticação ou as solicitações de contabilidade, separe os números de porta com vírgulas.

Para obter mais informações sobre as portas UDP do NPS, consulte [configurar informações da porta UDP NPS](nps-udp-ports-configure.md)


Para obter mais informações sobre o NPS, consulte [servidor de políticas de rede](nps-top.md)

