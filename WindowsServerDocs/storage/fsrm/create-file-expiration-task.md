---
title: Criar uma tarefa de expiração de arquivos
description: Este artigo descreve o processo de criação de uma tarefa de gerenciamento de arquivo para arquivos prestes a vencer
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 0ff4b46064ca780d63c6f06898c114cb180c3665
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971123"
---
# <a name="create-a-file-expiration-task"></a>Criar uma tarefa de expiração de arquivos

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

O procedimento a seguir percorre com você o processo de criação de uma tarefa de gerenciamento de arquivos para arquivos expirados. As tarefas de expiração de arquivo são usadas para mover automaticamente todos os arquivos que correspondem a certos critérios para um diretório de expiração especificado, onde um administrador pode então recuperar essas arquivos e deletá-los.

Quando uma tarefa de expiração de arquivo é executada, um novo diretório é criado no diretório de expiração, agrupados pelo nome do servidor no qual a tarefa foi executada.

O nome do novo diretório é baseado no nome da tarefa de gerenciamento de arquivos e na hora em que ela foi executada.Quando um arquivo expirado é encontrado, ele é movido para o novo diretório, preservando, ao mesmo tempo, sua estrutura original de diretório.

## <a name="to-create-a-file-expiration-task"></a>Para criar uma tarefa de expiração de arquivo

1. Clique no nó **Tarefas de Gerenciamento de Arquivos**.

2. Clique com botão direito em **Tarefas de gerenciamento de arquivo** e clique em **Criar tarefas de gerenciamento de arquivo** (ou clique em **Criar tarefas de gerenciamento de arquivo** no painel **Ações**). A caixa de diálogo **Criar Tarefa de Gerenciamento de Arquivos** é aberta.

3. Na guia **Geral**, insira a seguinte informação.

   -   **Nome**. Insira um nome para a tarefa.

   -   **Descrição**. Insira um rótulo descritivo opcional para essa tarefa.

   -   **Escopo**. Adicione os diretórios que essa tarefa deve operar usando o botão **Adicionar**. Opcionalmente, os diretórios podem ser removidos da lista usando o botão **Remover**. A tarefa de gerenciamento de arquivo será aplicada a todas as pastas e suas subpastas nessa lista.

4. Na guia **Ação**, insira as seguintes informações:

   - **Type**. Selecione **Arquivo expiração** na caixa suspensa.

   - **Diretório de Expiração**. Selecione um diretório onde os arquivos serão expirados.

     > [!Warning]
     > Não selecione um diretório no escopo da tarefa, conforme definido na etapa anterior. Ao fazer isso, você poderia causar um loop interativo que levaria à instabilidade do sistema e à perda de dados.

5. Como opção, na guia **Notificação**, clique em **Adicionar** para enviar notificações por email, registrar um evento ou executar um comando ou fazer script de um número mínimo de dias antes da tarefa executar uma ação em um arquivo.

   - Na caixa de combinação **Número de dias antes de tarefa é executada para enviar a notificação**, digite ou selecione um valor para especificar o número mínimo de dias antes de um arquivo que está sendo acionado em que uma notificação será enviada.

     > [!Note]
     > As notificações são enviadas apenas quando uma tarefa é executada. Se o número mínimo especificado de dias para enviar uma notificação não coincidir com uma tarefa agendada, a notificação será enviada no dia da tarefa agendada anterior.

   - Para configurar notificações por email, clique na guia **Mensagem de email** e insira as seguintes informações:

     - Para notificar os administradores quando um limite é alcançado, selecione a caixa de seleção **Enviar email para os seguintes administradores** e, em seguida, digite os nomes das contas administrativas que receberão as notificações. Use o formato <em>account@domain</em> e use ponto e vírgula para separar várias contas.

     - Para enviar email para a pessoa cujos arquivos estão prestes a expirar, selecione a caixa de seleção **Envie um email para o usuário cujos arquivos estão prestes a expirar**.

     - Para configurar a mensagem, edite o padrão assunto linha e corpo da mensagem que são fornecidas. O texto entre colchetes insere informações variáveis sobre o evento de cota que causou a notificação. Por exemplo, a variável de ** \[ proprietário \] do arquivo de origem** insere o nome do usuário cujo arquivo está prestes a expirar. Para inserir variáveis adicionais no texto, clique em **Inserir variável**.

     - Para anexar uma lista dos arquivos que estão prestes a expirar, clique em **Anexar ao email uma lista dos arquivos no qual ação será executada** e digite ou selecione um valor para **Número máximo de arquivos na lista**.

     - Para configurar cabeçalhos adicionais (inclusive de, Cc, Cco e para resposta), clique em **Cabeçalhos de email adicionais**.

   - Para registrar um evento, clique na guia **Log de eventos** e selecione a caixa de seleção **Enviar um aviso ao log de eventos** e edite a entrada de log padrão.

   - Para executar um comando ou script, clique na guia **Comando** guia e selecione a caixa de seleção **Executar esse comando ou script**. Em seguida, digite o comando ou clique em **Procurar** para procurar o local onde o script está armazenado. Você também pode inserir o comando argumentos, selecione um diretório de trabalho para o comando ou script ou modificar a configuração de segurança do comando.

6. Opcionalmente, use a guia **Relatório** para gerar um ou mais logs ou relatórios de armazenamento.

   - Para gerar logs, selecione a caixa de seleção **Gerar log** e selecione um ou mais logs disponíveis.

   - Para gerar relatórios, selecione a caixa de seleção **Gerar um relatório** e depois selecione um o mais formatos de relatório disponíveis.

   - Para criar logs gerados por email ou relatórios de armazenamento, selecione a caixa de seleção **Enviar relatórios para os seguintes administradores** e digite um ou mais endereços de email administrativos usando o formato <em>account@domain</em>. Use um ponto-e-vírgula para separar vários endereços.

     > [!Note]
     > O relatório é salvo no local padrão de relatórios de incidentes, que você pode modificar na caixa de diálogo **Opções do Gerenciador de Recursos de Servidor de Arquivos**.

7. Opcionalmente, use a guia **Condição** para executar essa tarefa somente em arquivos que correspondam a um conjunto definido de condições. As seguintes configurações estão disponíveis:

    -   **Condições de propriedade**. Clique em **Adicionar** para criar uma nova condição baseada na classificação do arquivo. Isso abrirá a caixa de diálogo **Condição de propriedade**, que permite que você selecione uma propriedade, um operador para executar na propriedade e o valor para comparar com a propriedade. Depois de clicar em **OK**, em seguida, você pode criar condições adicionais ou editar ou remover uma condição existente.

    -   **Dias desde que o arquivo foi modificado**. Clique na caixa de seleção e, em seguida, insira um número de dias na caixa de rotação. Isso resultará na tarefa de gerenciamento de arquivos que estão sendo aplicada somente a arquivos que não foram modificados para mais do que o número de dias especificado.

    -   **Dias desde que o arquivo foi acessado**. Clique na caixa de seleção e, em seguida, insira um número de dias na caixa de rotação. Se o servidor está configurado para rastrear carimbos para quando os arquivos foram acessados pela última, isso resultará na tarefa de gerenciamento de arquivos que estão sendo aplicada somente aos arquivos que não tenham sido acessados por mais do que o número de dias especificado. Se o servidor não estiver configurado para rastrear as horas acessadas, essa condição será ineficaz.

    -   **Dias desde que o arquivo foi criado**. Clique na caixa de seleção e, em seguida, insira um número de dias na caixa de rotação. Isso resultará na tarefa somente ser aplicada a arquivos que foram criados pelo menos o número de dias especificado.

    -   **Início efetivo**. Defina uma data quando esta tarefa de gerenciamento de arquivo deve começar o processamento arquivos. Essa opção é útil para atrasar a tarefa até que você tenha tido a oportunidade para notificar os usuários ou fazer outras preparações antecipadamente.

8. Na guia **Programação**, clique em **Criar programação**, e depois na caixa de diálogo **Agendar**, clique em **Novo**. Isso exibe um agendamento padrão para 9:00. diariamente, mas você pode modificar o agendamento padrão. Quando você terminar de configurar o agendamento, clique em **OK** e, em seguida, clique em **OK** novamente.

## <a name="additional-references"></a>Referências adicionais

-   [Gerenciamento de classificação](classification-management.md)
-   [Tarefas de gerenciamento de arquivos](file-management-tasks.md)