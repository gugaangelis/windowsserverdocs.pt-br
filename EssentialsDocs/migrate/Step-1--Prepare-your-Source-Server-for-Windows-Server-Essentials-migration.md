---
title: "Etapa 1: Preparar sua migração do servidor de origem para o Windows Server Essentials"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 244c8a06-04c6-4863-8b52-974786455373
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2efb1badde6d0ca11bc3b7526fdfb377d9f95d3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="step-1-prepare-your-source-server-for-windows-server-essentials-migration"></a>Etapa 1: Preparar sua migração do servidor de origem para o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este tópico explica como fazer backup de servidor de origem, avaliar a integridade do sistema de servidor de origem, instale os service packs e correções mais recentes e verifique se a configuração de rede.  
  
## <a name="to-prepare-for-migration"></a>Para preparar para a migração  
 Conclua as seguintes etapas preliminares para garantir que as configurações e os dados em seu servidor de origem migram com êxito para o servidor de destino.  
  
1.  [Faça backup do servidor de origem](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Instale os service packs mais recentes](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Excluir o Log em como uma configuração de conta de serviço](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_DeleteSvcAcctSetting)  
  
4.  [Avaliar a integridade do servidor de origem](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_EvaluateHealth)  
  
5.  [Criar um plano para migrar aplicativos de linha de negócios](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MigrateLOB)  
  
###  <a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>Faça backup do servidor de origem  
 Faça backup do servidor de origem antes de começar o processo de migração. Fazendo um backup ajuda a proteger seus dados contra perda acidental se ocorrer um erro irrecuperável durante a migração.  
  
##### <a name="to-back-up-the-source-server"></a>Para fazer backup do servidor de origem  
  
1.  Use um dos recursos na tabela a seguir para orientá-lo a fazer um backup completo do servidor de origem.  
  
2.  Verifique se o backup foi executado com êxito. Para testar a integridade do backup, selecione arquivos aleatórios do backup, restaurá-los em um local alternativo e confirme que os arquivos restaurados são iguais aos arquivos originais.  
  
   |Produto|Recurso|
   |---|---|
   |Windows Small Business Server 2003|[Fazendo backup e restaurando o Windows Small Business Server 2003](https://msdn.microsoft.com/library/cc875809.aspx) 
   |Windows Small Business Server 2008|[Fazendo backup e restaurando dados no Windows Small Business Server 2008](https://technet.microsoft.com/library/cc527505\(WS.10\).aspx)
   |Windows Server 2008 Foundation|[Backup e recuperação](https://technet.microsoft.com/library/cc754097\(WS.10\).aspx)  
   |Windows Small Business Server 2011 Essentials|[Saiba mais sobre como configurar o backup do servidor](https://technet.microsoft.com/library/server-backup-support-1.aspx)
   |Windows Small Business Server 2011 Standard|[Gerenciando o Backup do servidor](https://technet.microsoft.com/library/cc527488.aspx)  
   |Windows Server Essentials|[Gerenciar o Backup e restauração do Windows Server Essentials](https://technet.microsoft.com/library/jj713536.aspx)

###  <a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>Instale os service packs mais recentes  
 Você deve instalar os service packs e atualizações mais recentes no servidor de origem antes da migração.  
  
###  <a name="BKMK_DeleteSvcAcctSetting"></a>Excluir o Log em como uma configuração de conta de serviço  
 Se você estiver migrando do Windows Small Business Server 2003 ou Windows Server 2003, exclua o **fazer logon como um serviço** configuração da política de grupo da conta.  
  
##### <a name="to-delete-the-log-on-as-a-service-account-setting"></a>Para excluir o Log em como uma configuração de conta de serviço  
  
1.  Para abrir o **Group Policy Management** ferramenta, clique em **iniciar**, clique em **painel de controle**, clique em **ferramentas administrativas**e clique em **Group Policy Management**.  
  
2.  Clique com botão direito **política de controladores de domínio padrão**e clique em **editar**.  
  
3.  Navegue até **atribuição de direitos de usuário do computador Configuration\Windows Settings\Security Settings\Local**.  
  
4.  No painel de detalhes, clique duas vezes em **fazer logon como um serviço**.  
  
5.  Limpar o **definir essas configurações de política** caixa de seleção.  
  
6.  Exclua \scripts\SBS_LOGIN_SCRIPT.bat \\\localhost\SYSVOL\\ < domainname\ >.  
  
###  <a name="BKMK_EvaluateHealth"></a>Avaliar a integridade do servidor de origem  
 É importante avaliar a integridade do seu servidor de origem antes de começar a migração. Use os procedimentos a seguir para garantir que as atualizações sejam atuais, para gerar um relatório de integridade do sistema e executar o Windows Server soluções melhor prática BPA (Analyzer).  
  
#### <a name="download-and-install-critical-and-security-updates"></a>Download e instalação crítica e segurança atualizações  
 Instalando críticas e atualizações de segurança no servidor de origem ajuda a garantir que sua migração seja bem-sucedida e ajuda a proteger sua rede durante o processo de migração.  
  
###### <a name="to-check-for-the-latest-updates"></a>Para verificar as atualizações mais recentes  
  
1.  No servidor de origem, clique em **iniciar**, clique em **todos os programas**e clique em **Windows Update**.  
  
2.  Clique em **verificar se há atualizações**.  
  
3.  Se forem encontradas atualizações, clique em **instalar atualizações**.  
  
#### <a name="run-the-best-practices-analyzer"></a>Execute o analisador de práticas recomendadas  
 Você pode executar o analisador de práticas recomendadas (BPA) para verificar que não há nenhum problema no servidor, rede ou domínio antes de começar o processo de migração. A BPA coleta informações de configuração das seguintes fontes:  
  
-   No Active Directory Windows Management Instrumentation (WMI)  
  
-   O registro  
  
-   Serviços de informações da Internet (IIS)  
  
###### <a name="to-use-the-bpa-to-analyze-your-source-server"></a>Para usar a ferramenta BPA para analisar seu servidor de origem  
  
1.  A tabela a seguir fornece links para Microsoft Download Center onde você pode baixar e instalar o analisador de práticas recomendadas (BPA) para o servidor de origem.  
  
   |Se o seu servidor de origem está em execução|Você pode obter as ferramentas BPA do|
   |---|---|
   |Windows SBS 2003|[Site do Microsoft Windows Small Business Server 2003 analisador de práticas recomendadas](https://www.microsoft.com/download/details.aspx?id=5334)
   |Windows SBS 2008|[Site do Microsoft Windows Small Business Server 2008 analisador de práticas recomendadas](https://www.microsoft.com/download/details.aspx?id=6231)  
   |Windows SBS 2011 Essentials ou o Windows SBS 2011 Standard|[Site do analisador de práticas recomendadas do Windows Server Solutions](https://www.microsoft.com/download/details.aspx?id=15556) 
   |Windows Server Essentials ou o Windows Server 2012|O painel de servidor  
  
2.  Depois que o download for concluído, clique em **iniciar**, aponte para **todos os programas**e clique em **ferramenta do analisador de práticas recomendadas do SBS**.  
  
    > [!NOTE]
    >  Verificar se há atualizações antes de verificar o servidor.  
  
3.  No painel de navegação, clique em **iniciar uma verificação**.  
  
     Se o servidor de origem está executando o Windows Server Essentials, faça o seguinte:  
  
    1.  Faça logon no servidor de destino como administrador e, em seguida, abra o painel.  
  
    2.  No painel, clique no **dispositivos** guia.  
  
    3.  No <**servidor** >**tarefas** painel, clique em **analisador de práticas recomendadas**.  
  
4.  No painel de detalhes, digite o rótulo da verificação e clique em **inicie a verificação**. O rótulo da verificação é o nome do relatório de digitalização, por exemplo, **SBS BPA Scan 1 de julho de 2013**.  
  
5.  Depois que a verificação termina, clique em **visualizar um relatório dessa verificação de práticas recomendadas**.  
  
 Depois que a ferramenta BPA coleta informações sobre a configuração do servidor, ele verifica se as informações estão corretas e, em seguida, apresenta os administradores com uma lista de informações e problemas classificados por gravidade. A lista descreve cada problema e fornece uma solução de recomendação ou possíveis. Existem três tipos de relatório:  
  
|Tipo de relatório|Descrição
|-----------------|----------------- 
|Relatórios de lista|Exibe relatórios em uma lista unidimensional. 
|Relatórios de árvore|Exibe relatórios em uma lista hierárquica.

Para exibir a descrição e as soluções para um problema, clique no problema no relatório. Nem todos os problemas relatados pela ferramenta BPA afetam a migração, mas você deve resolver como muitos dos problemas possíveis garantir que a migração foi bem-sucedida.  
  
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
  
###  <a name="BKMK_MigrateLOB"></a>Criar um plano para migrar aplicativos de linha de negócios  
 Um aplicativo de linha de negócios (LOB) é um aplicativo de computador críticos que são vital para a execução de uma empresa. Os aplicativos LOB incluem gerenciamento de contabilidade, cadeia de suprimento e aplicativos de planejamento de recursos.  
  
 Quando você planeja migrar seus aplicativos LOB, consulte os provedores de aplicativos LOB para determinar o método apropriado para cada aplicativo a migração. Você também deve localizar a mídia que é usada para instalar os aplicativos LOB no servidor de destino.  
  
> [!NOTE]
>  Se você usou o SDK do Windows Small Business Server 2011 Essentials para desenvolver uma integridade do sistema que você personalizou ou alerta suplemento e você quiser continuar a usar o suplemento com o Windows Server Essentials, você também deve atualizar o suplemento e implantá-la no servidor de destino.  
  
  
### <a name="create-a-plan-to-migrate-email-hosted-on-windows-sbs-2011-windows-sbs-2008-and-windows-sbs-2003"></a>Criar um plano de migração de email hospedada no Windows SBS 2011, Windows SBS 2008 e Windows SBS 2003  
 No Windows SBS 2011, Windows SBS 2008 e Windows SBS 2003, o email é fornecido através do Microsoft Exchange Server. No entanto, o Windows Server Essentials não fornece um serviço de email da caixa de entrada. Se você atualmente estiver usando um servidor que executa o Windows SBS 2011, Windows SBS 2008 ou Windows SBS 2003 para hospedar seu email da empresa s, você precisa migrar para um alternativo no local ou hospedado solução.  
  
> [!NOTE]
>  Depois de atualizar e preparar seu servidor de origem para a migração, recomendamos que você criar um backup do servidor atualizado antes de continuar o processo de migração.  
  
#### <a name="migrate-email-to-microsoft-office-365"></a>Migrar email Microsoft Office 365  
 Se você optou por usar o Microsoft Office 365 como a solução de email para seu domínio, siga as diretrizes em [migrar todas as caixas de correio para a nuvem com uma migração migração do Exchange](http://help.outlook.com/140/ms.exch.ecp.emailmigrationwizardexchangelearnmore.aspx) para iniciar a migração de email para o Office 365. Recomendamos que você conclua a migração de email antes de instalar o Windows Server Essentials.  
  
> [!NOTE]
>  A etapa para remover o servidor do Exchange no local no servidor de origem é obrigatória se você pretende integrar o Windows Server Essentials com o Office 365. Para obter informações sobre como migrar pastas públicas do Exchange Server para o Office 365, consulte o blog postagem [Microsoft Exchange 2013 público pastas Scripts de migração para o Office 365](http://blogs.technet.com/b/fmustafa/archive/2013/04/11/microsoft-exchange-2013-public-folders-migration-scripts-for-office-365.aspx).  
>   
>  Depois de concluir a instalação, você deve ativar o recurso de integração do Office 365 no Windows Server Essentials, executando o **integrar com o Microsoft Office 365** tarefa.  
  
> [!IMPORTANT]
>  Para permitir que a ferramenta de migração do Office 365 para se conectar ao servidor do Exchange está em execução no servidor de origem, você deve habilitar RPC por HTTP no servidor de origem. Para obter informações sobre como habilitar RPC por HTTP, consulte [como implantar RPC por HTTP pela primeira vez no Small Business Server 2003 (Standard ou Premium)](https://technet.microsoft.com/library/bb123622%28EXCHG.65%29.aspx). Se você não pode executar a ferramenta de migração do Office 365 com êxito depois de habilitar RPC por HTTP, visualize o **ValidPorts** definindo o registro no HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy e certifique-se de que o nome de domínio totalmente qualificado (FQDN) do servidor de origem está listado. Se o FQDN não estiver listado, adicioná-lo manualmente usando o exemplo a seguir:  
>   
>  remoto. *Contoso*.com:6001-6002; remoto. *Contoso*.com:6004 (substitua *contoso* com o nome do seu domínio).  
  
#### <a name="migrate-email-to-another-on-premises-exchange-server"></a>Migrar email para outro servidor do Exchange no local  
 Para obter informações sobre como migrar email para outro servidor do Exchange no local, consulte [integrar um servidor do Exchange local diante com o Windows Server Essentials](https://technet.microsoft.com/library/jj200172.aspx). Recomendamos que você configure o servidor do Exchange no local novo depois de instalar o Windows Server Essentials e conclua a migração de email antes de rebaixá o servidor de origem.  
  
> [!NOTE]
>  O Windows Small Business Server POP3 Connector não está incluído com o Exchange Server. Depois de migrar dados de email para outro servidor do Exchange, você não pode usar o recurso de conector POP3.  
  
> [!NOTE]
>  Depois de atualizar e preparar seu servidor de origem para a migração, você deve criar um backup do servidor atualizado antes de continuar o processo de migração.  
  
## <a name="next-steps"></a>Próximas etapas  
 Você preparou o seu servidor de origem para a migração para o Windows Server Essentials.  Acesse agora [etapa 2: instalar o Windows Server Essentials como um controlador de domínio réplica novo](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md).  

Para ver todas as etapas, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

