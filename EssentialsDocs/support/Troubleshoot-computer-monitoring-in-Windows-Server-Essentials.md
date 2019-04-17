---
title: Solucionar problemas de monitoramento de computador no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f1e6b377-4a24-4d28-9b25-05910914826b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 72fe309e0e7ce6d7227cce8b7f2c5dbf018eb4a1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-computer-monitoring-in-windows-server-essentials"></a>Solucionar problemas de monitoramento de computador no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este tópico fornece a solução de problemas para problemas encontrados ao monitorar o status de integridade dos computadores no Visualizador de alerta e por meio de notificações de email no Windows Server Essentials.  
  
> [!NOTE]
>  Para as informações de solução de problemas mais atuais da comunidade do Windows Server Essentials, sugerimos que você visitar o [Fórum do Windows Server Essentials](https://social.technet.microsoft.com/Forums/winserveressentials/threads). O fórum do Windows Server Essentials é um ótimo lugar para procurar ajuda ou fazer uma pergunta.  
  
##  <a name="BKMK_TS"></a>Solução de problemas de notificações de email para alertas  
 Esta seção lista vários problemas que você pode encontrar ao usar notificações por email para alertas.  
  
### <a name="cannot-send-the-test-email-for-the-alert"></a>Não é possível enviar o email de teste para o alerta  
 **Problema** você receber um erro mensagem que diz, não é possível enviar o email de teste de alerta.  
  
 **Causa** esse erro pode ocorrer devido a qualquer um dos seguintes problemas nas configurações de notificações de alerta:  
  
-   Um nome de servidor SMTP incorreto ou o número da porta.  
  
-   Ele foi especificado incorretamente que o servidor de SMTP requer uma conexão única Sockets Layer (SSL).  
  
-   O servidor de SMTP requer autenticação e credenciais incorretas foram inseridas.  
  
 **Soluções** corrija os erros em suas configurações de notificação de email.  
  
##### <a name="to-identify-issues-in-your-email-notification-settings"></a>Para identificar problemas em suas configurações de notificação de email  
  
-   Peça a seu provedor de serviços de Internet (ISP) para o nome do servidor SMTP correto, número da porta e uso SSL.  
  
-   Examine os arquivos de log para notificações de email para o alerta no servidor, neste local:  
  
     %ProgramData%\Microsoft\Windows Server\Logs\SharedServiceHost-AlertServiceConfig.log  
  
    > [!TIP]
    >  Para ver a pasta ProgramData, você deve ter oculta itens exibidos. Se você don t visualizar a pasta ProgramData, na faixa de opções s **exibição** guia, o **Mostrar/ocultar** grupo, selecione o **itens ocultos** caixa de texto.  
  
##### <a name="to-update-your-email-notification-setup-for-alerts"></a>Para atualizar sua configuração de notificação de email para alertas  
  
1.  No painel, clique em qualquer ícone de alerta no canto superior direito para abrir o Visualizador de alerta.  
  
2.  Na parte inferior do Visualizador de alerta, clique em **configurar uma notificação de email para alertas**.  
  
3.  No **configurar uma notificação de email para alertas** caixa de diálogo, clique em **habilitar**.  
  
4.  No **configurações de SMTP** caixa de diálogo, atualizar as configurações de SMTP e clique em **Okey**.  
  
5.  Para testar as configurações atualizadas, clique em **aplicar e enviar email**.  
  
6.  Depois que você verifique se o email de teste foi bem-sucedida, clique em **Okey** para salvar as configurações atualizadas.  
  
### <a name="test-email-notification-does-not-list-any-alerts"></a>Notificação de email de teste não listar os alertas  
 **Problema** as notificações de email de teste para alertas não exibir os alertas mesmo que haja alertas listadas no Visualizador de alerta.  
  
 **Solução** nem todos os alertas que são relatados no Visualizador de alerta geram uma notificação por email. Somente os alertas que estão configurados para ser escalado como uma notificação por email dentro de seus arquivos de definição de integridade são enviados como emails para os destinatários de email especificada.  
  
 Quando você clica em **aplicar e enviar email**, normalmente você receberá uma notificação de email de exemplo com nenhum alertas de integridade listadas. No entanto, se um alerta de saúde que esteja configurado para enviar notificações por email é identificado durante esse processo de teste, esse alerta está incluída no email de teste.  
  
### <a name="active-alerts-are-displayed-for-an-uninstalled-application"></a>Alertas ativos são exibidas para um aplicativo não instalado  
 **Problema** alertas ativos para um aplicativo são exibidos, mesmo que o aplicativo e seu arquivo de definição de integridade tiverem sido desinstalados.  
  
 **Solução** exclua manualmente os alertas ativos do aplicativo instalado. Para excluir um alerta, faça o seguinte.  
  
##### <a name="to-delete-an-alert-from-the-server-by-using-the-dashboard"></a>Para excluir um alerta do servidor usando o painel  
  
1.  No servidor, abra o painel.  
  
2.  No painel de navegação, clique em qualquer ícone de alerta exibido (crítico, aviso ou informativo). Isso inicia o Visualizador de alerta.  
  
3.  No Visualizador de alerta, clique com botão direito o alerta que você deseja excluir e clique em **excluir esse alerta**.
