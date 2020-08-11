---
title: Shell de Rede (Netsh)
description: Este tópico fornece uma visão geral do utilitário de linha de comando do netsh (Shell de rede) no Windows Server 2016.
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: 1f331a7ea987f413f814dcb1b0e4173714b1153b
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994615"
---
# <a name="network-shell-netsh"></a>Netsh \(Shell de Rede\)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

O netsh (shell de rede) é um utilitário de linha de comando que permite configurar e exibir o status de vários componentes e funções de servidor de comunicações de rede depois que são instalados nos computadores que executam o Windows Server 2016.

Algumas tecnologias de clientes, como o cliente \(DHCP\) e o BranchCache, também fornecem comandos netsh que permitem configurar computadores cliente que executam o Windows 10.

Na maioria dos casos, os comandos netsh fornecem a mesma funcionalidade disponível quando você usa o snap\-in do MMC \(Console de Gerenciamento Microsoft\) para cada função de servidor de rede ou recurso de rede. Por exemplo, você pode configurar o NPS \(Servidor de Políticas de Rede\) usando o snap-in do MMC do NPS ou os comandos netsh no contexto do **netsh nps**.

Além disso, há comandos netsh para tecnologias de rede, como IPv6, ponte de rede e RPC \(Chamada de Procedimento Remoto\), que não estão disponíveis no Windows Server como um snap-in do MMC.

>[!IMPORTANT]
>É recomendável que você use o Windows PowerShell para gerenciar as tecnologias de rede no [Windows Server 2016 e Windows 10](/powershell/windows/get-started?view=win10-ps) em vez de no Shell de Rede. No entanto, o Shell de Rede é incluído para oferecer compatibilidade com seus scripts e há suporte para o uso dele.

## <a name="network-shell-netsh-technical-reference"></a>Referência Técnica do Netsh (Shell de Rede)

A Referência Técnica do Netsh fornece uma referência de comando netsh abrangente, incluindo sintaxe, parâmetros e exemplos para comandos netsh. Você pode usar a Referência Técnica do Netsh para criar scripts e arquivos em lotes usando comandos netsh para gerenciamento local ou remoto de tecnologias de rede em computadores que executam o Windows Server 2016 e o Windows 10.

### <a name="content-availability"></a>Disponibilidade do conteúdo

A Referência Técnica do Shell de Rede está disponível para download no formato \(*.chm\) da Ajuda do Windows na Galeria do TechNet: [Referência Técnica do Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)

---