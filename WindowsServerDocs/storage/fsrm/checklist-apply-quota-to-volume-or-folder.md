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
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="checklist-apply-a-quota-to-a-volume-or-folder"></a>Lista de verificação: aplicar uma cota a um volume ou uma pasta

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

1. Defina configurações de email se você pretende enviar notificações de limite ou relatórios de armazenamento por email. [Configurar notificações por email](configure-email-notifications.md)

2. Avalie os requisitos de armazenamento no volume ou na pasta. Você pode usar os relatórios no nó **Gerenciamento de relatórios de armazenamento** para fornecer dados. (Por exemplo, execute um relatório Arquivos por Proprietário em demanda para identificar os usuários que usam grandes quantidades de espaço em disco.) [Gerar relatórios sob demanda](generate-reports-on-demand.md)

3. Examine os modelos de cota pré-configurados disponíveis. (Em **Gerenciamento de Cota**, clique no nó **Modelos de Cota**.) [Editar propriedades de modelo de cota](edit-quota-template-properties.md) 
<br />- Ou - <br /> Crie um novo modelo de cota para impor uma política de armazenamento em sua organização. [Criar um modelo de cota](create-quota-template.md)

4. Crie uma cota baseada no modelo no volume ou na pasta.  
 [Crie uma cota](create-quota.md) <br /> - Ou - <br /> Crie uma cota de aplicação automática para gerar automaticamente cotas para todas as subpastas no volume ou na pasta. [Crie uma cota de aplicação automática](create-auto-apply-quota.md)

6. Agende uma tarefa de relatório que contém um relatório de Uso de cota para monitorar o uso de cota periodicamente. [Agende um conjunto de relatórios](schedule-set-of-reports.md)

> [!Note]
> Se você quiser fazer a triagem de arquivos em um volume ou pasta, consulte [Lista de verificação: aplicar uma triagem de arquivo para um volume ou pasta](checklist-apply-file-screen-to-volume-or-folder.md).











