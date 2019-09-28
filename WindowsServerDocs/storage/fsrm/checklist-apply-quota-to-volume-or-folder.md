---
title: Aplicar uma cota a um volume ou uma pasta
description: Este artigo descreve como aplicar uma cota de armazenamento a um volume ou uma pasta
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 44eea3c3802e261239db9bf8350777e1b83fb9c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394325"
---
# <a name="checklist-apply-a-quota-to-a-volume-or-folder"></a>Lista de verificação: Aplicar uma cota a um volume ou pasta

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server (canal semestral)

1. Defina configurações de email se você pretende enviar notificações de limite ou relatórios de armazenamento por email. [Configurar notificações por email](configure-email-notifications.md)

2. Avalie os requisitos de armazenamento no volume ou na pasta. Você pode usar os relatórios no nó **Gerenciamento de relatórios de armazenamento** para fornecer dados. (Por exemplo, execute um relatório arquivos por proprietário sob demanda para identificar os usuários que usam grandes quantidades de espaço em disco.) [Gerar relatórios sob demanda](generate-reports-on-demand.md)

3. Examine os modelos de cota pré-configurados disponíveis. (Em **Gerenciamento de cotas**, clique no nó **modelos de cota** .) [Editar propriedades do modelo de cota](edit-quota-template-properties.md) 
<br />-Or- <br /> Crie um novo modelo de cota para impor uma política de armazenamento em sua organização. [Criar um modelo de cota](create-quota-template.md)

4. Crie uma cota baseada no modelo no volume ou na pasta.  
 [Criar uma cota](create-quota.md) <br /> -Or- <br /> Crie uma cota de aplicação automática para gerar automaticamente cotas para todas as subpastas no volume ou na pasta. [Criar uma cota de aplicação automática](create-auto-apply-quota.md)

6. Agende uma tarefa de relatório que contém um relatório de Uso de cota para monitorar o uso de cota periodicamente. [Agendar um conjunto de relatórios](schedule-set-of-reports.md)

> [!Note]
> Se você quiser arquivos de tela em um volume ou pasta, consulte [Checklist: Aplique uma tela de arquivo a um volume ou pasta @ no__t-0.











