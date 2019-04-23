---
title: Gerenciar Backup e restauração no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41000915-f6ff-4dbb-b7be-629ef36386d4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6f6f0d27472664cd1cc538897d3d525fad506282
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828517"
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>Gerenciar Backup e restauração no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Windows Server Essentials fornece maneiras confiáveis de realizar backups regulares do seu servidor e dos computadores da rede. Em caso de perda de dados, você pode restaurar dados de um backup realizaod com sucesso no servidor sem restaurar todo o computador. Se necessário, você pode executar uma restauração completa do sistema para o servidor ou os computadores cliente na rede. A tabela a seguir descreve as diferentes opções de backup disponíveis para você, juntamente com suas vantagens.  
  
|Recurso de backup|Descrição|Vantagens|  
|--------------------|-----------------|----------------|  
|Backup do servidor|Faz backup do servidor que executa o Windows Server Essentials. Os backup dos dados para uma unidade USB externa.<br /><br /> Para obter mais informações, consulte [Manage Server Backup](Manage-Server-Backup-in-Windows-Server-Essentials.md) e [restaurar ou reparar seu servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md).|-Pode restaurar arquivos e pastas no servidor.<br /><br /> -Pode executar a restauração completa do sistema do seu servidor.|  
|Backup do Computador Cliente|Faz backup dos seus computadores cliente na rede. Os dados de backup são armazenados no servidor que executa o Windows Server Essentials.<br /><br /> Para obter mais informações, consulte [gerenciar o Backup do cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md) e [restaurar um sistema completo de um backup do computador cliente existente](Restore-a-full-system-from-an-existing-client-computer-backup.md).|-Pode restaurar arquivos e pastas do seu servidor.<br /><br /> -Pode executar uma restauração completa do sistema do computador cliente.|  
| Backup do Microsoft Azure|Executa um backup online de arquivos ou pastas no servidor. Quando você usa o Backup do Azure para fazer backup de dados do servidor, as informações são criptografadas usando uma senha antes de serem carregado em um datacenter seguro na Internet.<br /><br /> Para obter mais informações, consulte [Gerenciar Backup Online](Manage-Online-Backup-in-Windows-Server-Essentials.md).|-Pode restaurar arquivos e pastas do seu servidor.<br /><br /> -Com os backups incrementais, somente as alterações nos arquivos são transferidas para a nuvem.<br /><br /> -Backups são armazenados no Microsoft Azure e estão fora do local, reduzindo a necessidade de proteção e mídia de backup no local.|  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
