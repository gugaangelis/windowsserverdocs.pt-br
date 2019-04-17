---
title: Criar um modelo de cota
description: "Este artigo descreve como criar um modelo de cota para definir um limite de espaço de armazenamento"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f74382c4a5e2c0a8636edbd4f9cfe2227cd6334a
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="create-a-quota-template"></a>Criar um modelo de cota

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Um *modelo de cota* define um limite de espaço, o tipo de cota (rígida ou flexível) e, opcionalmente, um conjunto de notificações que será gerado automaticamente quando o uso da cota atingir limites definidos.

Ao criar cotas exclusivamente a partir de modelos, você pode gerenciar suas cotas de forma centralizada, atualizando os modelos em vez de replicar alterações em cada cota. Esse recurso simplifica a implementação das alterações de políticas de armazenamento, fornecendo um ponto central no qual todas as atualizações podem ser feitas.

## <a name="to-create-a-quota-template"></a>Para criar um modelo de cota

1.  Em **Gerenciamento de Cota**, clique no nó **Modelos de Cota**.

2.  Clique com o botão direito do mouse em **Modelos de Cota** e em **Criar Modelo de Cota** (ou selecione **Criar Modelo de Cota** no painel **Ações**). Essa ação abre a caixa de diálogo **Criar Modelo de Cota**.

3.  Se você quiser copiar as propriedades de um modelo existente para usar como base para um novo modelo, selecione um modelo na lista suspensa **Copiar propriedades do modelo de cota**. Em seguida, clique em **Copiar**.

    Se você optou por usar as propriedades de um modelo existente ou se está criando um novo modelo, modifique ou defina os seguintes valores na guia **Configurações**.

4.  Na caixa de texto **Nome do Modelo**, insira um nome para o novo modelo.

5.  Na caixa de texto **Rótulo**, insira um rótulo descritivo opcional que aparecerá ao lado de qualquer cota derivada do modelo.

6.  Em **Limite de Espaço**:

    -   Na caixa de texto **Limite**, insira um número e escolha uma unidade (KB, MB, GB ou TB) para especificar o limite de espaço para a cota.
    -   Clique na opção **Cota fixa** ou **Cota flexível**. (Uma cota fixa impede que os usuários salvem arquivos depois que o limite de espaço é atingido e gera notificações quando o volume de dados atinge cada limite configurado. Uma cota flexível não impõe o limite da cota, mas gera todas as notificações configuradas.)

7.  Você pode configurar uma ou mais notificações opcionais de limite para o modelo de cota, como descrito no procedimento seguir. Após selecionar todas as propriedades do modelo da cota que você deseja usar, clique em **OK** para salvar o modelo.

## <a name="setting-optional-notification-thresholds"></a>Definindo limites de notificação opcionais

Quando o armazenamento em um volume ou uma pasta atinge um limite específico ou nível que você definir, Gerenciador de recursos do servidor de arquivos pode enviar mensagens de email para os administradores de usuários, registra um evento, executar um comando ou um script ou gerar relatórios. Você pode configurar mais de um tipo de notificação para cada limite, e você pode definir vários limites para qualquer cota (ou modelo de cota). Por padrão, nenhuma notificação é geradas.

Por exemplo, você pode configurar limites para enviar uma mensagem de email para o administrador e os usuários que seriam interessante saber quando uma pasta atinge 85% do seu limite de cota e envie outra notificação quando o limite for atingido. Além disso, convém executar um script que usa o **dirquota.exe** comando para acionar a cota limitar automaticamente quando um limite for atingido.

> [!Important]
> Para enviar notificações por email e configurar os relatórios de armazenamento com parâmetros que são apropriados para seu ambiente de servidor, primeiro você deve definir as opções gerais do Gerenciador de recursos do servidor de arquivos. Para saber mais, confira [Definindo Opções do Gerenciador de Recursos de Servidor de Arquivos](setting-file-server-resource-manager-options.md)

**Para configurar notificações que o Gerenciador de recursos do servidor de arquivos irá gerar um limite de cota**

1.  Na caixa de diálogo **Criar modelo de cota**, em **Limites de notificação**, Clique em **Adicionar**. A caixa de diálogo **Adicionar limite** é aberta.

2.  Para definir uma porcentagem de limite de cota que irá gerar uma notificação:

    No **gerar notificações quando uso atinge (%)** texto, digite uma porcentagem do limite de cota para o limite de notificação. (A porcentagem padrão para o primeiro limite de notificação é 85%).

3.  Para configurar notificações por email:

    Na guia **Email**, defina as seguintes opções:

    -   Para notificar os administradores quando um limite é alcançado, selecione a caixa de seleção **Enviar email para os seguintes administradores** e, em seguida, digite os nomes das contas administrativas que receberão as notificações. Use o formato *account@domain* e use ponto e vírgula para separar várias contas.
    -   Para enviar email para a pessoa que salvou o arquivo que atingido o limite de cota, selecione a caixa de seleção **envie um email para o usuário que excedeu o limite**.
    -   Para configurar a mensagem, edite o padrão assunto linha e corpo da mensagem que são fornecidas. O texto entre colchetes insere informações variáveis sobre o evento de cota que causou a notificação. Por exemplo, a variável **\[Source Io Owner\]** insere o nome do usuário que salvou o arquivo que alcançou o limite da cota. Para inserir variáveis adicionais no texto, clique em **Inserir variável**.
    -   Para configurar cabeçalhos adicionais (inclusive de, Cc, Cco e para resposta), clique em **Cabeçalhos de email adicionais**.

4.  Para cadastrar um evento:

    Na guia **Log de eventos**, selecione a caixa de seleção **Enviar um aviso ao log de eventos** e edite a entrada de log padrão.

5.  Para executar um comando ou script:

    Na guia **Comando**, selecione a caixa de seleção **Executar esse comando ou script**. Em seguida, digite o comando ou clique em **Procurar** para procurar o local onde o script está armazenado. Você também pode inserir o comando argumentos, selecione um diretório de trabalho para o comando ou script ou modificar a configuração de segurança do comando.

6.  Para gerar um ou mais relatórios de armazenamento:

    Na guia **Relatório**, selecione a caixa de seleção **Gerar relatórios** e, em seguida, selecione os relatórios para gerar. (Você pode escolher um ou mais destinatários de email administrativos para o relatório ou enviar o relatório ao usuário que alcançou o limite.)

    O relatório é salvo no local padrão de relatórios de incidentes, que você pode modificar na caixa de diálogo **Opções do Gerenciador de Recursos de Servidor de Arquivos**.

7.  Clique em **OK** para salvar seu limite de notificação.

8.  Repita estas etapas se você quiser configurar limites de notificação adicionais para o modelo de cota.

## <a name="see-also"></a>Veja também

-   [Gerenciamento de cota](quota-management.md)
-    [Definindo Opções do Gerenciador de Recursos de Servidor de Arquivos](setting-file-server-resource-manager-options.md)
-   [Editar propriedades de modelo de cota](edit-quota-template-properties.md)
-   [Ferramentas de linha de comando](command-line-tools.md)


