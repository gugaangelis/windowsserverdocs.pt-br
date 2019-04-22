---
title: Configurar limites de notificação
description: Este artigo descreve como adicionar os limites de tempo para vários tipos de notificação
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: dba5b3b3c8b651935ec3c69695583d04087b7f2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826307"
---
# <a name="configure-notification-limits"></a>Configurar limites de notificação

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Para reduzir o número de notificações acumuladas por exceder repetidamente um limite de cota ou tentar salvar um arquivo não autorizado, o Gerenciador de recursos do servidor de arquivos aplica os limites de tempo para os seguintes tipos de notificação:

-   Email
-   Log de eventos
-   Comando
-   Relatório

Cada limite especifica um período de tempo para que outra notificação configurada do mesmo tipo seja gerada para um problema idêntico.

Um limite de 60 minutos padrão é definido para cada tipo de notificação, mas você pode alterar esses limites. O limite aplica-se a todas as notificações de um determinado tipo, sejam elas geradas por limites de cota ou eventos de triagem de arquivo.

## <a name="to-specify-a-standard-notification-limit-for-each-notification-type"></a>Para especificar um limite de notificação padrão para cada tipo de notificação

1.  Na árvore de console, clique com o botão direito do mouse em **Gerenciador de Recursos de Servidor de Arquivos** e em **Configurar Opções**. A caixa de diálogo **Opções do Gerenciador de Recursos de Servidor de Arquivos** é aberta.

2.  Na guia **Limites de notificação**, insira um valor em minutos para cada tipo de notificação exibido.

3.  Clique em **OK**.

> [!Note]
> Para personalizar limites de tempo que são associados com notificações de uma cota ou triagem de arquivo específico, você pode usar as ferramentas de linha de comando do Gerenciador de Recursos de Servidor de Arquivos **Dirquota.exe** e **Filescrn.exe**, ou uso o cmdlets do [Gerenciador de Recursos de Servidor de Arquivos](https://technet.microsoft.com/itpro/powershell/windows/fileserverresourcemanager/fileserverresourcemanager).

## <a name="see-also"></a>Consulte também

-   [Opções do Gerenciador de recursos de servidor de arquivos de configuração](setting-file-server-resource-manager-options.md)
-   [Ferramentas de linha de comando](command-line-tools.md)