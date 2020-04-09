---
title: Solucionar problemas de monitoramento de computador no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: f1e6b377-4a24-4d28-9b25-05910914826b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5e2c8706c29e63a74d637e4a2c072d0ffa0a56d5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852199"
---
# <a name="troubleshoot-computer-monitoring-in-windows-server-essentials"></a>Solucionar problemas de monitoramento de computador no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este tópico fornece a solução de problemas encontrados durante o monitoramento do status de integridade de computadores no Visualizador de alertas e por meio de notificações por email no Windows Server Essentials.  
  
> [!NOTE]
>  Para obter as informações de solução de problemas mais recentes da Comunidade do Windows Server Essentials, sugerimos que você visite o [Fórum do Windows Server Essentials](https://social.technet.microsoft.com/Forums/winserveressentials/threads). O fórum do Windows Server Essentials é uma boa oportunidade para obter ajuda ou para fazer uma pergunta.  
  
##  <a name="troubleshooting-email-notifications-for-alerts"></a><a name="BKMK_TS"></a>Solucionando problemas de notificações por email para alertas  
 Esta seção lista diversos problemas que você pode encontrar ao usar notificações por email para alertas.  
  
### <a name="cannot-send-the-test-email-for-the-alert"></a>Não é possível enviar o email de teste para o alerta  
 **Problema** Você recebe uma mensagem de erro que diz, não é possível enviar o email de teste para o alerta.  
  
 **Causa** Esse erro poderá ocorrer devido a qualquer um dos seguintes problemas nas configurações de notificações de alerta:  
  
- Um nome de servidor SMTP ou número de porta incorreto.  
  
- Foi especificado incorretamente que o servidor SMTP requer uma conexão Single Sockets Layer (SSL).  
  
- O servidor SMTP exige autenticação; além disso, credenciais incorretas foram inseridas.  
  
  **Soluções** Corrigir quaisquer erros em suas configurações de notificação por email.  
  
##### <a name="to-identify-issues-in-your-email-notification-settings"></a>Para identificar problemas em suas configurações de notificação por email  
  
-   Peça ao seu provedor de serviços de Internet (ISP) que forneça o nome do servidor SMTP, o número da porta e o uso de SSL corretos.  
  
-   Revise os arquivos de log para notificações por email para o alerta no servidor, neste local:  
  
     %ProgramData%\Microsoft\Windows Server\Logs\SharedServiceHost-AlertServiceConfig.log  
  
    > [!TIP]
    >  Para ver a pasta ProgramData, você deve usar uma configuração para exibir itens ocultos. Se você não vir a pasta ProgramData, na guia **exibição** da faixa de opções, no grupo **Mostrar/ocultar** , marque a caixa de texto **itens ocultos** .  
  
##### <a name="to-update-your-email-notification-setup-for-alerts"></a>Para atualizar a configuração de notificação por email para alertas  
  
1.  No Painel, clique em qualquer ícone de alerta no canto superior direito para abrir o Visualizador de alerta.  
  
2.  Na parte inferior do Visualizador de alertas, clique em **Configurar notificação de email para alertas**.  
  
3.  Na caixa de diálogo **Configurar notificação de email para alertas**, clique em **Habilitar**.  
  
4.  Na caixa de diálogo **Configurações de SMTP**, atualize as configurações de SMTP e, em seguida, clique em **OK**.  
  
5.  Para testar suas configurações atualizadas, clique em **Aplicar e enviar emails**.  
  
6.  Depois de verificar se o email de teste foi bem-sucedido, clique em **OK** para salvar suas configurações atualizadas.  
  
### <a name="test-email-notification-does-not-list-any-alerts"></a>A notificação por email de teste não lista nenhum alerta  
 **Problema** as notificações por email de teste para alertas não exibem nenhum alerta, mesmo que haja alertas listados no Visualizador de alertas.  
  
 **Solução** Nem todos os alertas que são relatados no Visualizador de alertas geram uma notificação por email. Somente os alertas que estão configurados para ser dimensionados como uma notificação por email em seus arquivos de definição de integridade são enviados como emails para os destinatários de email especificados.  
  
 Quando você clica em **aplicar e enviar emails**, geralmente você receberá uma notificação de email de amostra com alertas de integridade não listados. No entanto, se um alerta de integridade que é configurado para enviar notificações por email é identificado durante esse processo de teste, esse alerta é incluído no email de teste.  
  
### <a name="active-alerts-are-displayed-for-an-uninstalled-application"></a>Alertas ativos são exibidas para um aplicativo desinstalado  
 **Problema** Alertas ativos para um aplicativo são exibidos, mesmo que o aplicativo e seu arquivo de definição de integridade tenham sido desinstalados.  
  
 **Solução** Você deve excluir manualmente os alertas ativos do aplicativo desinstalado. Para excluir um alerta, faça o que é descrito a seguir.  
  
##### <a name="to-delete-an-alert-from-the-server-by-using-the-dashboard"></a>Para excluir um alerta do servidor usando o Painel  
  
1.  No servidor, abra o Painel.  
  
2.  No painel de navegação, clique em qualquer ícone de alerta exibida (crítico, aviso ou informacional). Isso inicia o Visualizador de alertas.  
  
3.  No Visualizador de alertas, clique no alerta que você deseja excluir e, em seguida, clique em **Excluir este alerta**.
