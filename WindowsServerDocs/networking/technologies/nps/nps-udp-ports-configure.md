---
title: Configurar informações da porta UDP do NPS
description: Você pode usar este tópico para configurar as portas que usa o servidor de diretivas de rede (NPS) para autenticação de RADIUS Remote Authentication Dial-In usuário Service () e tráfego de contabilidade no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 44c20092180c47e97f1505271203f4491606bbcf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851967"
---
# <a name="configure-nps-udp-port-information"></a>Configurar informações da porta UDP do NPS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar o procedimento a seguir para configurar as portas que o servidor de diretivas de rede (NPS) usa para Remote Authentication Dial-In User Service \(RADIUS\) tráfego de contabilização e autenticação.

Por padrão, o NPS escuta o tráfego RADIUS nas portas 1812, 1813, 1645 e 1646 para ambos os protocolo IP versão 6 \(IPv6\) e IPv4 para todos os adaptadores de rede instalados.

>[!NOTE]
>Se você desinstalar o IPv4 ou IPv6 em um adaptador de rede, o NPS não monitora o tráfego RADIUS para o protocolo desinstalado.

Os valores de porta 1812 para autenticação e 1813 para estatísticas são portas padrão RADIUS definidas pela Internet Engineering Task Force \(IETF\) nas RFCs 2865 e 2866. No entanto, por padrão, muitos servidores de acesso usam portas 1645 para solicitações de autenticação e 1646 para solicitações de estatísticas. Não importa quais números de porta que você decida usar, certifique-se de que o NPS e o servidor de acesso são configurados para usar os mesmos.

>[IMPORTANTE] Se você não usar números de porta padrão RADIUS, você deve configurar exceções no firewall para o computador local permitir o tráfego RADIUS nas novas portas. Para obter mais informações, consulte [configurar Firewalls para o tráfego RADIUS](nps-firewalls-configure.md).

Ser membro do grupo **Admins. do Domínio**, ou equivalente, é o mínimo necessário para concluir este procedimento.

## <a name="to-configure-nps-udp-port-information"></a>Para configurar as informações da porta UDP de NPS 

1. Abra o console do NPS.
2. Clique com botão direito **servidor de políticas de rede**e, em seguida, clique em **propriedades**.
3. Clique o **portas** guia e, em seguida, examine as configurações das portas. Se sua autenticação RADIUS e as portas UDP de contabilização RADIUS variam de valores padrão fornecidos (1812 e 1645 para autenticação e 1813 e 1646 para estatísticas), digite as configurações da porta no **autenticação** e  **Contabilidade**.
4. Para usar várias configurações de portas para autenticação ou as solicitações de contabilidade, separe os números de porta com vírgulas.

Para obter mais informações sobre como gerenciar o NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
