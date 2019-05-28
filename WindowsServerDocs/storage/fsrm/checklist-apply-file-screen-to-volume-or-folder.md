---
title: Lista de verificação - Aplicar uma triagem de arquivo a um volume ou uma pasta
description: Este artigo descreve como aplicar uma triagem de arquivo a um volume ou uma pasta
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 03325d8a65d88c35f09985223608fc7474a0fde5
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475846"
---
# <a name="checklist---apply-a-file-screen-to-a-volume-or-folder"></a>Lista de verificação - Aplicar uma triagem de arquivo a um volume ou uma pasta

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Para aplicar uma triagem de arquivo a um volume ou uma pasta, use a listas seguir:
1. Defina configurações de email se planeja enviar notificações de triagem de arquivo ou relatórios de armazenamento por email seguindo as instruções a seguir em [Configurar notificações por email](configure-email-notifications.md).

2. Habilite a gravação de eventos de triagem de arquivo no banco de dados de auditoria se planeja gerar relatórios de Audição de triagem de arquivo.
[Configurar auditoria da triagem de arquivo](configure-file-screen-audit.md)

3. Avalie os tipos de arquivo armazenados que são candidatos para regras de triagem. Você pode usar os relatórios no nó **Gerenciamento de relatórios de armazenamento** para fornecer dados. (Por exemplo, executar um arquivos por relatório de grupo de arquivos ou um relatório de arquivos grandes sob demanda para identificar arquivos que ocupam grandes quantidades de espaço em disco.) [Gerar relatórios sob demanda](generate-reports-on-demand.md) 

4. Revise os grupos de arquivos pré-configurados ou crie um novo grupo de arquivos para impor uma política de triagem específica em sua organização. [Definir grupos de arquivos para triagem](define-file-groups-for-screening.md)  

5. Examine as propriedades dos modelos de triagem de arquivo disponíveis. (No **gerenciamento de triagem de arquivo**, clique no **modelos de tela de arquivo** nó.) [Editar propriedades do modelo de triagem de arquivo](edit-file-screen-template-properties.md) 
<br />
 -Or-
 <br /> Crie um novo modelo de triagem de arquivo para impor uma política de armazenamento em sua organização.  [Criar um modelo de triagem de arquivo](create-file-screen-template.md) 

6. Crie uma triagem de arquivo baseada no modelo no volume ou na pasta. 
 [Criar uma tela de arquivo](create-file-screen.md)
 
7. Configure exceções de triagem de arquivo em subpastas do volume ou da pasta. [Criar uma exceção de triagem de arquivo](create-file-screen-exception.md) 

8. Agende uma tarefa de relatório que contém um relatório de auditoria de triagem de arquivo para monitorar a atividade de triagem periodicamente.
  [Agendar um conjunto de relatórios](schedule-set-of-reports.md)


> [!NOTE]
> Para limitar o armazenamento em um volume ou pasta, consulte [lista de verificação: aplicar uma cota a um volume ou uma pasta](checklist-apply-file-screen-to-volume-or-folder.md)