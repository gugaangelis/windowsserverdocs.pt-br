---
title: Criar um modelo de triagem de arquivo
description: Este artigo descreve como criar um modelo de triagem de arquivo
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b06597bce0b88ed5a2e98ad45d0cbc355d1b13fc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858347"
---
# <a name="create-a-file-screen-template"></a>Criar um modelo de triagem de arquivo

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Um *Modelo de triagem de arquivo* define um conjunto de grupos de arquivos para triagem, o tipo de triagem a ser realizada (ativa ou passiva) e, opcionalmente, um conjunto de notificações que serão geradas automaticamente quando um usuário salva, ou tentar salvar, um arquivo não autorizado.

Gerenciador de recursos do servidor de arquivos pode enviar mensagens de email para os administradores ou usuários específicos, registra um evento, executar um comando ou um script ou gerar relatórios. Você pode configurar mais de um tipo de notificação para um evento de triagem de arquivo.

Ao criar triagens de arquivo exclusivamente a partir de modelos, você pode gerenciar suas triagens de arquivo de forma centralizada, atualizando os modelos em vez de replicar alterações em cada triagem de arquivo. Esse recurso simplifica a implementação das alterações de políticas de armazenamento, fornecendo um ponto central no qual todas as atualizações podem ser feitas.

> [!Important]
> Para enviar notificações por email e configurar os relatórios de armazenamento com parâmetros que são apropriados para seu ambiente de servidor, primeiro você deve definir as opções gerais do Gerenciador de recursos do servidor de arquivos. Para saber mais, confira [Definindo Opções do Gerenciador de Recursos de Servidor de Arquivos](setting-file-server-resource-manager-options.md).

## <a name="to-create-a-file-screen-template"></a>Para criar um modelo de triagem de arquivo

1.  Em **Gerenciamento de triagem de arquivo**, clique no nó **Modelos de triagem de arquivo**.

2.  Clique com botão direito em **Modelos de triagem de arquivo**e clique em **Criar modelo da triagem de arquivo** (ou selecione **Criar modelo da triagem de arquivo** no painel **Ações**). A caixa de diálogo **Crie um modelo de triagem de arquivo** é aberta.

3.  Se você quiser copiar as propriedades de um modelo existente para usar como base para um novo modelo, selecione um modelo na lista suspensa **Copiar propriedades do modelo** e clique em **Copiar**.

    Se você optou por usar as propriedades de um modelo existente ou se está criando um novo modelo, modifique ou defina os seguintes valores na guia **Configurações**.

4.  Na caixa de texto **Nome do modelo**, insira um nome para o novo modelo.

5.  Em **Tipo de triagem**, clique na opção **Triagem ativa** ou **Triagem passiva**. (Triagem ativa impede que os usuários salvem arquivos que são membros de grupos de arquivos bloqueados e gera notificações quando os usuários tentam salvar arquivos não autorizados. A triagem passiva envia notificações configuradas, mas ela não impede que os usuários salvando arquivos).

6.  Para especificar quais arquivos agrupar à triagem:

    Em **Grupos de arquivos**, selecione cada grupo de arquivos que você deseja incluir. (Para marcar a caixa de seleção do grupo de arquivos, clique duas vezes no rótulo do grupo de arquivos.)

    Se você quiser exibir os tipos de arquivo que um grupo de arquivos inclui e exclui, clique no rótulo do grupo de arquivo e, em seguida, clique em **editar**. Para criar um novo grupo de arquivos, clique em **criar**.

    Além disso, você pode configurar Gerenciador de recursos do servidor de arquivos para gerar uma ou mais notificações definindo opções a seguir nas guias **Mensagem de Email**, **Log de eventos**, **Comando** e **Relatório**.

7.  Para configurar notificações por email:

    Na guia **Email**, defina as seguintes opções:

    -   Para notificar os administradores quando um usuário ou aplicativo tenta salvar um arquivo não autorizado, selecione o **Enviar email para os seguintes administradores** caixa de seleção e, em seguida, digite os nomes das contas administrativas que receberão as notificações. Use o formato *account*@*domain* e use ponto e vírgula para separar várias contas.
    -   Para enviar email para o usuário que tentou salvar o arquivo, selecione a caixa de seleção **Enviar um email para o usuário que tentou salvar um arquivo não autorizado**.
    -   Para configurar a mensagem, edite o padrão assunto linha e corpo da mensagem que são fornecidas. O texto entre colchetes insere informações variáveis sobre o evento de triagem de arquivo que causou a notificação. Por exemplo, o \[ **proprietário da fonte de e/s** \] variável insere o nome do usuário que tentou salvar um arquivo não autorizado. Para inserir variáveis adicionais no texto, clique em **Inserir variável**.
    -   Para configurar cabeçalhos adicionais (inclusive de, Cc, Cco e para resposta), clique em **Cabeçalhos de email adicionais**.

8.  Para registrar um erro ao log de eventos quando um usuário tenta salvar um arquivo não autorizado:

    Na guia **Log de eventos**, selecione a caixa de seleção **Enviar um aviso ao log de eventos** e edite a entrada de log padrão.

9.  Para executar um comando ou script quando um usuário tenta salvar um arquivo não autorizado:

    Na guia **Comando**, selecione a caixa de seleção **Executar esse comando ou script**. Em seguida, digite o comando ou clique em **Procurar** para procurar o local onde o script está armazenado. Você também pode inserir o comando argumentos, selecione um diretório de trabalho para o comando ou script ou modificar a configuração de segurança do comando.

10. Para gerar um ou mais armazenamento relata quando um usuário tenta salvar um arquivo não autorizado:

    Na guia **Relatório**, selecione a caixa de seleção **Gerar relatórios** e, em seguida, selecione os relatórios para gerar. (Você pode escolher um ou mais destinatários de email administrativos para o relatório ou enviar o relatório ao usuário que tentou salvar o arquivo.)

    O relatório é salvo no local padrão de relatórios de incidentes, que você pode modificar na caixa de diálogo **Opções do Gerenciador de Recursos de Servidor de Arquivos**.

11. Após selecionar todas as propriedades do modelo de arquivo que você deseja usar, clique em **OK** para salvar o modelo.

## <a name="see-also"></a>Consulte também

-   [Gerenciamento de triagem de arquivo](file-screening-management.md)
-   [Opções do Gerenciador de recursos de servidor de arquivos de configuração](setting-file-server-resource-manager-options.md)
-   [Editar propriedades do modelo de triagem de arquivo](edit-file-screen-template-properties.md)

