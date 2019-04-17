---
title: "Configurar relatórios de armazenamento"
description: "Este artigo descreve como configurar os parâmetros padrão de relatórios de armazenamento"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f62109a8d3ea3e4e6386956789d276f9aa911e80
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="configure-storage-reports"></a>Configurar relatórios de armazenamento

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Você pode configurar os parâmetros padrão de relatórios de armazenamento. Esses parâmetros padrão são usados para relatórios de incidentes gerados quando ocorre um evento de triagem de arquivo ou cota. Eles também são usados para relatórios agendados e por demanda, e você pode substituir os parâmetros padrão quando definir as propriedades específicas desses relatórios.

> [!Important]
> Quando você altera os parâmetros padrão para um tipo de relatório, as alterações afetam relatórios de todos os incidentes e quaisquer tarefas de relatórios programadas que usem os padrões.

## <a name="to-configure-the-default-parameters-for-storage-reports"></a>Para configurar os parâmetros padrão de Relatórios de Armazenamento

1. Na árvore de console, clique com o botão direito do mouse em **Gerenciador de Recursos de Servidor de Arquivos** e em **Configurar Opções**. A caixa de diálogo **Opções do Gerenciador de Recursos de Servidor de Arquivos** é aberta.

2. Na guia **Relatórios de Armazenamento** em **Configurar os parâmetros padrão**, selecione o tipo de relatório que você deseja modificar.

3. Clique em **Editar parâmetros**.

4. Dependendo do tipo de relatório que você selecionar, parâmetros de relatório diferentes estarão disponíveis para edição. Execute todas as modificações necessárias e, em seguida, clique em **OK** para salvá-las como os parâmetros padrão desse tipo de relatório.

5.  Repita as etapas 2 a 4 para cada tipo de relatório que deseja editar.

6. Para ver uma lista dos parâmetros padrão de todos os relatórios, clique em **Examinar relatórios**. Em seguida, clique em **Fechar**.

7.  Clique em **OK**.

## <a name="see-also"></a>Veja também

-   [Definindo Opções do Gerenciador de Recursos de Servidor de Arquivos](setting-file-server-resource-manager-options.md)
-   [Gerenciamento de relatórios de armazenamento](storage-reports-management.md)