---
title: Lista de verificação - Aplicar uma triagem de arquivo a um volume ou uma pasta
description: Este artigo descreve como aplicar uma triagem de arquivo a um volume ou uma pasta
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4d38e9a92a9c99663d81aab2a0164c5a30002f6a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402000"
---
# <a name="checklist---apply-a-file-screen-to-a-volume-or-folder"></a>Lista de verificação - Aplicar uma triagem de arquivo a um volume ou uma pasta

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Para aplicar uma triagem de arquivo a um volume ou uma pasta, use a listas seguir:
1. Defina configurações de email se planeja enviar notificações de triagem de arquivo ou relatórios de armazenamento por email seguindo as instruções a seguir em [Configurar notificações por email](configure-email-notifications.md).

2. Habilite a gravação de eventos de triagem de arquivo no banco de dados de auditoria se planeja gerar relatórios de Audição de triagem de arquivo.
[Configurar auditoria da triagem de arquivo](configure-file-screen-audit.md)

3. Avalie os tipos de arquivo armazenados que são candidatos para regras de triagem. Você pode usar os relatórios no nó **Gerenciamento de relatórios de armazenamento** para fornecer dados. (Por exemplo, execute um relatório Arquivos por grupo de arquivos ou um relatório de Arquivos grandes em demanda para identificar arquivos que ocupam grande quantidade de espaço do disco.) [Gerar relatórios em demanda](generate-reports-on-demand.md) 

4. Revise os grupos de arquivos pré-configurados ou crie um novo grupo de arquivos para impor uma política de triagem específica em sua organização. [Definir grupos de arquivos para triagem](define-file-groups-for-screening.md)  

5. Examine as propriedades dos modelos de triagem de arquivo disponíveis. (Em **Gerenciamento de triagem de arquivo**, clique no nó **modelos de tela de arquivo** .) [Editar propriedades do modelo de tela de arquivo](edit-file-screen-template-properties.md) 
<br />
 \- Ou -
 <br /> Crie um novo modelo de triagem de arquivo para impor uma política de armazenamento em sua organização.  [Criar um modelo de triagem de arquivo](create-file-screen-template.md) 

6. Crie uma triagem de arquivo baseada no modelo no volume ou na pasta. 
 [Criar uma triagem de arquivo](create-file-screen.md)
 
7. Configure exceções de triagem de arquivo em subpastas do volume ou da pasta. [Criar uma exceção de triagem de arquivo](create-file-screen-exception.md) 

8. Agende uma tarefa de relatório que contém um relatório de auditoria de triagem de arquivo para monitorar a atividade de triagem periodicamente.
  [Agendar um conjunto de relatórios](schedule-set-of-reports.md)


> [!NOTE]
> Para limitar o armazenamento em um volume ou uma pasta, consulte [Lista de verificação: aplicar uma cota a um volume ou pasta](checklist-apply-file-screen-to-volume-or-folder.md)