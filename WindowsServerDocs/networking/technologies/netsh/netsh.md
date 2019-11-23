---
title: Shell de Rede (Netsh)
description: Este tópico fornece uma visão geral do utilitário de linha de comando do Shell de rede (netsh) no Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: ac440c8187424733c0636cf2013342458f08d2f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405543"
---
# <a name="network-shell-netsh"></a>\(do Shell de rede\) netsh

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

O Shell de rede (netsh) é um utilitário de linha de comando que permite configurar e exibir o status de várias funções e componentes do servidor de comunicações de rede depois que eles são instalados em computadores que executam o Windows Server 2016.

Algumas tecnologias de cliente, como o protocolo de configuração de host dinâmico \(DHCP\) Client e BranchCache, também fornecem comandos netsh que permitem configurar computadores cliente que executam o Windows 10.

Na maioria dos casos, os comandos netsh fornecem a mesma funcionalidade que está disponível quando você usa o console de gerenciamento Microsoft \(MMC\) snap\-no para cada função de servidor de rede ou recurso de rede. Por exemplo, você pode configurar o servidor de políticas de rede \(NPS\) usando o snap-in do MMC do NPS ou os comandos netsh no contexto **netsh nps** .

Além disso, há comandos netsh para tecnologias de rede, como para IPv6, ponte de rede e chamada de procedimento remoto \(\)RPC, que não estão disponíveis no Windows Server como um snap-in do MMC.

>[!IMPORTANT]
>É recomendável que você use o Windows PowerShell para gerenciar as tecnologias de rede no [Windows Server 2016 e no Windows 10](https://technet.microsoft.com/library/mt156917.aspx) em vez do Shell de rede. O Shell de rede é incluído para compatibilidade com seus scripts, no entanto, e seu uso tem suporte.

## <a name="network-shell-netsh-technical-reference"></a>Referência técnica do Shell de rede (netsh)

A referência técnica do netsh fornece uma referência de comando netsh abrangente, incluindo sintaxe, parâmetros e exemplos para comandos netsh. Você pode usar a referência técnica do netsh para criar scripts e arquivos em lotes usando comandos netsh para gerenciamento local ou remoto de tecnologias de rede em computadores que executam o Windows Server 2016 e o Windows 10.  
  
### <a name="content-availability"></a>Disponibilidade do conteúdo  
  
A referência técnica do Shell de rede está disponível para download na ajuda do Windows \(*. chm\) formato da galeria do TechNet: [referência técnica de netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  
---
