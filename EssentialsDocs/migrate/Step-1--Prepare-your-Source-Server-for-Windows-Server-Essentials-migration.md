---
title: 'Etapa 1: Preparar o servidor de origem para a migração para o Windows Server Essentials'
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 244c8a06-04c6-4863-8b52-974786455373
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 214f25d1743cca3693907b7a7a8380fd564d4114
ms.sourcegitcommit: 32f810c5429804c384d788c680afac427976e351
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83203472"
---
# <a name="step-1-prepare-your-source-server-for-windows-server-essentials-migration"></a>Etapa 1: Preparar o servidor de origem para a migração para o Windows Server Essentials

> Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este tópico explica como fazer backup do servidor de origem, avaliar a integridade de sistema do servidor de origem, instalar os service packs e correções mais recentes e verificar a configuração de rede.

## <a name="to-prepare-for-migration"></a>Para se preparar para a migração
 Conclua as etapas preliminares a seguir para garantir que as configurações e dados do servidor de origem sejam migradas com êxito para o servidor de destino.

1.  [Fazer backup do Servidor de Origem](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)

2.  [Instalar os service packs mais recentes](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)

3.  [Excluir a configuração fazer logon como uma conta de serviço](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_DeleteSvcAcctSetting)

4.  [Avaliar a integridade do servidor de origem](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_EvaluateHealth)

5.  [Crie um plano para migrar aplicativos de linha de negócios](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MigrateLOB)

###  <a name="back-up-your-source-server"></a><a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>Fazer backup do servidor de origem
 Faça o backup do Servidor de Origem antes de iniciar o processo migração. Isso ajuda a proteger seus dados de perda acidental caso ocorra um erro irrecuperável durante a migração.

##### <a name="to-back-up-the-source-server"></a>Para fazer backup do Servidor de Origem

1. Use um dos recursos na tabela a seguir para ajudá-lo a executar um backup completo do servidor de origem.

2. Verifique se o backup foi executado com êxito. Para testar a integridade do backup, selecione arquivos aleatórios do backup, restaure-os em um local alternativo e verifique se os arquivos restaurados são iguais aos arquivos originais.

   |Produto|Recurso|
   |---|---|
   |Windows Small Business Server 2003|[Fazendo backup e restaurando o Windows Small Business Server 2003](https://msdn.microsoft.com/library/cc875809.aspx)
   |Windows Small Business Server 2008|[Fazendo Backup e Restaurando os Dados do Windows Small Business Server 2008](https://technet.microsoft.com/library/cc527505\(WS.10\).aspx)
   |Windows Server 2008 Foundation|[Backup e recuperação](https://technet.microsoft.com/library/cc754097\(WS.10\).aspx)
   |Windows Small Business Server 2011 Essentials|[Saiba mais sobre como configurar o backup do servidor](https://technet.microsoft.com/library/server-backup-support-1.aspx)
   |Windows Small Business Server 2011 Standard|[Gerenciando backup do servidor](https://technet.microsoft.com/library/cc527488.aspx)
   |Windows Server Essentials|[Gerenciar Backup e restauração no Windows Server Essentials](https://technet.microsoft.com/library/jj713536.aspx)

###  <a name="install-the-most-recent-service-packs"></a><a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>Instalar os service packs mais recentes
 Você deve instalar as últimas atualizações e service packs no Servidor de Origem antes da migração.

###  <a name="delete-the-log-on-as-a-service-account-setting"></a><a name="BKMK_DeleteSvcAcctSetting"></a>Excluir a configuração fazer logon como uma conta de serviço
 Se você estiver migrando do Windows Small Business Server 2003 ou Windows Server 2003, exclua a configuração de conta **fazer logon como um serviço** da política de grupo.

##### <a name="to-delete-the-log-on-as-a-service-account-setting"></a>Para excluir a configuração fazer logon como uma conta de serviço

1.  Para abrir a ferramenta **Gerenciamento de Política de Grupo**, clique em **Iniciar**, clique em **Painel de controle**, clique em **Ferramentas administrativas** e, por fim, clique em **Gerenciamento de Política de Grupo**.

2.  Clique com botão direito na **Política de Controladores de Domínio Padrão** e clique em **Editar**.

3.  Navegue até **Configuração\Configurações do Windows\Configurações de Segurança\Políticas locais\Atribuição de direitos de usuário**.

4.  No painel de detalhes, clique duas vezes em **Fazer logon como um serviço**.

5.  Limpe a caixa de seleção **Definir estas configurações de política**.

6.  Exclua \\ \localhost\SYSVOL \\<nome_do_domínio \> \scripts\ SBS_LOGIN_SCRIPT. bat.

###  <a name="evaluate-the-health-of-the-source-server"></a><a name="BKMK_EvaluateHealth"></a>Avaliar a integridade do servidor de origem
 É importante avaliar a integridade do servidor de origem antes de começar a migração. Use os procedimentos a seguir para garantir que as atualizações sejam atuais, para gerar um relatório de integridade do sistema e executar o Analisador de Práticas Recomendadas do Windows Server Solutions.

#### <a name="download-and-install-critical-and-security-updates"></a>Download e instalação de atualizações críticas e de segurança
 Instalar atualizações críticas e de segurança no servidor de origem ajuda a garantir que a migração seja bem-sucedida e a proteger sua rede durante o processo de migração.

###### <a name="to-check-for-the-latest-updates"></a>Para verificar as atualizações mais recentes

1.  No servidor de origem, clique em **Iniciar**, clique em **Todos os Programas** e clique em **Windows Update**.

2.  Clique em **Procurar atualizações**.

3.  Se forem encontradas atualizações, clique em **Instalar Atualizações**.

#### <a name="run-the-best-practices-analyzer"></a>Executar o Analisador de Práticas Recomendadas
 Você pode executar o Analisador de Práticas Recomendadas (BPA) para verificar se não há problemas no servidor, rede ou domínio antes de iniciar o processo de migração. O BPA coleta informações de configuração das seguintes origens:

-   WMI (Instrumentação de Gerenciamento do Windows) do Active Directory

-   O Registro

-   Serviços de Informações da Internet (IIS)

###### <a name="to-use-the-bpa-to-analyze-your-source-server"></a>Usar o BPA para analisar seu servidor de origem

1. A tabela a seguir fornece links para a Microsoft Download Center onde você pode baixar e instalar o Analisador de Práticas Recomendadas (BPA) para o servidor de origem.


   |             Se o servidor de origem estiver em execução             |                                                     Você pode obter as ferramentas do BPA de                                                      |
   |----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
   |                     Windows SBS 2003                     | [Site do Analisador de Práticas Recomendadas do Microsoft Windows Small Business Server 2003](https://www.microsoft.com/download/details.aspx?id=5334) |
   |                     Windows SBS 2008                     | [Site do Analisador de Práticas Recomendadas do Microsoft Windows Small Business Server 2008](https://www.microsoft.com/download/details.aspx?id=6231) |
   | Windows SBS 2011 Essentials ou Windows SBS 2011 Standard |          [Site do Analisador de Práticas Recomendadas do Windows Server Solutions](https://www.microsoft.com/download/details.aspx?id=15556)           |
   |     Windows Server Essentials ou Windows Server 2012     |                                                          O Painel do servidor                                                           |


2. Após a conclusão do download, clique em **Iniciar**, clique em **Todos os Programas** e clique em **Ferramenta Analisadora de Práticas Recomendadas do SBS**.

   > [!NOTE]
   >  Verifique se há atualizações antes de examinar o servidor.

3. No painel de navegação, clique na opção de **iniciar uma varredura**.

    Se o servidor de origem estiver executando o Windows Server Essentials, faça o seguinte:

   1.  Faça logon no servidor de destino como administrador e abra o Painel.

   2.  No painel, clique na guia **Dispositivos**.

   3.  No painel Tarefas do <**Server**  > **Tasks** , clique em **analisador de práticas recomendadas**.

4. No painel de detalhes, digite o rótulo da varredura e, em seguida, clique em **Iniciar a varredura**. Ele corresponde ao nome do relatório de varredura, por exemplo, **SBS BPA Scan 1Jul2013**.

5. Quando a varredura terminar, clique na opção de **exibição de relatório da varredura de práticas recomendadas**.

   Depois que a ferramenta BPA coleta informações sobre a configuração do servidor, ele verifica se as informações estão corretas e, em seguida, apresenta os administradores com uma lista de problemas classificados por gravidade e informações. A lista descreve cada problema e fornece uma recomendação ou solução possível. Há três tipos de relatórios disponíveis:

|Tipo de relatório|Descrição
|-----------------|-----------------
|Relatórios de lista|Exibe relatórios em uma lista unidimensional.
|Relatórios de árvore|Exibe relatórios em uma lista hierárquica.

Para exibir a descrição e as soluções de um problema, clique no problema no relatório. Nem todos os problemas relatados pela ferramenta BPA afetam a migração, mas você deve solucionar o máximo de problemas possível para garantir que a migração seja bem-sucedida.

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

###  <a name="create-a-plan-to-migrate-line-of-business-applications"></a><a name="BKMK_MigrateLOB"></a>Criar um plano para migrar aplicativos de linha de negócios
 Um aplicativo LOB (linha de negócios) é um aplicativo de computador crítico para a administração de um negócio. Os aplicativos de LOB são os aplicativos de contabilidade, gerenciamento cadeia de fornecedores e planejamento de recursos.

 Ao planejar a migração de seus aplicativos de LOB, consulte um provedor de aplicativo de LOB para determinar o método apropriado de migração de cada aplicativo. Você também deve localizar a mídia usada para instalar os aplicativos de LOB no Servidor de Destino.

> [!NOTE]
>  Se você usou o SDK do Windows Small Business Server 2011 Essentials para desenvolver um suplemento de alerta ou de integridade do sistema personalizado e quiser continuar a usar o suplemento com o Windows Server Essentials, também deverá atualizar o suplemento e implantá-lo no servidor de destino.


### <a name="create-a-plan-to-migrate-email-hosted-on-windows-sbs-2011-windows-sbs-2008-and-windows-sbs-2003"></a>Crie um plano para migrar emails hospedados no Windows SBS 2011, o Windows SBS 2008 e o Windows SBS 2003
 No Windows SBS 2011, o Windows SBS 2008 e o Windows SBS 2003, os emails são fornecidos pelo Microsoft Exchange Server. No entanto, o Windows Server Essentials não fornece um serviço de email de caixa de entrada. Se você estiver usando um servidor que executa o Windows SBS 2011, o Windows SBS 2008 ou o Windows SBS 2003 para hospedar o email s de sua empresa, será necessário migrar para uma solução alternativa local ou hospedada.

> [!NOTE]
>  Depois de atualizar e preparar o Servidor de Origem para migração, recomendamos criar um backup do servidor atualizado antes de continuar o processo de migração.

#### <a name="migrate-email-to-microsoft-office-365"></a>Migrar emails para o Microsoft Office 365
 Caso tenha optado por usar o Microsoft Office 365 como solução de email para seu domínio, siga as diretrizes em [Migrar todas as caixas de correio para a nuvem com uma migração de substituição do Exchange](https://help.outlook.com/140/ms.exch.ecp.emailmigrationwizardexchangelearnmore.aspx) para iniciar a migração de emails para o Office 365. Recomendamos que você conclua a migração de email antes de instalar o Windows Server Essentials.

> [!NOTE]
>  A etapa para remover o Exchange Server local no servidor de origem é obrigatória se você pretende integrar o Windows Server Essentials com o Office 365. Para obter informações sobre como migrar pastas públicas do Exchange Server para o Office 365, consulte a postagem de blog [Microsoft Exchange 2013 Public Folders Migration Scripts for Office 365 (Scripts de migração de pastas públicas do Microsoft Exchange 2013 para o Office 365)](https://blogs.technet.com/b/fmustafa/archive/2013/04/11/microsoft-exchange-2013-public-folders-migration-scripts-for-office-365.aspx).
>
>  Depois de concluir a instalação, você deve ativar o recurso integração do Office 365 no Windows Server Essentials executando a tarefa **integrar com Microsoft Office 365** .

> [!IMPORTANT]
>  Para permitir que a ferramenta de migração do Office 365 conecte-se com o Exchange Server em execução no servidor de origem, você deve habilitar RPC sobre HTTP nesse servidor. Para obter informações sobre como habilitar o RPC sobre HTTP, consulte [How to Deploy RPC over HTTP for the First Time in Small Business Server 2003 (Standard ou Premium) (Como implantar o RPC sobre HTTP pela primeira vez no Small Business Server 2003 (Standard ou Premium))](https://technet.microsoft.com/library/bb123622%28EXCHG.65%29.aspx). Caso não consiga executar a ferramenta de migração do Office 365 após habilitar RPC sobre HTTP, exiba a configuração **ValidPorts** no Registro em HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy e certifique-se de que o FQDN (nome de domínio totalmente qualificado) do servidor de origem esteja listado. Caso não esteja, adicione-o manualmente usando o exemplo a seguir:
>
>  remoto. *contoso*.com: 6001-6002; remoto. *contoso*.com:6004 (substitua *contoso* pelo nome do seu domínio)

#### <a name="migrate-email-to-another-on-premises-exchange-server"></a>Migrar emails para outro Exchange Server local
 Para obter informações sobre como migrar emails para outro servidor Exchange local, consulte [integrar um servidor Exchange local com o Windows Server Essentials](https://technet.microsoft.com/library/jj200172.aspx). Recomendamos que você configure o novo Exchange Server local após a instalação do Windows Server Essentials e, em seguida, conclua a migração de email antes de rebaixar o servidor de origem.

> [!NOTE]
>  O conector POP3 do Windows Small Business Server não está incluído no Exchange Server. Após migrar os dados de emails para outro Exchange Server, você não poderá mais usar o recurso de conector POP3.

> [!NOTE]
>  Depois de atualizar e preparar o servidor de origem para migração, você deve criar um backup do servidor atualizado antes de continuar o processo de migração.

## <a name="next-steps"></a>Próximas etapas
 Você preparou o servidor de origem para migração para o Windows Server Essentials.  Agora vá para a [etapa 2: instalar o Windows Server Essentials como um novo controlador de domínio de réplica](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md).

Para exibir todas as etapas, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

