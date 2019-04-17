---
title: "Gerenciar o Backup e restauração do Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>Gerenciar o Backup e restauração do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Windows Server Essentials oferece maneiras confiáveis para realizar backups regulares do seu servidor e dos computadores da rede. No caso de perda de dados, você pode restaurar dados de um backup bem-sucedido no servidor sem restaurar todo o computador. Se necessário, você pode executar uma restauração completa do sistema ao seu servidor ou computadores cliente na rede. A tabela a seguir descreve as diferentes opções de backup disponíveis para você junto com suas vantagens.  
  
|Recurso de backup|Descrição|Vantagens|  
|--------------------|-----------------|----------------|  
|Backup do servidor|Backup de servidor que executa o Windows Server Essentials. É feito backup dos dados em uma unidade USB externa.<br /><br /> Para obter mais informações, consulte [gerenciar o Backup do servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md) e [restaurar ou reparar seu servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md).|-Pode restaurar arquivos e pastas em seu servidor.<br /><br /> -Pode executar a restauração do sistema completo do seu servidor.|  
|Backup do computador cliente|Backup de seus computadores cliente na rede. É feito backup dos dados no servidor que executa o Windows Server Essentials.<br /><br /> Para obter mais informações, consulte [gerenciar o Backup do cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md) e [restaurar um sistema completo de um backup do computador cliente existente](Restore-a-full-system-from-an-existing-client-computer-backup.md).|-Pode restaurar arquivos e pastas do seu servidor.<br /><br /> -Pode executar a restauração do sistema completo do computador cliente.|  
| Backup do Microsoft Azure|Realiza um backup online de arquivos ou pastas em seu servidor. Quando você usa o Backup do Azure para fazer backup dos dados do servidor, as informações são criptografadas usando sua senha antes que ela é carregada em um datacenter seguro na Internet.<br /><br /> Para obter mais informações, consulte [gerenciar Online Backup](Manage-Online-Backup-in-Windows-Server-Essentials.md).|-Pode restaurar arquivos e pastas do seu servidor.<br /><br /> -Com backups incrementais, apenas as alterações nos arquivos serão transferidas para a nuvem.<br /><br /> -Backups são armazenados no Microsoft Azure e são externo, reduzindo a necessidade de proteger e mídia de backup no local.|  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
