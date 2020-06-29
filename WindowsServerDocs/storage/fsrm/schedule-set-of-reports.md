---
title: Agende um conjunto de relatórios
description: Este artigo descreve como gerar um conjunto de relatórios em uma programação regular
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: cc0c155afe62a22c9a39b2c1dd89730246709221
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475493"
---
# <a name="schedule-a-set-of-reports"></a>Agende um conjunto de relatórios

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Para gerar um conjunto de relatórios regularmente, você agenda uma *tarefa de relatório*. A tarefa de relatório especifica os relatórios para gerar e quais parâmetros devem ser usados; os volumes e pastas a serem incluídos no relatório; frequência para gerar os relatórios e em quais formatos de arquivo salvá-los.

Relatórios agendados são salvos em um local padrão, que você pode especificar na caixa de diálogo **Opções do Gerenciador de Recursos de Servidor de Arquivos**. Você também tem a opção de fornecer os relatórios por email para um grupo de administradores.

> [!Note]
> Para minimizar o impacto do relatório de desempenho de processamento, gere vários relatórios na mesma agenda para que os dados sejam coletados apenas uma vez. Para adicionar rapidamente os relatórios para tarefas de relatório existentes, você pode usar a ação **Adicionar ou remover relatórios de uma tarefa de relatório**. Isso permite que você adicionou remova relatórios de várias tarefas de relatório e editar os parâmetros de relatório. Para alterar agendas ou endereços de entrega, você deve editar tarefas individuais do relatório.

## <a name="to-schedule-a-report-task"></a>Para agendar uma Tarefa de relatório

1. Clique no nó **Gerenciamento de relatórios de armazenamento**.

2. Clique com botão direito **Gerenciamento de relatórios de armazenamento** e clique em **Agendar uma nova tarefa de relatório** (ou selecione **Agendar uma nova tarefa de relatório** no painel **Ações**). Isso abre a caixa de diálogo **Propriedades da Tarefa de Relatórios de Armazenamento**.

3. Para selecionar volumes ou pastas para gerar relatórios:

   -   Em **Escopo**, clique em **Adicionar**.
   -   Navegue até o volume ou pasta em que você deseja gerar os relatórios, selecione-a e, em seguida, clique em **OK** para adicionar o caminho à lista.
   -   Adicione quantos volumes ou pastas que você deseja incluir nos relatórios. (Para remover um volume ou pasta, clique no demarcador e depois clique em **Remover**).

4. Para especificar quais relatórios gerar:

   -  Em **Relatar dados**, selecione cada relatório que você deseja incluir. Por padrão, todos os relatórios são gerados para uma tarefa agendada de relatório.

   Para editar os parâmetros de um relatório:

   -   Clique no rótulo do relatório e em **Editar parâmetros**.
   -   Na caixa de diálogo **Parâmetros de relatório**, edite os parâmetros conforme necessário e clique em **OK**.

   -   Para ver uma lista de parâmetros de todos relatórios selecionados, clique em **Rever relatórios selecionados**. Em seguida, clique em **Fechar**.

5. Para especificar os formatos para salvar os relatórios:

   -  Em **Relatar formatos**, selecione um ou mais formatos para os relatórios agendados. Por padrão, os relatórios são gerados em HTML dinâmico (DHTML). Você também pode selecionar formatos de texto, HTML, XML e CSV. Os relatórios são salvos no local padrão para relatórios programados.

6. Para fornecer cópias dos relatórios aos administradores por email:

   - Na guia **Entrega**, marque a caixa de seleção **Enviar relatórios para os seguintes administradores** e, em seguida, digite os nomes das contas administrativas que receberão relatórios.
   - Use o formato <em>account@domain</em> e use ponto e vírgula para separar várias contas.

7. Para agendar os relatórios:

   Na guia **Programação**, clique em **Criar programação**, e depois na caixa de diálogo **Agendar**, clique em **Novo**. Isso exibe um agendamento padrão para 9:00. diariamente, mas você pode modificar o agendamento padrão.

   -   Para especificar uma frequência para gerar relatórios, selecione um intervalo na lista suspensa **Agendar tarefas**.
       Você pode agendar diariamente, semanalmente ou mensalmente os relatórios ou gera-los apenas uma vez. Você também pode gerar relatórios na inicialização do sistema ou login ou quando o computador estiver ocioso por um tempo especificado.
   -   Para fornecer informações adicionais sobre agendamento para o intervalo escolhido, modificar ou definir os valores nas opções **Agendar tarefas**.
       Essas opções são alteradas com base no intervalo que você escolher. Por exemplo, para um relatório semanal, você pode especificar o número de semanas entre os relatórios e em quais dias da semana eles serão gerados.
   -   Para especificar a hora do dia quando você deseja gerar o relatório, digite ou selecione o valor na caixa **Hora de início**.
   -   Para acessar opções de agendamento adicionais (incluindo uma data de início e final para a tarefa), clique em **Avançado**.
   -   Para salvar o agendamento, clique em **OK**.
   -  Para criar um agendamento adicional para uma tarefa (ou modificar um agendamento existente), na guia **Agendamento**, clique em **Editar agendamento**.

8. Para salvar uma tarefa de relatório, clique em **OK**.

A tarefa de relatório é adicionada ao nó **Gerenciamento de relatórios de armazenamento**. Tarefas são identificadas pelos relatórios a serem gerado, o namespace a ser relatado no e o agendamento de relatório.

Além disso, você pode exibir o status atual do relatório (esteja o relatório sendo executado ou não), o último tempo de execução e o resultado de funcionamento e a próxima hora agendada de execução.

## <a name="additional-references"></a>Referências adicionais

-   [Gerenciamento de relatórios de armazenamento](storage-reports-management.md)
-   [Definir opções do Gerenciador de Recursos de Servidor de Arquivos](setting-file-server-resource-manager-options.md)


