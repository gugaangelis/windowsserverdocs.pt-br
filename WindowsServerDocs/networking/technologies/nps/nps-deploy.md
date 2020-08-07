---
title: Implantar o Servidor de Políticas de Rede
description: Este tópico fornece links para o conteúdo de implantação do servidor de políticas de rede para o Windows Server 2016 e inclui links para diretrizes adicionais sobre o NPS.
manager: brianlic
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b4f80d18b2facba7bb706bb566aed698c2a85c8d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87937407"
---
# <a name="deploy-network-policy-server"></a>Implantar o Servidor de Políticas de Rede

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para obter informações sobre como implantar o servidor de políticas de rede.

>[!NOTE]
>Para obter a documentação adicional do servidor de políticas de rede, você pode usar as seções de biblioteca a seguir.
>- [Introdução ao Servidor de Políticas de Rede](nps-getstart-top.md)
>- [Planejar Servidor de Políticas de Rede](nps-plan-top.md)
>- [Gerenciar o Servidor de Políticas de Rede](nps-manage-top.md)

O guia de rede do Windows Server 2016 Core inclui uma seção sobre como planejar e instalar o NPS do servidor de políticas de rede \( \) , e as tecnologias apresentadas no guia servem como pré-requisitos para implantar o NPS em um domínio Active Directory. Para obter mais informações, consulte a seção "implantar NPS1" no [Guia de rede](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1)do Windows Server 2016 Core.

## <a name="deploy-nps-certificates-for-vpn-and-8021x-access"></a>Implantar certificados NPS para acesso VPN e 802.1 X

Se você quiser implantar métodos de autenticação como EAP do protocolo de autenticação extensível \( \) e EAP protegido que exigem o uso de certificados de servidor em seu NPS, você pode implantar certificados NPS com o guia [implantar certificados de servidor para implantações com e sem fio 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="deploy-nps-for-8021x-wireless-access"></a>Implantar NPS para acesso sem fio 802.1 X

Para implantar o NPS para acesso sem fio, você pode usar o guia [implantar o acesso sem fio autenticado baseado em senha 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).

## <a name="deploy-nps-for-windows-10-vpn-access"></a>Implantar o NPS para acesso à VPN do Windows 10

Você pode usar o NPS para processar solicitações de conexão para Always On \( conexões VPN de rede privada virtual \) para funcionários remotos que estão usando computadores e dispositivos que executam o Windows 10.

Para obter mais informações, consulte o [Guia de implantação de VPN Always on acesso remoto para Windows Server 2016 e Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

