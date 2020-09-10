---
title: Gerenciar Backup e restauração no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 41000915-f6ff-4dbb-b7be-629ef36386d4
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 996eb1293eebbd424c6063654322617a92043eae
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623222"
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>Gerenciar Backup e restauração no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Windows Server Essentials fornece maneiras confiáveis de realizar backups regulares do seu servidor e dos computadores da rede. Em caso de perda de dados, você pode restaurar dados de um backup realizaod com sucesso no servidor sem restaurar todo o computador. Se necessário, você pode executar uma restauração completa do sistema para o servidor ou os computadores cliente na rede. A tabela a seguir descreve as diferentes opções de backup disponíveis para você, juntamente com suas vantagens.

|Recurso de backup|Descrição|Vantagens|
|--------------------|-----------------|----------------|
|Backup do servidor|Faz backup do servidor que executa o Windows Server Essentials. Os backup dos dados para uma unidade USB externa.<br /><br /> Para obter mais informações, consulte [gerenciar o backup do servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md) e [restaurar ou reparar o servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md).|-Pode restaurar arquivos e pastas no servidor.<br /><br /> -Pode executar a restauração completa do sistema do seu servidor.|
|Backup do Computador Cliente|Faz backup dos seus computadores cliente na rede. Os dados de backup são armazenados no servidor que executa o Windows Server Essentials.<br /><br /> Para obter mais informações, consulte [gerenciar o backup do cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md) e [restaurar um sistema completo de um backup de computador cliente existente](Restore-a-full-system-from-an-existing-client-computer-backup.md).|-Pode restaurar arquivos e pastas do seu servidor.<br /><br /> -Pode executar a restauração completa do sistema do seu computador cliente.|
| Backup do Microsoft Azure|Executa um backup online de arquivos ou pastas no servidor. Quando você usa o backup do Azure para fazer backup de dados do servidor, as informações são criptografadas usando sua frase secreta antes de serem carregadas em um datacenter seguro na Internet.<br /><br /> Para obter mais informações, consulte [gerenciar o backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md).|-Pode restaurar arquivos e pastas do seu servidor.<br /><br /> -Com backups incrementais, somente as alterações nos arquivos são transferidas para a nuvem.<br /><br /> -Os backups são armazenados em Microsoft Azure e são externos, reduzindo a necessidade de proteger a mídia de backup no local.|

## <a name="additional-references"></a>Referências adicionais

-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)

-   [Utilizar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
