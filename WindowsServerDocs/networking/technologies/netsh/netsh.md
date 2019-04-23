---
title: Shell de Rede (Netsh)
description: Este tópico fornece uma visão geral do utilitário de linha de comando Network Shell (netsh) no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 038b21783ef1d27619657ec3f9a472cf3caba68e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849357"
---
# <a name="network-shell-netsh"></a>Shell de rede \(Netsh\)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Netsh (Network shell) é um utilitário de linha de comando que lhe permite configurar e exibir o status de vários componentes e funções de servidor de comunicações de rede depois que eles são instalados em computadores que executam o Windows Server 2016.

Algumas tecnologias de cliente, como Dynamic Host Configuration Protocol \(DHCP\) cliente e o BranchCache, também fornecem comandos netsh que permitem que você configure computadores cliente que executam o Windows 10.

Na maioria dos casos, os comandos netsh fornecem a mesma funcionalidade que está disponível quando você usar o Console de gerenciamento Microsoft \(MMC\) encaixar\-para cada rede recurso de rede ou a função de servidor. Por exemplo, você pode configurar o servidor de políticas de rede \(NPS\) usando o snap-in NPS MMC ou os comandos netsh na **netsh nps** contexto.

Além disso, há comandos netsh para tecnologias de rede, como para IPv6, a ponte de rede e a chamada de procedimento remoto \(RPC\), que não estão disponíveis no Windows Server como um snap-in do MMC.

>[!IMPORTANT]
>É recomendável que você usa o Windows PowerShell para gerenciar tecnologias de rede no [Windows Server 2016 e Windows 10](https://technet.microsoft.com/library/mt156917.aspx) em vez de Shell de rede. No entanto, o Shell de rede é incluído para compatibilidade com seus scripts, e seu uso é suportado.

## <a name="network-shell-netsh-technical-reference"></a>Referência técnica do Network Shell (Netsh)

Referência técnica do Netsh fornece uma referência de comando netsh abrangentes, incluindo sintaxe, parâmetros e exemplos de comandos do netsh. Você pode usar a referência técnica do Netsh para criar scripts e arquivos em lotes usando comandos netsh para o gerenciamento local ou remoto de tecnologias de rede em computadores que executam o Windows Server 2016 e Windows 10.  
  
### <a name="content-availability"></a>Disponibilidade do conteúdo  
  
A referência técnica do Shell de rede está disponível para download na Ajuda do Windows \(chm\) formato da Galeria do TechNet: [Referência técnica do Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  
---
