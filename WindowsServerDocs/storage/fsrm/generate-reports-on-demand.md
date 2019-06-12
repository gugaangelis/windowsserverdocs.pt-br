---
title: Gerar relatórios sob demanda
description: Este artigo descreve como gerar relatórios sob demanda para analisar o uso de disco no servidor
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b51ab0ad2a9c670aa63c4c86f7ff2c052139cb95
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445968"
---
# <a name="generate-reports-on-demand"></a>Gerar relatórios sob demanda

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Durante as operações diárias, você pode usar a opção **Gerar relatórios agora** para gerar relatórios de um ou mais sob demanda. Com esses relatórios, você pode analisar os diferentes aspectos de uso de disco atual no servidor. Dados atuais são coletados antes dos relatórios são gerados.

Quando você gera relatórios sob demanda, os relatórios são salvos em um local padrão que você especificar na caixa de diálogo **Opção do Gerenciador de recursos do servidor de arquivos**, mas nenhuma tarefa de relatório é criado para uso posterior. Você pode exibir os relatórios imediatamente depois que eles são gerados ou enviar por email para um grupo de administradores.

> [!Note]
> Se você optar por abrir os relatórios imediatamente, você deve esperar enquanto os relatórios são gerados. O tempo de processamento varia de acordo com os tipos de relatórios e o escopo dos dados.

## <a name="to-generate-reports-immediately"></a>Para gerar Relatórios imediatamente

1. Clique no nó **Gerenciamento de relatórios de armazenamento**.

2. Clique com botão direito em **Gerenciamento de relatórios de armazenamento** e clique em **Gerar relatórios agora** (ou selecione **Gerar relatórios agora** no painel **Ações**). Isso abre a caixa de diálogo **Propriedades da Tarefa de Relatórios de Armazenamento**.

3. Para selecionar volumes ou pastas para gerar relatórios:

   -   Em **Escopo**, clique em **Adicionar**.
   -   Navegue até o volume ou pasta em que você deseja gerar os relatórios, selecione-a e, em seguida, clique em **OK** para adicionar o caminho à lista.
   -   Adicione quantos volumes ou pastas que você deseja incluir nos relatórios. (Para remover um volume ou pasta, clique no demarcador e depois clique em **Remover**).

4. Para especificar quais relatórios gerar:

    -   Em **Relatar dados**, selecione cada relatório que você deseja incluir.

   Para editar os parâmetros de um relatório:

   -   Clique no rótulo do relatório e em **Editar parâmetros**.
   -   Na caixa de diálogo **Parâmetros de relatório**, edite os parâmetros conforme necessário e clique em **OK**.
   -  Para ver uma lista de parâmetros de todos relatórios selecionados, clique em **Rever relatórios selecionados** e depois clique em **Fechar**.
 
5. Para especificar os formatos para salvar os relatórios:

   -  Em **Relatar formatos**, selecione um ou mais formatos para os relatórios agendados. Por padrão, os relatórios são gerados em HTML dinâmico (DHTML). Você também pode selecionar formatos de texto, HTML, XML e CSV. Os relatórios são salvos no local padrão para relatórios sob demanda.

6. Para fornecer cópias dos relatórios aos administradores por email:

   - Na guia **Entrega**, marque a caixa de seleção **Enviar relatórios para os seguintes administradores** e, em seguida, digite os nomes das contas administrativas que receberão relatórios. 
   - Use o formato <em>account@domain</em> e use ponto e vírgula para separar várias contas.

7. Para coletar os dados e gerar os relatórios, clique em **OK**. Isso abre a caixa de diálogo **Gerar Relatórios de Armazenamento**.

8. Escolha como deseja gerar os relatórios sob demanda:

   -   Se você quiser exibir os relatórios imediatamente depois que eles são gerados, clique em **Aguardar criação de relatórios e depois exibi-los**. Cada relatório abre em sua própria janela.
   -   Para exibir os relatórios mais tarde, clique em **Gerar relatórios em segundo plano**.

   Ambas as opções salvar os relatórios e se você habilitou a entrega por email, enviar relatórios para administradores nos formatos que você selecionou.

## <a name="see-also"></a>Consulte também

-   [Gerenciamento de relatórios de armazenamento](storage-reports-management.md)
-   [Definir opções do Gerenciador de Recursos de Servidor de Arquivos](setting-file-server-resource-manager-options.md)

