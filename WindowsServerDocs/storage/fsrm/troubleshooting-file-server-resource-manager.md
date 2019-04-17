---
title: "Solução de problemas do Gerenciador de Recursos de Servidor de Arquivos"
description: Este artigo descreve como solucionar problemas comuns ao usar o Gerenciador de recursos do servidor de arquivos
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 923710fac426f63d2c38d9b9a68c92427783abb1
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="troubleshooting-file-server-resource-manager"></a>Solução de problemas do Gerenciador de Recursos de Servidor de Arquivos

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Esta seção lista problemas comuns que você pode encontrar ao usar Gerenciador de Recursos de Servidor de Arquivos.

> [!Note]
> É uma boa opção de solução de problemas procurar os logs de eventos que foram gerados pelo Gerenciador de Recursos de Servidor de Arquivos. Todas as entradas de log de eventos para o Gerenciador de Recursos de Servidor de Arquivos podem ser encontradas no evento do **Aplicativo** sob o **SRMSVC** de origem

## <a name="i-am-not-receiving-e-mail-notifications"></a>Não estou recebendo notificações por email.

-   **Causa**: as opções de email não foram configuradas ou foram configuradas incorretamente.

-   **Solução**: na guia **Notificações por email**, na caixa de diálogo **Opções do Gerenciador de Recursos do Servidor de Arquivos**, verifique se o servidor SMTP e os destinatários de email padrão foram especificados e são válidos. Envie um email de teste para confirmar se as informações estão corretas e se o servidor SMTP usado para enviar as notificações está funcionando corretamente. Para mais informações, consulte [Configurar notificações por email](configure-email-notifications.md).


## <a name="i-am-only-receiving-one-e-mail-notification-even-though-the-event-that-triggered-that-notification-happened-several-times-in-a-row"></a>Só estou recebendo uma notificação por email, mesmo que o evento que disparou essa notificação tenha acontecido várias vezes em uma linha.

-   **Causa**: quando um usuário tenta por várias vezes salvar um arquivo que está bloqueado ou salvar um arquivo que excede o limite de cota e há uma notificação por email configurada para esse evento de cota ou triagem de arquivo, somente um email é enviado para o administrador por um período de 60 minutos, por padrão. Isso evita a abundância de mensagens redundantes na conta de email do administrador.

-   **Solução**: na guia **Limites de notificação**, na caixa de diálogo **Opções do Gerenciador de Recursos do Servidor de Arquivos**, você pode definir um limite de tempo para cada um dos tipos de notificação: email, log de eventos, comando e relatório. Cada limite especifica um período de tempo que deve passar antes de outra notificação do mesmo tipo ser gerada para o mesmo problema. Para mais informações, consulte [Configurar limites de notificação](configure-notification-limits.md).


## <a name="my-storage-reports-keep-failing-and-little-or-no-information-is-available-in-the-event-log-regarding-the-source-of-the-failure"></a>Meus relatórios de armazenamento continuam caindo e pouca ou nenhuma informação está disponível no Log de eventos com relação à origem da falha.

-   **Causa**: o volume onde os relatórios estão sendo salvos pode estar corrompido.
-   **Solução**: execute **chkdsk** no volume e tente gerar os relatórios novamente.

## <a name="my-file-screening-audit-reports-do-not-contain-any-information"></a>Meus subordinados de auditoria de triagem de arquivo não contêm nenhuma informação.

-   **Causar**: um ou mais dos procedimentos a seguir podem ser a causa:
    -   O banco de dados de auditoria não está configurado para registrar atividade de triagem de arquivo.
    -   O banco de dados de auditoria está vazio (ou seja, nenhum arquivo de atividade de triagem foi registrado).
    -   Os parâmetros para o relatório de auditoria de triagem de arquivo não estão selecionando dados do banco de auditoria.
    
-   **Solução**: na guia **Auditoria da triagem de arquivo**, a caixa de diálogo **Opções do Gerenciador de Recursos do Servidor de Arquivos**, verifique se a caixa de seleção **Registrar atividade no banco de dados de auditoria de triagem de arquivo** está marcada.
    -   Para obter mais informações sobre a gravação de atividade de triagem, consulte [Configurar auditoria da triagem de arquivo](configure-file-screen-audit.md).
    -   Para configurar os parâmetros padrão do relatório de auditoria de triagem de arquivo, consulte [Configurar relatórios de armazenamento](configure-storage-reports.md).
    -   Para editar os parâmetros de relatório para tarefas de relatório agendadas ou relatórios por demanda, consulte [Gerenciamento de relatórios de armazenamento](storage-reports-management.md).

## <a name="the-used-and-available-values-for-some-of-the-quotas-i-have-created-do-not-correspond-to-the-actual-limit-setting"></a>Os valores "Usado" e "Disponível" de algumas das cotas que criei não correspondem a configuração de "Limite" real.

-   **Causa**: você pode ter uma cota aninhada, onde a cota de uma subpasta deriva um limite mais restritivo da cota de sua pasta pai. Por exemplo, você pode ter um limite de cota de 100 megabytes (MB) aplicados a uma pasta pai e uma cota de 200 MB aplicados separadamente para cada uma de suas subpastas. Se a pasta pai tiver um total de 50 MB de dados armazenados nela (a soma dos dados armazenados nas subpastas), cada uma das subpastas listará somente 50 MB de espaço disponível.

-   **Solução**: em **Gerenciamento de cota**, clique em **Cotas**. No painel **Resultados**, selecione a entrada de cota para a qual está solucionando problemas. No painel **Ações**, clique em **Exibir cotas que afetam a pasta** e, em seguida, procure as cotas que são aplicadas às pastas pai. Isso permitirá que você identifique quais cotas da pasta pai têm uma configuração de limite de armazenamento inferior a cota que você selecionou.

