---
title: Configurar informações de porta UDP para NPS
description: Você pode usar este tópico para configurar as portas que o servidor de política de rede (NPS) usa para autenticação remota Authentication Dial-In User Service (RADIUS) e tráfego de contabilidade no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f0e703dc6f9083f1e79091a6cee6d1ac58753d12
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-nps-udp-port-information"></a>Configurar informações de porta UDP para NPS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar o procedimento a seguir para configurar as portas que o servidor de política de rede (NPS) usa para autenticação Remote Authentication Dial-In User Service \(RADIUS\) e tráfego de contabilidade.

Por padrão, o NPS escuta tráfego RADIUS nas portas 1812, 1813, 1645 e 1646 para o protocolo IP versão 6 \(IPv6\) e IPv4 para todos os adaptadores de rede instalados.

>[!NOTE]
>Se você desinstalar o IPv4 ou IPv6 em um adaptador de rede, NPS não monitora o tráfego de RAIO para o protocolo desinstalado.

Os valores de porta 1812 para autenticação e 1813 para estatísticas são portas padrão RADIUS definidas pelo \(IETF\) a Internet Engineering Task Force no RFCs 2865 e 2866. No entanto, por padrão, muitos servidores de acesso usam portas 1645 para solicitações de autenticação e 1646 para solicitações de contabilidade. Não importa quais números de porta que você decidir usar, certifique-se de que o NPS e o servidor de acesso estão configurados para usar as mesmas.

>[IMPORTANTE] Se você não usar os números de porta padrão RADIUS, você deve configurar exceções no firewall para o computador local permitir o tráfego RADIUS nas novas portas. Para obter mais informações, consulte [configurar Firewalls para o tráfego de RAIO](nps-firewalls-configure.md).

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para concluir este procedimento.

## <a name="to-configure-nps-udp-port-information"></a>Para configurar as informações de porta UDP de NPS 

1. Abra o console NPS.
2. Clique com botão direito **NPS**e clique em **propriedades**.
3. Clique no **portas** guia e, em seguida, examine as configurações das portas. Se a autenticação RADIUS e portas de UDP estatísticas RADIUS variam dos valores padrão fornecidos (1812 e 1645 para autenticação e 1813 e 1646 para estatísticas), digite suas configurações de porta no **autenticação** e **estatísticas**.
4. Para usar várias configurações de porta para autenticação ou solicitações de estatísticas, separe os números de porta com vírgulas.

Para obter mais informações sobre o gerenciamento de NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).
