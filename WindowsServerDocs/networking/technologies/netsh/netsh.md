---
title: Shell de rede (Netsh)
description: Este tópico fornece uma visão geral do utilitário de linha de comando do Shell de rede (netsh) no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0a23d60bc3f181fee62ade105e546bbb7161c133
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="network-shell-netsh"></a>Rede Shell \(Netsh\)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Shell de rede (netsh) é um utilitário de linha de comando que permite que você configure e exibir o status de várias funções de servidor de comunicações de rede e componentes depois que eles são instalados em computadores que executam o Windows Server 2016.

>[!NOTE]
>Além de neste tópico, o seguinte conteúdo de Shell de rede está disponível.
>
> - [Sintaxe do comando netsh, contextos e formatação](netsh-contexts.md)
> - [Arquivo de lote de exemplo de Shell (Netsh) de rede](netsh-wins.md)
> - [Referência técnica do Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc) 

Algumas tecnologias de cliente, como o cliente Dynamic Host Configuration Protocol \(DHCP\) e BranchCache, também fornecem comandos netsh que permitem que você configurar os computadores cliente que executam o Windows 10.

Na maioria dos casos, os comandos netsh fornecem a mesma funcionalidade que está disponível quando você usar o Console de gerenciamento Microsoft \(MMC\) snap\-para cada recurso de rede ou a função de servidor rede. Por exemplo, você pode configurar o servidor de política de rede \(NPS\) usando o snap-in NPS MMC ou os comandos netsh no **netsh nps** contexto.

Além disso, há comandos netsh para tecnologias de rede, como para IPv6, ponte de rede e \(RPC\) chamada de procedimento remoto, que não estão disponíveis no Windows Server como um snap-in do MMC.

>[!IMPORTANT]
>É recomendável que você use o Windows PowerShell para gerenciar as tecnologias de rede em [Windows Server 2016 e o Windows 10](https://technet.microsoft.com/library/mt156917.aspx) em vez de Shell de rede. No entanto, o Shell de rede está incluída para compatibilidade com seus scripts, e seu uso é suportado.

## <a name="network-shell-netsh-technical-reference"></a>Referência técnica de Shell (Netsh) de rede

A referência técnica do Netsh fornece uma referência de comando netsh abrangente, incluindo sintaxe, parâmetros e exemplos de comandos netsh. Você pode usar a referência técnica do Netsh para criar scripts e arquivos em lotes usando comandos netsh para gerenciamento local ou remoto de tecnologias de rede em computadores que executam o Windows Server 2016 e o Windows 10.  
  
### <a name="content-availability"></a>Disponibilidade de conteúdo  
  
A referência técnica de Shell de rede está disponível para download no formato de \(*.chm\) ajuda do Windows da Galeria do TechNet: [referência técnica do Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  

