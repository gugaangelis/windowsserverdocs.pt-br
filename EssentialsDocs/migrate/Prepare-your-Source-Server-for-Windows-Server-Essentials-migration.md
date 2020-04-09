---
title: Preparar o servidor de origem para o Windows Server Essentials migration1
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: f5861ae9-77cb-4d37-b4c5-8f0757213385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d7a718e9e84866b6a1f626499b7e2bec58de498f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852399"
---
# <a name="prepare-your-source-server-for-windows-server-essentials-migration1"></a>Preparar o servidor de origem para o Windows Server Essentials migration1

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Conclua as etapas preliminares a seguir para garantir que as configurações e dados do servidor de origem sejam migradas com êxito para o servidor de destino.  
  
#### <a name="to-prepare-for-migration"></a>Para se preparar para a migração  
  

1.  [Fazer backup do servidor de origem](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Instalar os service packs mais recentes](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Avaliar a integridade do servidor de origem](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [Executar a ferramenta de preparação de migração no servidor de origem](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [Criar um plano para migrar aplicativos de linha de negócios](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

1.  [Fazer backup do servidor de origem](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Instalar os service packs mais recentes](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Avaliar a integridade do servidor de origem](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [Executar a ferramenta de preparação de migração no servidor de origem](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [Criar um plano para migrar aplicativos de linha de negócios](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

  
###  <a name="back-up-your-source-server"></a><a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>Fazer backup do servidor de origem  
 Faça o backup do Servidor de Origem antes de iniciar o processo migração. Isso ajuda a proteger seus dados de perda acidental caso ocorra um erro irrecuperável durante a migração.  
  
##### <a name="to-back-up-the-source-server"></a>Para fazer backup do Servidor de Origem  
  
1.  Faça um backup completo do Servidor de Origem. Para obter mais informações sobre como fazer backup do Windows Small Business Server 2011 Essentials, consulte [saiba mais sobre como configurar o backup do servidor](https://technet.microsoft.com/library/server-backup-support-1.aspx).  
  
2.  Verifique se o backup foi executado com êxito. Para testar a integridade do backup, selecione arquivos aleatórios do backup, restaure-os em um local alternativo e verifique se os arquivos restaurados são iguais aos arquivos originais.  
  
###  <a name="install-the-most-recent-service-packs"></a><a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>Instalar os service packs mais recentes  
 Você deve instalar as últimas atualizações e service packs no Servidor de Origem antes da migração.  
  
###  <a name="evaluate-the-health-of-the-source-server"></a><a name="BKMK_UseWindowsBestPracticeAnalyzer"></a>Avaliar a integridade do servidor de origem  
 É importante avaliar a integridade do servidor de origem antes de começar a migração. Use os procedimentos a seguir para garantir que as atualizações sejam atuais, para gerar um relatório de integridade do sistema e executar o Analisador de Práticas Recomendadas do Windows Server Solutions.  
  
#### <a name="download-and-install-critical-and-security-updates"></a>Download e instalação de atualizações críticas e de segurança  
 Instalar atualizações críticas e de segurança no servidor de origem ajuda a garantir que a migração seja bem-sucedida e a proteger sua rede durante o processo de migração.  
  
###### <a name="to-check-for-the-latest-updates"></a>Para verificar as atualizações mais recentes  
  
1.  No servidor de origem, clique em **Iniciar**, clique em **Todos os Programas** e clique em **Windows Update**.  
  
2.  Clique em **Procurar atualizações**.  
  
3.  Se forem encontradas atualizações, clique em **Instalar Atualizações**.  
  
#### <a name="check-the-alert-viewer-for-critical-errors"></a>Verifique o Visualizador de alertas de erros críticos  
 Você pode verificar o Visualizador de alertas no Painel para erros críticos.  
  
#### <a name="run-the-windows-server-solutions-best-practices-analyzer"></a>Execute o Analisador de Práticas Recomendadas do Windows Server Solutions  
 Você pode executar o Analisador de Práticas Recomendadas (BPA) do Windows Server Solutions para verificar se não há problemas no servidor, rede ou domínio antes de iniciar o processo de migração. O BPA coleta informações de configuração das seguintes origens:  
  
-   Active Directory&reg; Instrumentação de Gerenciamento do Windows (WMI)  
  
-   O registro  
  
-   A metabase do IIS (Serviços de Informações da Internet)  
  
###### <a name="to-use-the-windows-server-solutions-bpa-to-analyze-your-source-server"></a>Para usar o Windows Server Solutions BPA para analisar o servidor de origem  
  
1. Baixe e instale o [analisador de práticas recomendadas de soluções do Windows Server](https://www.microsoft.com/download/details.aspx?id=15556) no centro de download da Microsoft.  
  
2. Após a conclusão do download, clique em **Iniciar**, clique em **Todos os Programas** e clique em **Ferramenta Analisadora de Práticas Recomendadas do SBS**.  
  
   > [!NOTE]
   >  Verifique se há atualizações antes de examinar o servidor.  
  
3. No painel de navegação, clique na opção de **iniciar uma varredura**.  
  
4. No painel de detalhes, digite o rótulo da varredura e, em seguida, clique em **Iniciar a varredura**. O rótulo de varredura corresponde ao nome do relatório de varredura, por exemplo, **Varredura BPA SBS 1Jul2012**.  
  
5. Quando a varredura terminar, clique na opção de **exibição de relatório da varredura de práticas recomendadas**.  
  
   Depois de coletar informações sobre a configuração do servidor, o Windows Server Solutions BPA verifica que as informações estão corretas e, em seguida, apresenta os administradores com uma lista de problemas classificados por gravidade e informações. A lista descreve cada problema e fornece uma recomendação ou solução possível. Há três tipos de relatórios disponíveis:  
  
|Tipo de relatório|Descrição|  
|-----------------|-----------------|  
|Relatórios de lista|Exibe relatórios em uma lista unidimensional.|  
|Relatórios de árvore|Exibe relatórios em uma lista hierárquica.|  
|Outros relatórios|Exibe relatórios como um log de tempo de execução.|  
  
 Para exibir a descrição e as soluções de um problema, clique no problema no relatório. Nem todos os problemas relatados pelo BPA do Windows SBS 2011 Essentials afetam a migração, mas você deve resolver o máximo possível de problemas para garantir que a migração seja bem-sucedida.  
  
####  <a name="synchronize-the-source-server-time-with-an-external-time-source"></a><a name="BKMK_SynchronizeTheSourceServerTimeWithAnExternalTimeSource"></a>Sincronizar a hora do servidor de origem com uma fonte de tempo externa  
 A hora no Servidor de Origem pode ter uma diferença de, no máximo, cinco minutos da hora no Servidor de Destino e a data e o fuso horário devem ser os mesmos em ambos os servidores. Se o Servidor de Origem estiver sendo executado em uma máquina virtual, a data, a hora e o fuso horário no servidor host devem ser os mesmos que no Servidor de Origem e no Servidor de Destino. Para ajudar a garantir que o Windows Server Essentials seja instalado com êxito, você deve sincronizar a hora do servidor de origem com o servidor protocolo NTP (NTP) na Internet.  
  
###### <a name="to-synchronize-the-source-server-time-with-the-ntp-server"></a>Para sincronizar a hora do servidor de origem com a do servidor NTP  
  
1.  Faça logon no servidor de origem com uma conta de administrador de domínio e senha.  
  
2.  Clique em **Iniciar**, em **Executar**, digite **cmd** na caixa de texto e pressione ENTER.  
  
3.  No prompt de comando, digite w32tm /config /syncfromflags:domhier /reliable:no /update e pressione ENTER.  
  
4.  No prompt de comando, digite net stop w32time e pressione ENTER.  
  
5.  No prompt de comando, digite net start w32time e pressione ENTER.  
  
> [!IMPORTANT]
>  Durante a instalação do Windows Server Essentials, você tem a oportunidade de verificar a hora no servidor de destino e alterá-la, se necessário. Certifique-se de que a hora esteja no máximo cinco minutos à frente da hora definida no servidor de origem. Quando a instalação for concluída, o servidor de destino será sincronizado com o NTP. Todos os computadores que fazem parte do domínio, incluindo o servidor de origem, são sincronizados com o servidor de destino, que assume a função de mestre emulador PDC (controlador de domínio primário).  
  
###  <a name="run-the-migration-preparation-tool-on-the-source-server"></a><a name="BKMK_MPT"></a>Executar a ferramenta de preparação de migração no servidor de origem  
 Você não pode realizar uma instalação de modo de migração sem primeiro executar a ferramenta de preparação da migração no servidor de origem. Essa ferramenta foi projetada para preparar o servidor de origem e o domínio a serem migrados para o Windows Server Essentials.  
  
> [!IMPORTANT]
>  Faça backup do Servidor de Origem antes de executar a Ferramenta de Preparação da Migração. Todas as mudanças feitas pela Ferramenta de Preparação da Migração ao esquema são irreversíveis. Se tiver problemas durante a migração, a única forma de retornar o Servidor de Origem ao estado anterior à execução da ferramenta é restaurar o backup do sistema.  
  
 Para executar a Ferramenta de Preparação da Migração, você deve ser membro do grupo Administradores de Empresa, Administradores de Esquema e Administradores de Domínio.  
  
##### <a name="to-verify-that-you-have-the-appropriate-permissions-to-run-the-tool-on-the-source-server"></a>Para verificar se você tem as permissões apropriadas para executar a ferramenta no Servidor de Origem  
  
1. No Servidor de Origem, clique em **Iniciar**, **Ferramentas Administrativas** e clique em **Usuários e Computadores do Active Directory**.  
  
2. Na árvore do console, clique para expandir seu domínio e clique em **Usuários**.  
  
3. Clique com o botão direito do mouse na conta de usuário que você está usando para a migração e, em seguida, clique em **Propriedades**.  
  
4. Clique na guia **Membro de** e verifique se Administradores de Empresa, Administradores de Esquema e Administradores de Domínio estão listados na caixa de texto **Membro de**.  
  
5. Se esses grupos não estiver listados, clique em **Adicionar** e adicione cada um dos grupos que estiver listado.  
  
   > [!NOTE]
   > - Você pode receber um erro de permissão se o serviço Netlogon não tiver iniciado.  
   >   -   Você deve fazer logoff e fazer logon novamente no servidor para que as alterações tenham efeito.  
  
    Você pode usar a última versão do Agente de Atualização do Windows para garantir que o processo de atualização do servidor funcione adequadamente.  
  
   Você pode usar a última versão do Agente de Atualização do Windows para garantir que o processo de atualização do servidor funcione adequadamente.  
  
   Antes de instalar Windows Update Agent no servidor de origem, você deve primeiro instalar o Windows PowerShell 2,0 e o Microsoft Baseline Configuration Analyzer 2,0.  
  
-   Para baixar e instalar o Windows PowerShell 2,0, consulte o [artigo 968929](https://go.microsoft.com/fwlink/p/?LinkId=241483) na base de dados de conhecimento Microsoft.  
  
-   Para baixar e instalar o Microsoft Baseline Configuration Analyzer 2,0, consulte o [Microsoft Baseline Configuration analyzer 2,0](https://go.microsoft.com/fwlink/p/?LinkId=241484) no centro de download da Microsoft.  
  
-   Para baixar e instalar a versão mais recente do agente de Windows Update, consulte o [artigo 949104](https://go.microsoft.com/fwlink/p/?LinkId=237493) na base de dados de conhecimento Microsoft.  
  
##### <a name="to-install-and-run-the-migration-preparation-tool-on-the-source-server"></a>Para instalar e executar a Ferramenta de Preparação da Migração no Servidor de Origem  
  
1. Insira DVD1 do Windows Server Essentials na unidade de DVD no servidor de origem.  
  
2. Abra o Windows Explorer, navegue até a pasta **\support\tools** do DVD e clique duas vezes no arquivo **sourcetool.msi**.  
  
   > [!NOTE]
   > - Se a ferramenta de preparação da migração já estiver instalada no servidor, execute-a a partir do menu **Iniciar**.  
   >   -   Para garantir que você esteja preparado para a melhor experiência de migração possível, recomenda-se que você sempre instale a atualização mais recente.  
  
    O assistente instala a Ferramenta de Preparação de Migração no Servidor de Origem. Quando a instalação é concluída, a Ferramenta de Preparação de Migração é executada automaticamente e instala as atualizações mais recentes.  
  
3. Na Ferramenta de Preparação da Migração, selecione **Eu tenho um backup e estou pronto para prosseguir** e em **Avançar**.  
  
   > [!WARNING]
   >  Se você receber uma mensagem de erro relacionada a uma instalação de hotfix, consulte o método 2: renomeie a pasta Catroot2 no [artigo 822798](https://go.microsoft.com/FWLink/p/?LinkID=118672) da base de dados de conhecimento Microsoft.  
  
    A Ferramenta de Preparação da Migração prepara o domínio de origem para migração estendendo o esquema de Diretório Ativo. Depois de a tarefa ser concluída, clique em **Avançar** para continuar.  
  
4. Depois de preparar o domínio de origem, a Ferramenta de Preparação da Migração verifica o Servidor de Origem para identificar dois tipos de problemas em potencial.  
  
   - **Erros** do Problemas encontrados no servidor de origem que podem bloquear a migração ou fazer com que a migração falhe. Siga as instruções na mensagem de erro para corrigir os problemas e então clique em **Verificar novamente**.  
  
   - **Avisos** Problemas encontrados no servidor de origem que podem causar problemas funcionais durante a migração. É fortemente recomendado que você siga as instruções na mensagem de erro para corrigir problemas antes de continuar com a migração.  
  
     Depois de corrigir ou confirmar todos os problemas, clique em **Avançar**.  
  
5. Na Ferramenta de Preparação da Migração, clique em **Concluir**.  
  
6. Quando a ferramenta de preparação de migração for concluída, você poderá ser solicitado a reiniciar o servidor de origem antes de poder começar a migrar para o Windows Server Essentials.  
  
> [!NOTE]
>  Você deve concluir uma execução bem-sucedida da ferramenta de preparação de migração no servidor de origem dentro de duas semanas após a instalação do Windows Server Essentials no servidor de destino. Caso contrário, a instalação do Windows Server Essentials no servidor de destino será bloqueada. Se isso ocorrer, você deve executar a Ferramenta de Preparação da Migração no Servidor de Origem novamente.  
  
###  <a name="create-a-plan-to-migrate-line-of-business-applications"></a><a name="BKMK_PlanToMigrateLineOfBusinessApplications"></a>Criar um plano para migrar aplicativos de linha de negócios  
 Um aplicativo LOB (linha de negócios) é um aplicativo de computador crítico para a administração de um negócio. Os aplicativos de LOB são os aplicativos de contabilidade, gerenciamento cadeia de fornecedores e planejamento de recursos.  
  
 Ao planejar a migração de seus aplicativos de LOB, consulte um provedor de aplicativo de LOB para determinar o método apropriado de migração de cada aplicativo. Você também deve localizar a mídia usada para instalar os aplicativos de LOB no Servidor de Destino.  
  
> [!NOTE]
>  Se você usou o SDK do Windows Small Business Server 2011 Essentials para desenvolver um suplemento de alerta ou de integridade do sistema personalizado e quiser continuar a usar o suplemento com o Windows Server Essentials, também deverá atualizar o suplemento e implantá-lo no servidor de destino.  
  
