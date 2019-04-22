---
title: Implantar o Servidor de Políticas de Rede
description: Este tópico fornece links para conteúdo de implantação do servidor de políticas de rede do Windows Server 2016 e inclui links para diretrizes adicionais sobre o NPS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8da8951a9c6ed5022c892bbf01b33614d38abc5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814837"
---
# <a name="deploy-network-policy-server"></a>Implantar o Servidor de Políticas de Rede

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para obter informações sobre como implantar o servidor de políticas de rede.

>[!NOTE]
>Para obter documentação adicional do servidor de políticas de rede, você pode usar as seguintes seções da biblioteca.  
>- [Introdução ao servidor de políticas de rede](nps-getstart-top.md)
>- [Planejar o servidor de políticas de rede](nps-plan-top.md)
>- [Gerenciar o servidor de políticas de rede](nps-manage-top.md)

Guia de rede do Windows Server 2016 Core inclui uma seção sobre como planejar e instalar o servidor de políticas de rede \(NPS\), e as tecnologias apresentadas no guia servem como pré-requisitos para implantar o NPS em um domínio do Active Directory. Para obter mais informações, consulte a seção "Implantar o NPS1" do Windows Server 2016 [guia da rede principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1).

## <a name="deploy-nps-certificates-for-vpn-and-8021x-access"></a>Implantar certificados NPS para acesso 802.1X e VPN

Se você quiser implantar métodos de autenticação como o protocolo de autenticação extensível \(EAP\) e EAP protegido que exigem o uso do servidor de certificados no seu NPS, você pode implantar certificados NPS com o guia [ Implantar certificados de servidor para 802.1 X com fio e sem fio implantações](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="deploy-nps-for-8021x-wireless-access"></a>Implantar o NPS para acesso sem fio 802.1 X

Para implantar o NPS para acesso sem fio, você pode usar o guia [baseado em senha implantar acesso autenticado 802.1 X sem fio](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).

## <a name="deploy-nps-for-windows-10-vpn-access"></a>Implantar o NPS para acesso VPN do Windows 10

Você pode usar o NPS para processar solicitações de conexão para sempre na rede Virtual privada \(VPN\) conexões para funcionários remotos que estão usando computadores e dispositivos que executam o Windows 10.

Para obter mais informações, consulte o [remotos acesso sempre em VPN implantação guia para o Windows Server 2016 e Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

