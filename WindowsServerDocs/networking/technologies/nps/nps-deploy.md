---
title: Implantar o servidor de políticas de rede
description: Este tópico fornece links para conteúdo de implantação do servidor de políticas de rede para Windows Server 2016 e inclui links para orientação adicional sobre NPS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: daf62e2a593014e16bcb5fd16542b2e9c75b9c1d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-network-policy-server"></a>Implantar o servidor de políticas de rede

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para obter informações sobre a implantação do servidor de políticas de rede.

>[!NOTE]
>Para obter a documentação adicional do servidor de política de rede, você pode usar as seguintes seções de biblioteca.  
>- [Introdução ao servidor de políticas de rede](nps-getstart-top.md)
>- [Planeje o servidor de políticas de rede](nps-plan-top.md)
>- [Gerenciar o servidor de políticas de rede](nps-manage-top.md)

Guia de rede do Windows Server 2016 Core inclui uma seção sobre como planejar e instalar o servidor de política de rede \(NPS\) e as tecnologias apresentadas no guia servem como pré-requisitos para a implantação NPS em um domínio do Active Directory. Para obter mais informações, consulte a seção "Implantar NPS1" no Windows Server 2016 [guia da rede principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1).

## <a name="deploy-nps-server-certificates-for-vpn-and-8021x-access"></a>Implantação de certificados do servidor NPS para VPN e acesso 802.1 X

Se você deseja implantar métodos de autenticação como Extensible Authentication Protocol \(EAP\) e EAP protegido que exigem o uso de certificados de servidor no servidor NPS, você pode implantar certificados do servidor NPS com o guia [implantar certificados de servidor para 802.1 X com e sem fio implantações](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="deploy-nps-for-8021x-wireless-access"></a>Implantar o NPS para acesso sem fio 802.1 X

Para implantar o NPS para acesso sem fio, você pode usar o guia [baseada em senha implantar 802.1 X autenticado acesso sem fio](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).

## <a name="deploy-nps-for-windows-10-vpn-access"></a>Implantar o NPS para acesso VPN do Windows 10

Você pode usar o NPS para processar solicitações de conexão para conexões de \(VPN\) sempre em rede Virtual privada para funcionários remotos que estão usando computadores e dispositivos que executam o Windows 10.

Para obter mais informações, consulte o [remoto acesso sempre em VPN implantação guia para Windows Server 2016 e o Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

