---
title: Preparar seu servidor de origem do Windows Server Essentials migration1
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5861ae9-77cb-4d37-b4c5-8f0757213385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 929c7506c78667646e429c4f28df7e5642c575ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="prepare-your-source-server-for-windows-server-essentials-migration1"></a>Preparar seu servidor de origem do Windows Server Essentials migration1

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Conclua as seguintes etapas preliminares para garantir que as configurações e os dados em seu servidor de origem migram com êxito para o servidor de destino.  
  
#### <a name="to-prepare-for-migration"></a>Para preparar para a migração  
  

1.  [Faça backup do servidor de origem](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Instale os service packs mais recentes](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Avaliar a integridade do servidor de origem](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [Execute a ferramenta de preparação de migração no servidor de origem](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [Criar um plano para migrar aplicativos de linha de negócios](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

1.  [Faça backup do servidor de origem](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Instale os service packs mais recentes](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Avaliar a integridade do servidor de origem](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [Execute a ferramenta de preparação de migração no servidor de origem](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [Criar um plano para migrar aplicativos de linha de negócios](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

  
###  <a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>Faça backup do servidor de origem  
 Faça backup do servidor de origem antes de começar o processo de migração. Fazendo um backup ajuda a proteger seus dados contra perda acidental se ocorrer um erro irrecuperável durante a migração.  
  
##### <a name="to-back-up-the-source-server"></a>Para fazer backup do servidor de origem  
  
1.  Faça um backup completo do servidor de origem. Para obter mais informações sobre como fazer backup Windows Small Business Server 2011 Essentials, consulte [Saiba mais sobre como configurar o backup do servidor](https://technet.microsoft.com/library/server-backup-support-1.aspx).  
  
2.  Verifique se o backup foi executado com êxito. Para testar a integridade do backup, selecione arquivos aleatórios do backup, restaurá-los em um local alternativo e confirme que os arquivos restaurados são iguais aos arquivos originais.  
  
###  <a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>Instale os service packs mais recentes  
 Você deve instalar os service packs e atualizações mais recentes no servidor de origem antes da migração.  
  
###  <a name="BKMK_UseWindowsBestPracticeAnalyzer"></a>Avaliar a integridade do servidor de origem  
 É importante avaliar a integridade do seu servidor de origem antes de começar a migração. Use os procedimentos a seguir para garantir que as atualizações sejam atuais, para gerar um relatório de integridade do sistema e executar o Windows Server soluções melhor prática BPA (Analyzer).  
  
#### <a name="download-and-install-critical-and-security-updates"></a>Download e instalação crítica e segurança atualizações  
 Instalando críticas e atualizações de segurança no servidor de origem ajuda a garantir que sua migração seja bem-sucedida e ajuda a proteger sua rede durante o processo de migração.  
  
###### <a name="to-check-for-the-latest-updates"></a>Para verificar as atualizações mais recentes  
  
1.  No servidor de origem, clique em **iniciar**, clique em **todos os programas**e clique em **Windows Update**.  
  
2.  Clique em **verificar se há atualizações**.  
  
3.  Se forem encontradas atualizações, clique em **instalar atualizações**.  
  
#### <a name="check-the-alert-viewer-for-critical-errors"></a>Verifique o Visualizador de alerta para erros críticos  
 Você pode verificar o Visualizador de alerta no painel para quaisquer erros críticos.  
  
#### <a name="run-the-windows-server-solutions-best-practices-analyzer"></a>Execute o analisador de práticas recomendadas do Windows Server Solutions  
 Você pode executar o Windows Server soluções BPA Best Practices Analyzer () para verificar que não há nenhum problema no servidor, rede ou domínio antes de começar o processo de migração. A BPA coleta informações de configuração das seguintes fontes:  
  
-   Active Directory® Windows Management Instrumentation (WMI)  
  
-   O registro  
  
-   La Internet Information Services (IIS)  
  
###### <a name="to-use-the-windows-server-solutions-bpa-to-analyze-your-source-server"></a>Usar o Windows Server Solutions BPA para analisar seu servidor de origem  
  
1.  Baixe e instale o [analisador de práticas recomendadas do Windows Server Solutions](https://www.microsoft.com/en-us/download/details.aspx?id=15556) no Microsoft Download Center.  
  
2.  Depois que o download for concluído, clique em **iniciar**, aponte para **todos os programas**e clique em **ferramenta do analisador de práticas recomendadas do SBS**.  
  
    > [!NOTE]
    >  Verificar se há atualizações antes de verificar o servidor.  
  
3.  No painel de navegação, clique em **iniciar uma verificação**.  
  
4.  No painel de detalhes, digite o rótulo da verificação e clique em **inicie a verificação**. O rótulo da verificação é o nome do relatório de digitalização, por exemplo, **SBS BPA Scan 1 de julho de 2012**.  
  
5.  Depois que a verificação termina, clique em **visualizar um relatório dessa verificação de práticas recomendadas**.  
  
 Depois de coletar informações sobre a configuração do servidor, o Windows Server Solutions BPA verifica se as informações estão corretas e, em seguida, apresenta os administradores com uma lista de informações e problemas classificados por gravidade. A lista descreve cada problema e fornece uma solução de recomendação ou possíveis. Existem três tipos de relatório:  
  
|Tipo de relatório|Descrição|  
|-----------------|-----------------|  
|Relatórios de lista|Exibe relatórios em uma lista unidimensional.|  
|Relatórios de árvore|Exibe relatórios em uma lista hierárquica.|  
|Outros relatórios|Exibe os relatórios como um Log de tempo de execução.|  
  
 Para exibir a descrição e as soluções para um problema, clique no problema no relatório. Nem todos os problemas relatados pelo BPA do Windows SBS 2011 Essentials afetam a migração, mas você deve resolver como muitos dos problemas quanto possível para garantir que a migração foi bem-sucedida.  
  
####  <a name="BKMK_SynchronizeTheSourceServerTimeWithAnExternalTimeSource"></a>Sincronizar a hora do servidor de origem com uma fonte de horário externo  
 O tempo no servidor de origem deve ser definido como dentro de cinco minutos de tempo no servidor de destino, e a data e o fuso horário devem ser o mesmo em ambos os servidores. Se o servidor de origem está executando em uma máquina virtual, a data, hora e fuso horário no servidor host devem corresponder do servidor de origem e o servidor de destino. Para ajudar a garantir que o Windows Server Essentials é instalado com êxito, você deve sincronizar a hora do servidor de origem para o servidor de rede NTP (Time Protocol) na Internet.  
  
###### <a name="to-synchronize-the-source-server-time-with-the-ntp-server"></a>Para sincronizar a hora do servidor de origem com o servidor NTP  
  
1.  Entrar servidor de origem com uma conta de administrador do domínio e a senha.  
  
2.  Clique em **iniciar**, clique em **executar**, tipo **cmd** na caixa de texto e pressione ENTER.  
  
3.  No prompt de comando, digite w32tm /config /syncfromflags: / confiável: nenhuma /update e pressione ENTER.  
  
4.  No prompt de comando, digite net stop w32time e pressione ENTER.  
  
5.  No prompt de comando, digite net start w32time e pressione ENTER.  
  
> [!IMPORTANT]
>  Durante a instalação do Windows Server Essentials, você tem uma oportunidade de verificar a hora no servidor de destino e alterá-lo, se necessário. Certifique-se de que o tempo é dentro de cinco minutos do tempo que é definido no servidor de origem. Quando a instalação é concluída, o servidor de destino sincronizará com o NTP. Todos os computadores ingressados em domínio, incluindo o servidor de origem, sincronizam com o servidor de destino, que assume a função de domínio primário mestre emulador do controlador (PDC).  
  
###  <a name="BKMK_MPT"></a>Execute a ferramenta de preparação de migração no servidor de origem  
 Você não pode executar uma instalação do modo de migração sem primeiro executar a ferramenta de preparação de migração no servidor de origem. Essa ferramenta é desenvolvida para preparar seu servidor de origem e o domínio devem ser migrados para o Windows Server Essentials.  
  
> [!IMPORTANT]
>  Faça backup do servidor de origem antes de executar a ferramenta de preparação de migração. Todas as alterações que faz com que a ferramenta de preparação de migração para o esquema forem irreversíveis. Se você tiver problemas durante a migração, a única maneira de retornar o servidor de origem para o estado em que se encontrava antes de você executar a ferramenta é restaurar o servidor a partir de um backup do sistema.  
  
 Para executar a ferramenta de preparação de migração, você deve ser um membro do grupo Administradores de empresa, o grupo Administradores de esquema e do grupo Admins. do domínio.  
  
##### <a name="to-verify-that-you-have-the-appropriate-permissions-to-run-the-tool-on-the-source-server"></a>Para verificar se você tem as permissões adequadas para executá-la no servidor de origem  
  
1.  No servidor de origem, clique em **iniciar**, clique em **ferramentas administrativas**e clique em **usuários e Active Directory computadores**.  
  
2.  Na árvore de console, clique para expandir seu domínio e, em seguida, clique em **usuários**.  
  
3.  Clique com botão direito a conta de administrador que você está usando para a migração e, em seguida, clique em **propriedades**.  
  
4.  Clique no **membro de** guia e, em seguida, verifique se que Admins. do domínio, administradores de esquema e administradores corporativos estão listados no **membro do** caixa de texto.  
  
5.  Se os grupos não estiverem listados, clique em **adicionar**, e, em seguida, adicione cada grupo que não esteja listado.  
  
    > [!NOTE]
    >  -   Se o serviço de logon de rede não é iniciado, você pode receber um erro de permissão.  
    > -   Você deve fazer logoff e logon novamente no servidor para que as alterações entrem em vigor.  
  
     Você pode usar a versão mais recente do Windows Update Agent para garantir que o processo de atualização do servidor funcione corretamente.  
  
 Você pode usar a versão mais recente do Windows Update Agent para garantir que o processo de atualização do servidor funcione corretamente.  
  
 Antes de instalar o Windows Update Agent no servidor de origem, primeiro você deve instalar o Windows PowerShell 2.0 e o Microsoft Baseline Configuration Analyzer 2.0.  
  
-   Para baixar e instalar o Windows PowerShell 2.0, veja [968929 do artigo](https://go.microsoft.com/fwlink/p/?LinkId=241483) na Base de Conhecimento Microsoft.  
  
-   Para baixar e instalar o Microsoft Baseline Configuration Analyzer 2.0, veja o [Microsoft Baseline Configuration Analyzer 2.0](https://go.microsoft.com/fwlink/p/?LinkId=241484) no Microsoft Download Center.  
  
-   Para baixar e instalar a versão mais recente do agente do Windows Update, consulte [949104 do artigo](https://go.microsoft.com/fwlink/p/?LinkId=237493) na Base de Conhecimento Microsoft.  
  
##### <a name="to-install-and-run-the-migration-preparation-tool-on-the-source-server"></a>Instalar e executar a ferramenta de preparação de migração no servidor de origem  
  
1.  Insira DVD1 do Windows Server Essentials na unidade de DVD no servidor de origem.  
  
2.  Abra o Windows Explorer, navegue até o **\support\tools** pasta do DVD e clique duas vezes na **sourcetool.msi** arquivo.  
  
    > [!NOTE]
    >  -   Se a ferramenta de preparação de migração já estiver instalada no servidor, execute a ferramenta do **iniciar** menu.  
    > -   Para garantir que você está preparado para a melhor experiência de migração possíveis, é recomendável que você optar por instalar a atualização mais recente.  
  
     O assistente instala a ferramenta de preparação de migração no servidor de origem. Quando a instalação for concluída, a ferramenta de preparação de migração é executada automaticamente e instala as atualizações mais recentes.  
  
3.  Na ferramenta de preparação de migração, selecione **eu tenha um backup e estou pronto para prosseguir**e clique em **próxima**.  
  
    > [!WARNING]
    >  Se você receber uma mensagem de erro referentes à instalação de um hotfix, consulte o método 2: renomeie a pasta Catroot2 em [822798 do artigo](https://go.microsoft.com/FWLink/p/?LinkID=118672) na Base de Conhecimento Microsoft.  
  
     A ferramenta de preparação da migração prepara o domínio de origem para migração ao estender o esquema do Active Directory. Depois que a tarefa é concluída, clique em **próxima** para continuar.  
  
4.  Após preparar o domínio de origem, a ferramenta de preparação da migração verifica o servidor de origem para identificar os dois tipos de problemas em potencial.  
  
    -   **Erros** problemas encontrados no servidor de origem que podem bloquear a migração ou fazer com que a migração falhar. Siga as instruções na mensagem de erro para corrigir os problemas e clique em **verificar novamente**.  
  
    -   **Avisos** problemas encontrados no servidor de origem que podem causar problemas funcionais durante a migração. É altamente recomendável que você siga as instruções na mensagem de erro para corrigir problemas antes de prosseguir com a migração.  
  
     Depois de corrigir ou reconhece todos os problemas, clique em **próxima**.  
  
5.  Na ferramenta de preparação de migração, clique em **concluir**.  
  
6.  Quando a ferramenta de preparação de migração é concluído, você pode ser solicitado a reiniciar o servidor de origem antes de começar a migrar para o Windows Server Essentials.  
  
> [!NOTE]
>  Você deve concluir uma execução bem-sucedida da ferramenta de preparação de migração no servidor de origem dentro de duas semanas de instalação do Windows Server Essentials no servidor de destino. Caso contrário, a instalação do Windows Server Essentials no servidor de destino será bloqueada. Se isso ocorrer, você deve executar a ferramenta de preparação de migração no servidor de origem novamente.  
  
###  <a name="BKMK_PlanToMigrateLineOfBusinessApplications"></a>Criar um plano para migrar aplicativos de linha de negócios  
 Um aplicativo de linha de negócios (LOB) é um aplicativo de computador críticos que são vital para a execução de uma empresa. Os aplicativos LOB incluem gerenciamento de contabilidade, cadeia de suprimento e aplicativos de planejamento de recursos.  
  
 Quando você planeja migrar seus aplicativos LOB, consulte os provedores de aplicativos LOB para determinar o método apropriado para cada aplicativo a migração. Você também deve localizar a mídia que é usada para instalar os aplicativos LOB no servidor de destino.  
  
> [!NOTE]
>  Se você usou o SDK do Windows Small Business Server 2011 Essentials para desenvolver uma integridade do sistema que você personalizou ou alerta suplemento e você quiser continuar a usar o suplemento com o Windows Server Essentials, você também deve atualizar o suplemento e implantá-la no servidor de destino.  
  
