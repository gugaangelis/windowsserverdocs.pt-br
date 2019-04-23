---
title: Aplicar uma cota a um volume ou uma pasta
description: Este artigo descreve como aplicar uma cota de armazenamento a um volume ou uma pasta
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 62910af666fb16db5c2e7a30b49eedfa8c12cacb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860847"
---
# <a name="checklist-apply-a-quota-to-a-volume-or-folder"></a>Lista de verificação: Aplique uma cota para um volume ou pasta

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

1. Defina configurações de email se você pretende enviar notificações de limite ou relatórios de armazenamento por email. [Configurar notificações por email](configure-email-notifications.md)

2. Avalie os requisitos de armazenamento no volume ou na pasta. Você pode usar os relatórios no nó **Gerenciamento de relatórios de armazenamento** para fornecer dados. (Por exemplo, executar um arquivos por relatório de proprietário sob demanda para identificar os usuários que usam grandes quantidades de espaço em disco.) [Gerar relatórios sob demanda](generate-reports-on-demand.md)

3. Examine os modelos de cota pré-configurados disponíveis. (No **gerenciamento de cotas**, clique no **modelos de cota** nó.) [Editar propriedades do modelo de cota](edit-quota-template-properties.md) 
<br />-Or- <br /> Crie um novo modelo de cota para impor uma política de armazenamento em sua organização. [Criar um modelo de cota](create-quota-template.md)

4. Crie uma cota baseada no modelo no volume ou na pasta.  
 [Criar uma cota](create-quota.md) <br /> -Or- <br /> Crie uma cota de aplicação automática para gerar automaticamente cotas para todas as subpastas no volume ou na pasta. [Crie uma cota de aplicação](create-auto-apply-quota.md)

6. Agende uma tarefa de relatório que contém um relatório de Uso de cota para monitorar o uso de cota periodicamente. [Agendar um conjunto de relatórios](schedule-set-of-reports.md)

> [!Note]
> Se você quiser triagem de arquivos em um volume ou pasta, consulte [lista de verificação: Aplicar uma triagem de arquivo para um Volume ou pasta](checklist-apply-file-screen-to-volume-or-folder.md).











