---
title: Configurar informações da porta UDP do NPS
description: Você pode usar este tópico para configurar as portas que o servidor de diretivas de rede (NPS) usa para autenticação de serviço RADIUS (RADIUS) e o tráfego de contabilização no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f874469666ab9b68d9eb970cf7fcb6a89ef27f0c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315576"
---
# <a name="configure-nps-udp-port-information"></a>Configurar informações da porta UDP do NPS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar o procedimento a seguir para configurar as portas que o servidor de diretivas de rede (NPS) usa para o serviço RADIUS \(RADIUS\) autenticação e o tráfego de contabilização.

Por padrão, o NPS escuta o tráfego RADIUS nas portas 1812, 1813, 1645 e 1646 para o protocolo IP versão 6 \(IPv6\) e IPv4 para todos os adaptadores de rede instalados.

>[!NOTE]
>Se você desinstalar o IPv4 ou IPv6 em um adaptador de rede, o NPS não monitorará o tráfego RADIUS para o protocolo desinstalado.

Os valores de porta de 1812 para autenticação e 1813 para contabilidade são portas RADIUS padrão definidas pela Internet Engineering Task Force \(IETF\) nas RFCs 2865 e 2866. No entanto, por padrão, muitos servidores de acesso usam as portas 1645 para solicitações de autenticação e 1646 para solicitações de contabilização. Não importa quais números de porta você decidir usar, verifique se o NPS e o servidor de acesso estão configurados para usar os mesmos.

>FUNDAMENTAL Se você não usar os números de porta RADIUS padrão, deverá configurar exceções no firewall para o computador local para permitir o tráfego RADIUS nas novas portas. Para obter mais informações, consulte [Configurar firewalls para tráfego RADIUS](nps-firewalls-configure.md).

A associação no **Admins. do Domínio** ou equivalente é o requisito mínimo exigido para concluir este procedimento.

## <a name="to-configure-nps-udp-port-information"></a>Para configurar as informações de porta UDP do NPS 

1. Abra o console do NPS.
2. Clique com o botão direito do mouse em **servidor de políticas de rede**e clique em **Propriedades**.
3. Clique na guia **portas** e examine as configurações de portas. Se as portas UDP de autenticação RADIUS e contabilização RADIUS variarem dos valores padrão fornecidos (1812 e 1645 para autenticação e 1813 e 1646 para contabilização), digite as configurações de porta em **autenticação** e **contabilidade**.
4. Para usar várias configurações de porta para solicitações de autenticação ou de estatísticas, separe os números de porta com vírgulas.

Para obter mais informações sobre como gerenciar o NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
