---
title: Gerenciar a integridade do sistema no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3043f83b-389c-4f37-a1ff-85afe99314fa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 91635a58c64fbf74d3b0139be7c9c36365487319
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-system-health-in-windows-server-essentials"></a>Gerenciar a integridade do sistema no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Este tópico discute como visualizar e responder a todos os alertas em sua rede usando o Dashboard.  
  
> [!NOTE]
>  No Windows Server Essentials e no Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada, os alertas de integridade para o servidor e computadores cliente na rede não são exibidos no Visualizador de alerta, mas em vez disso, podem ser exibidos no **relatórios de integridade** guia a **Home** página.  
  
 Windows Server Essentials ativamente monitora cada computador que esteja conectado ao servidor e alerta o administrador para problemas relacionados à saúde sistema s, incluindo as atualizações críticas ausentes proteção contra malware, definições de vírus desatualizados em computadores cliente e outros problemas importantes que exigem ação. Esses problemas são exibidos como alertas no Visualizador de alerta, que pode ser iniciado no servidor s painel ou o computador cliente s barra inicial no Windows Server Essentials ou no **relatórios de integridade** guia no Windows Server Essentials. Por padrão, os alertas são atualizados a cada 30 minutos, mas você pode avaliar sua rede para alertas a qualquer momento clicando em **atualizar** no Visualizador de alerta ou ativado o **relatórios de integridade** guia.  
  
 Os tópicos a seguir ajudarão você a entender, exibir e responder a alertas no Visualizador de alerta e também fornecer instruções para configurar seu servidor para receber notificações de alerta por email:  
  
-   [Sobre a integridade relatar suplementar](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_AddIn)  
  
-   [Exibir alertas usando o Visualizador de alerta](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_View)  
  
-   [Organizar alertas no Visualizador de alerta](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Organize)  
  
-   [Responder a alertas](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Respond)  
  
-   [Configurar notificações de email para alertas](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email)  
  
-   [Possíveis alertas de computador](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Potential)  
  
##  <a name="BKMK_AddIn"></a>Sobre a integridade relatar suplementar  
 O suplemento de relatório de integridade do Windows Server Essentials fornece consolidadas informações sobre a rede do Windows Server Essentials e permite que você distribuir essas informações para outras pessoas. Essas informações podem ser exibidas no **relatórios** guia do painel. Com o **relatórios** guia, você pode fazer o seguinte:  
  
-   [Gere um relatório sob demanda ou em agendamento](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Generate)  
  
-   [Personalizar o conteúdo do relatório](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Customize)  
  
-   [O relatório de email](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_emailreport)  
  
> [!NOTE]
>  **Windows Server Essentials:** você pode baixar o suplemento de relatório de integridade do Windows Server Essentials do [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?LinkId=266342).  
>   
>  **Windows Server Essentials:** por padrão, o suplemento de relatório de integridade é integrado ao Windows Server Essentials ou o Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada, e os relatórios de integridade são exibidos na **relatórios de integridade** guia do painel s **Home** página.  
  
###  <a name="BKMK_Generate"></a>Gere um relatório sob demanda ou em agendamento  
 Depois de instalar o suplemento de relatório de integridade e reiniciar o painel, uma nova guia, **relatórios** é adicionado ao painel. Você pode gerar um relatório de integridade sob demanda a qualquer momento clicando o **gerar um relatório de integridade** de tarefas no **relatórios** guia.  
  
 Depois que um relatório de integridade é gerado, um novo item é criado no painel de lista, identificado pela data e hora, que o relatório foi gerado. Para abrir um item, você pode clicar duas vezes no painel de lista, ou você pode selecioná-lo e, em seguida, clique em **abrir o relatório de integridade** no painel de tarefas. O relatório é exibido em uma nova janela no formato HTML.  
  
 Além de gerar um relatório manualmente, convém também o relatório seja gerado automaticamente em um agendamento diário ou por hora. Para fazer isso, no painel de tarefas, clique em **personalizar as configurações de relatório de integridade**e, em seguida, clique no **agendamento e Email** guia. O **agendamento** recurso está desativado por padrão, e você pode ativá-lo, selecionando o **gerar um relatório de integridade no horário agendado** caixa de seleção.  
  
###  <a name="BKMK_Customize"></a>Personalizar o conteúdo do relatório  
 O relatório de integridade contém o seguinte:  
  
-   **Os alertas essenciais e avisos** isso é consistente com os alertas essenciais e avisos que você vê no Visualizador de alerta no painel. Alertas de informações não estão incluídas no relatório de integridade.  
  
-   **Erros críticos nos logs de eventos** logs de aplicativos e serviços são verificados e os erros que são registrados nas últimas 24 horas serão apresentados na **detalhes** seção do relatório.  
  
-   **Backup do servidor** as informações sobre o último backup do servidor são apresentadas no **detalhes** seção do relatório.  
  
-   **Início automático serviços não está em execução** no momento em que o relatório é gerado, se um serviço de inicialização automática não está em execução, as informações sobre esse serviço serão listadas no **detalhes** seção do relatório.  
  
-   **Atualizações** você pode ver o status de atualização do servidor e todos os computadores cliente no **detalhes** seção.  
  
-   **Armazenamento** a lista de unidades e sua capacidade é apresentada no **detalhes** seção.  
  
 O relatório de integridade, primeiro exibir o **resumo**e para os itens com um ícone de erro vermelho ou um ícone de aviso amarelo, clique no **detalhes** link na mesma linha para exibir os detalhes sobre o item.  
  
 Se você não estiver interessado em alguns dos pontos de dados que estão incluídos no relatório por padrão, você pode personalizar o conteúdo do relatório clicando **personalizar as configurações de relatório de integridade** no painel de tarefas e, em seguida, clicar o **conteúdo**guia desmarque as caixas de seleção para o conteúdo que você don t deseja ver no relatório. Por exemplo, se você tem seu próprio plano de backup do servidor e don t quer ver os avisos sobre backups de servidor, você pode excluir backups de servidor do relatório de limpando a **backup do servidor** caixa de seleção.  
  
###  <a name="BKMK_emailreport"></a>O relatório de email  
 Fazer logon no painel para ler relatórios é ainda inconveniente para alguns administradores, especialmente se eles têm mais de um servidor para gerenciar. Com o recurso de email ativado, após um relatório é gerado, um email será enviado para uma lista de endereços de email especificados com o conteúdo do relatório. O administrador pode facilmente exibir esse relatório de qualquer dispositivo ou qualquer aplicativo cliente e certifique-se de que o servidor está em execução em seu estado melhor.  
  
 No **personalizar as configurações do relatório de integridade** caixa de diálogo, depois que você habilite o email, alterar as configurações de SMTP e especifica uma lista de destinatários de email, você notará que uma nova tarefa aparece no painel de tarefas: **o relatório de integridade de Email**. Para obter mais informações sobre as configurações de SMTP, consulte [configurar notificações de email para alertas](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email).  
  
 Você pode selecionar um relatório existente e, em seguida, clique em **o relatório de integridade de Email**. Você também pode gerar um novo relatório e enviá-lo automaticamente à caixa de entrada. Se você tiver configurado uma agenda para o relatório gerado automaticamente, o relatório será entregue automaticamente à caixa de entrada depois que estão sendo gerados todos os dias (ou a cada hora) conforme o agendamento.  
  
##  <a name="BKMK_View"></a>Exibir alertas usando o Visualizador de alerta  
 Esta seção discute como usar o painel ou barra inicial para abrir o Visualizador de alertas para exibir o status de integridade de todos os computadores da rede do servidor.  
  
#### <a name="to-open-the-alert-viewer-by-using-the-dashboard"></a>Para abrir o Visualizador de alerta usando o Dashboard  
  
1.  Abra o painel.  
  
2.  No painel de navegação, clique em qualquer um dos ícones alertas exibidos (crítico, aviso ou informativo). Isso abre o Visualizador de alerta.  
  
#### <a name="to-open-the-alert-viewer-from-the-launchpad"></a>Para abrir o Visualizador de alerta a partir da barra inicial  
  
1.  Em um computador que esteja conectado ao servidor, abra a barra inicial. Se solicitado, faça logon à barra inicial com seu nome de usuário e senha.  
  
2.  Clique em qualquer alertas ícones exibidos (críticos, de aviso e informativos) na parte inferior da barra inicial para abrir o Visualizador de alerta e siga as instruções no painel de detalhes do Visualizador de alerta para resolver o alerta.  
  
##  <a name="BKMK_Organize"></a>Organizar alertas no Visualizador de alerta  
 Você pode organizar alertas no Visualizador de alerta e sejam exibidas com base em sua gravidade (crítico, aviso ou informativo) ou com base no nome do computador.  
  
#### <a name="to-organize-alerts-in-the-alert-viewer"></a>Para organizar alertas no Visualizador de alerta  
  
1.  Abra o painel.  
  
2.  No painel de navegação, clique em qualquer um dos ícones alertas exibidos (crítico, aviso ou informativo). Isso abre o Visualizador de alerta.  
  
3.  Expanda o **organizar** lista suspensa e siga um destes procedimentos:  
  
    1.  Selecione **filtrar por computador**e clique no nome do computador para o qual você gostaria de ver os alertas. Isso exibe alertas no Visualizador de alerta somente para o computador selecionado.  
  
    2.  Selecione **filtrar por tipo de alerta**e clique no tipo alerta (crítico, aviso ou informativo) para o qual você gostaria de ver os alertas. Isso exibe somente o tipo de alerta selecionado no Visualizador de alerta.  
  
##  <a name="BKMK_Respond"></a>Responder a alertas  
 Quando você encontrar um alerta, você pode optar por fazer um destes procedimentos:  
  
-   [Resolver um alerta](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Resolve)  
  
-   [Ignorar um alerta](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Habilitar um alerta](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Excluir um alerta](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_4)  
  
###  <a name="BKMK_Resolve"></a>Resolver um alerta  
 Siga as instruções de resolução no Visualizador de alerta para resolver o alerta. Depois que um alerta for resolvido, ele ainda será exibido no Visualizador de alerta até que ela seja atualizada.  
  
###  <a name="BKMK_3"></a>Ignorar um alerta  
 Você pode optar por ignorar um alerta se você preferir responder a ele posteriormente. Quando você ignorar um alerta, ele ainda é listado no Visualizador de alerta, e, mas ela é desabilitada e esmaecida. Um alerta ignorado não está incluído na avaliação de integridade geral do computador. Para resolver um alerta ignorado, você primeiro precisa ativar o alerta.  
  
##### <a name="to-ignore-an-alert"></a>Para ignorar um alerta  
  
1.  No computador que esteja conectado ao servidor Windows Server Essentials, abra a barra inicial.  
  
2.  Na barra inicial, clique em qualquer um dos ícones exibidos alertas (críticos, de aviso e informativos). Isso abre o Visualizador de alerta.  
  
3.  No Visualizador de alerta, selecione o alerta que você quiser ignorar, e, em seguida, no **tarefas** seção, clique em **ignorar o alerta**.  
  
 Para responder a um alerta desabilitado, você precisará primeiro habilite o alerta.  
  
###  <a name="BKMK_5"></a>Habilitar um alerta  
 Você pode habilitar um alerta que você escolheu para ignorar anteriormente. Depois que o alerta é habilitado, você pode resolvê-lo ou excluí-lo conforme necessário. Um alerta é exibido como desabilitada quando marcado para ser ignorado. Quando você habilita um alerta que você tenha desativado anteriormente, ele se torna ativo e foi novamente incluído na avaliação de integridade geral de computadores.  
  
##### <a name="to-enable-an-alert"></a>Para habilitar um alerta  
  
1.  No computador que esteja conectado ao servidor, abra a barra inicial.  
  
2.  Na barra inicial, clique em qualquer alerta ícones exibidos (crítico, de aviso e informativas) para abrir o Visualizador de alerta.  
  
3.  No Visualizador de alerta, clique com botão direito o alerta que você deseja habilitar e, em seguida, clique em **habilitar o alerta**.  
  
###  <a name="BKMK_4"></a>Excluir um alerta  
 Você pode excluir um alerta se você não quiser resolver ou ignorá-la. Você pode usar o Visualizador de alerta na barra inicial para excluir alertas gerados para o seu computador. Se você excluir um alerta e o servidor detecta o problema novamente no próximo ciclo de avaliação de integridade de rede, ele gera um alerta de novo.  
  
##### <a name="to-delete-an-alert"></a>Para excluir um alerta  
  
1.  No computador que esteja conectado ao servidor, abra a barra inicial.  
  
2.  Na barra inicial, clique em qualquer alerta ícones exibidos (crítico, de aviso e informativas) para abrir o Visualizador de alerta.  
  
3.  No Visualizador de alerta, clique com botão direito o alerta que você deseja excluir e clique em **excluir o alerta**.  
  
##  <a name="BKMK_Email"></a>Configurar notificações de email para alertas  
 Você pode configurar seu servidor para notificá-lo por email sobre a ocorrência de alertas. As notificações de email para esses alertas contêm informações sobre os problemas de rede e suas etapas de resolução, que é idêntico às informações que são exibidas no Visualizador de alerta. Algumas das avaliações de integridade de rede são feitas por meio de programação.  
  
 Quando você configurar seu servidor para enviar notificações de alerta por email, uma notificação por email é enviada para os alertas que são encontradas durante a avaliação de integridade de rede. No entanto, nem todos os alertas que são relatados no Visualizador de alerta são relatados por email.  
  
 A cada 30 minutos, a tarefa de alerta Email avaliação é executada no servidor, que avalia a rede para alertas. Uma notificação por email é enviada se qualquer alerta que é definido para notificação por email ocorre. Um segundo email não é enviado se o alerta ainda está ativo no próximo ciclo de avaliação para evitar a saturação sua caixa de correio. No entanto, se um novo alerta for detectado dentro de um ciclo de avaliação de alerta futuras, uma notificação por email é enviado, que inclui os alertas de novo e anteriores.  
  
###  <a name="BKMK_list"></a>Alertas que resultam em notificações por email  
 Os seguintes alertas no Visualizador de alerta resultam em notificações por email ao configurar seu servidor para enviar notificações por email para alertas:  
  
-   Existem erros em um backup do computador cliente.  
  
-   O servidor foi reiniciado.  
  
-   Um ou mais serviços não estão em execução.  
  
-   O serviço de armazenamento de servidor do Windows não está executando.  
  
-   O período de avaliação é sobre.  
  
-   Ative agora.  
  
-   Servidor de origem está sendo desligado.  
  
-   OS resultados da verificação contenham erros.  
  
-   OS resultados da verificação contêm avisos.  
  
-   Um certificado não está disponível para o acesso em qualquer lugar.  
  
-   O certificado de acesso em qualquer local expirou.  
  
-   O roteador não está configurado corretamente.  
  
-   O servidor Web não está configurado corretamente.  
  
-   Serviços de área de trabalho remota não está configurado corretamente.  
  
-   O firewall não está configurado corretamente.  
  
-   O nome de domínio da Internet expirou.  
  
-   O nome de domínio da Internet não pode ser atualizado.  
  
-   Erro de licença: Seleção de confiança de floresta.  
  
-   Erro de licença: Seleção de controlador de domínio.  
  
-   Erro de licença: Verificação de função FSMO.  
  
-   Erro de licença: Imposição FSMO políticas.  
  
-   Erro de licença: Políticas de carregamento de imposição.  
  
-   Erro de licença: Serviços de domínio do Active Directory.  
  
-   Sua assinatura do Office 365 expirou.  
  
-   Autenticação do Office 365 não teve êxito.  
  
-   A política de senha não está correta.  
  
-   O serviço de sincronização de senha não é possível sincronizar uma senha de usuário com o Office 365.  
  
-   Altere sua senha do Windows.  
  
-   Sua senha do Office 365 não é o mesmo que a senha do Windows.  
  
-   Não consegue se conectar ao servidor do Exchange.  
  
-   Microsoft Exchange Server tem um problema.  
  
-   Um ou mais discos rígidos no Backup do servidor não estão conectados.  
  
-   O disco rígido de backup não tem espaço livre suficiente para o Backup do servidor.  
  
-   Backup do servidor não teve êxito porque um instantâneo de unidade não pôde ser realizado.  
  
-   Um backup agendado não foi concluída com êxito.  
  
-   Uma ou mais pastas de servidor predefinidos estão ausentes.  
  
-   Espaço livre é baixo em um ou mais discos rígidos de servidor.  
  
-   O gravador de VSS para o serviço de armazenamento não está em execução.  
  
-   Capacidade de armazenamento de baixa em discos rígidos.  
  
-   Uma ou mais unidades não estão funcionando e estão offline.  
  
###  <a name="BKMK_SMTP"></a>Configurando o SMTP no seu servidor para enviar notificações de alerta por email no Windows Server Essentials  
 Esta seção discute como configurar seu servidor para enviar notificações por email para alertas.  
  
> [!NOTE]
>  Você pode baixar o suplemento de relatório de integridade do Windows Server Essentials do [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?LinkId=266342).  
  
##### <a name="to-set-up-email-notification-for-alerts"></a>Para configurar a notificação por email para alertas  
  
1.  Do **painel**, abra o **Alert Viewer**.  
  
2.  No **Alert Viewer**, clique em **configurar notificações de alerta email**.  
  
3.  No **configurar uma notificação de email para alertas** janela, clique em **habilitar**.  
  
4.  No **configurações de SMTP** janela, faça o seguinte:  
  
    1.  Para **de endereço de email**, tipo avisa se o endereço de email que você deseja usar para enviar o email do. Este endereço de email será exibido como o endereço do remetente s na notificação de alerta.  
  
    2.  Para **nome do servidor SMTP**, além do **de endereço de email** texto, digite o nome do servidor SMTP que você especificou na etapa 4a. (Consulte a tabela 1 para obter uma lista de alguns nomes de servidor SMTP).  
  
    3.  Para **porta SMTP**, digite o número da porta que é usado pelo servidor SMTP para enviar e receber emails. (Consulte a tabela 1 para os números de porta são usados por alguns dos servidores SMTP).  
  
    4.  Selecione **este servidor exige uma conexão segura (SSL)** se o servidor de SMTP usar SSL (consulte a tabela 1).  
  
    5.  Selecione **este servidor exige autenticação** se o servidor de SMTP requer um informações de nome e a senha do usuário (consulte a tabela 1). Se você marcar essa caixa de seleção, digite o nome de usuário e senha do endereço de email que você inseriu no **de endereço de email** campo na etapa 4a e clique em **Okey**.  
  
    > [!NOTE]
    >  Você pode obter as informações sobre o nome do servidor SMTP, o número da porta e o uso SSL do seu provedor de serviços de Internet.  
  
     **A tabela 1** nomes de servidor de exemplos de SMTP, autenticação e requisitos de criptografia de SSL e números de porta  
  
    |Servidor SMTP|SSL necessária|Autenticação necessária|Número da porta|Nome de nome/logon de conta|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |SMTP.Gmail.com|Sim|Sim|587|Fornece o endereço de email completo com nome de domínio e a senha para autenticação.|  
    |SMTP.Live.com|Sim|Sim|587|Fornece o endereço de email completo com nome de domínio e a senha para autenticação.|  
    |SMTP.Comcast.NET|Sim|Não|587|Fornece o endereço de email completo com nome de domínio e a senha para autenticação.|  
    |Yahoo.com|Não|Sim|25|Forneça apenas o endereço de email sem um nome de domínio para o nome do usuário.|  
  
5.  Em **configurar uma notificação para alertas**, para **destinatários de Email**, digite os endereços de email das pessoas que você gostaria de receber notificações de alerta por email. Certifique-se de que você separe cada endereço de email com um ponto e vírgula (;).  
  
6.  Para verificar se você configurou seu servidor de SMTP configurações corretamente para enviar notificações por email para alertas, clique em **aplicar e enviar email**.  
  
    > [!NOTE]
    >  Quando você clica em **aplicar e enviar email**, normalmente você receberá uma notificação de email de exemplo com nenhum alertas de integridade listadas. No entanto, se um alerta de saúde que esteja configurado para enviar uma notificação por email é identificado durante esse processo de teste, esse alerta está incluída no email de teste.  
  
### <a name="configuring-smtp-on-your-server-to-send-health-reports-in-windows-server-essentials"></a>Configurando o SMTP no seu servidor para enviar relatórios de integridade no Windows Server Essentials  
 Esta seção discute como configurar as configurações de SMTP para seu servidor para que você possa receber relatórios de integridade por email.  
  
> [!NOTE]
>  Por padrão, o suplemento de relatório de integridade é integrado ao Windows Server Essentials ou o Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada, e os relatórios de integridade são exibidos na **relatórios de integridade** guia do painel s **Home** página.  
  
##### <a name="to-set-up-email-notification-for-health-reports"></a>Para configurar a notificação por email para relatórios de integridade  
  
1.  Do **painel**, clique no **relatórios** guia.  
  
2.  No **tarefas de relatório de integridade** painel de tarefas, clique em **personalizar as configurações de relatório de integridade**.  
  
3.  No **personalizar as configurações do relatório de integridade** janela, clique no **agendamento e Email** guia.  
  
4.  No **agendamento e Email** guia, o **Email** seção, faça o seguinte:  
  
    1.  Clique em **habilitar**e digite o endereço de email que você deseja usar para enviar a integridade relatos dos. Este endereço de email será exibido como o endereço do remetente s nos relatórios de saúde que são enviados por email.  
  
        1.  Para **nome do servidor SMTP**, digite o nome do servidor SMTP. (Consulte a tabela 1 para obter uma lista de alguns nomes de servidor SMTP).  
  
        2.  Para **porta SMTP**, digite o número da porta que é usado pelo servidor SMTP para enviar e receber emails. (Consulte a tabela 1 para os números de porta são usados por alguns dos servidores SMTP).  
  
        3.  Selecione **este servidor exige uma conexão segura (SSL)** se o servidor de SMTP usar SSL (consulte a tabela 1).  
  
        4.  Selecione **este servidor exige autenticação** se o servidor de SMTP requer um informações de nome e a senha do usuário (consulte a tabela 1). Se você marcar essa caixa de seleção, digite o nome de usuário e senha do endereço de email que você inseriu no **de endereço de email** campo na etapa 4a e clique em **Okey**.  
  
            > [!NOTE]
            >  Você pode obter as informações sobre o nome do servidor SMTP, o número da porta e o uso SSL do seu provedor de serviços de Internet.  
  
            > [!NOTE]
            >  Microsoft recomenda veementemente que você use SSL porque o relatório pode conter o status do servidor que poderiam ser usado por pessoas mal-intencionadas para detectar vulnerabilidades (por exemplo: faltando a atualização do windows). Habilitar SSL criptografar os dados em trânsito e reduzir o risco de expor a vulnerabilidade do servidor.  
  
5.  Depois de habilitar o email, o **configurações de SMTP alteração** link é exibido. Também é uma nova tarefa, **o relatório de integridade de Email**, é exibida no **tarefas de relatório de integridade**.  
  
     **A tabela 1** nomes de servidor de exemplos de SMTP, autenticação e requisitos de criptografia de SSL e números de porta  
  
    |Servidor SMTP|SSL necessária|Autenticação necessária|Número da porta|Nome de nome/logon de conta|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |SMTP.Gmail.com|Sim|Sim|587|Fornece o endereço de email completo com nome de domínio e a senha para autenticação.|  
    |SMTP.Live.com|Sim|Sim|587|Fornece o endereço de email completo com nome de domínio e a senha para autenticação.|  
    |SMTP.Comcast.NET|Sim|Não|587|Fornece o endereço de email completo com nome de domínio e a senha para autenticação.|  
    |Yahoo.com|Não|Sim|25|Forneça apenas o endereço de email sem um nome de domínio para o nome do usuário.|  
  
6.  Em **personalizar as configurações do relatório de integridade**, para **enviar automaticamente o relatório de integridade para os seguintes destinatários de email:**, digite os endereços de email das pessoas que você gostaria de receber relatórios de integridade por email. Certifique-se de que você separe cada endereço de email com um ponto e vírgula (;).  
  
7.  Para verificar se você configurou seu servidor de SMTP configurações corretamente para enviar relatórios de integridade por email, na guia Helath relatório no painel, selecione um relatório em clique em **o relatório de integridade de Email** do painel de tarefas.  
  
##  <a name="BKMK_Potential"></a>Possíveis alertas de computador  
 Esta seção discute o entendimento e o gerenciamento de alertas que são específicas ao seu computador que esteja conectado ao servidor e que aparecem na barra inicial do seu computador.  
  
 A tabela a seguir lista alguns dos alertas de computador que podem ser gerados e exibidos no Visualizador do alerta se eles forem aplicáveis ao seu computador.  
  
|Título do alerta|Resolução e o impacto de alerta|  
|-----------------|---------------------------------|  
|O status atual do Firewall de rede oferece proteção reduzida para esse computador.|Pessoas não autorizadas ou software pode ser capaz de acessar este computador se o Firewall do Windows não está ativado.|  
|Proteção contra vírus está desativada, não instalado ou não atualizados.|Os dados em seu computador estão em risco se o **proteção contra vírus** configuração de segurança está desativada ou não atualizada. [Para proteger seu computador](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect), siga as etapas indicadas.|  
|Spyware e proteção de software indesejado está desativada, não instalado ou não atualizados.|Os dados em seu computador estão em risco se o **Spyware e indesejado proteção de software** está desativado ou não atualizados. [Para proteger seu computador](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect), siga as etapas indicadas.|  
|Windows Update estiver desativado.|Você não poderá aproveitar as funcionalidades novas e corrigida das atualizações, a menos que o Windows Update está ativado. Para ativar o Windows Update, no Visualizador de alerta, clique em **abrir o Windows Update**.<br /><br /> Se o **abrir o Windows Update** tarefa não for exibida, você não está conectado ao computador onde o alerta foi gerado. Você deve estar conectado ao computador no qual o alerta foi gerado para executar essa tarefa no Visualizador de alerta.|  
|Atualizações importantes devem ser instaladas.|Você não poderá aproveitar as funcionalidades novas e corrigida das atualizações, a menos que o Windows Update está ativado. Para ativar o Windows Update, no Visualizador de alerta, clique em **abrir o Windows Update**.<br /><br /> Se o **abrir o Windows Update** tarefa não for exibida, você não está conectado ao computador onde o alerta foi gerado. Você deve estar conectado ao computador no qual o alerta foi gerado para executar essa tarefa no Visualizador de alerta.|  
|Reinicie o computador para aplicar as atualizações.|Você não poderá aproveitar as funcionalidades novas e corrigida das atualizações até que eles sejam aplicados. Salve todos os seus dados e reinicie o computador para aplicar as atualizações.|  
|Espaço livre estiver fraca em discos rígidos.|Se o espaço não forem disponibilizado, você não poderá salvar informações adicionais. Para aumentar o espaço livre no computador, considere as seguintes opções:<br /><br /> -Adicione um novo disco rígido.<br /><br /> -Executar **limpeza de disco** para remover arquivos temporários e antigos.<br /><br /> -Mova seus arquivos para uma pasta compartilhada em outro computador.<br /><br /> -Arquivar os arquivos em uma mídia removível, como um CD, DVD ou um disco rígido externo.|  
|O **histórico de arquivos** agente no servidor não está configurado corretamente para executar neste computador.|Não não possível criar backups de histórico de arquivos.|  
|Um ou mais serviços não estão em execução.||  
|Altere sua senha do Windows.||  
|Sua senha do Microsoft Office 365 não é o mesmo que a senha do Windows.||  
  
###  <a name="BKMK_Protect"></a>Para proteger seu computador  
  
1.  Abrir a Central de segurança.  
  
2.  Determine o status da proteção antivírus instalado.  
  
3.  Execute uma das seguintes tarefas dependendo do status de proteção:  
  
    -   Se não estiver habilitado, ativá-lo.  
  
    -   Se não for atualizado, conclua o processo para atualizar as assinaturas.  
  
    -   Se a proteção contra vírus não estiver instalada, considere a possibilidade de instalá-lo.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Usar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
