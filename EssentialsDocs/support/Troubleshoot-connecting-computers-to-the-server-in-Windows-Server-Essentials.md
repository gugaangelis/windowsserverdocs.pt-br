---
title: Solucionar problemas de computadores que se conectam ao servidor no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: e45b3d89-c057-4c70-a627-86fb06dd22aa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2d98d4cc561a3c29ce73455f38f787709149d056
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852189"
---
# <a name="troubleshoot-connecting-computers-to-the-server-in-windows-server-essentials"></a>Solucionar problemas de computadores que se conectam ao servidor no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Este tópico contém diretrizes para solução de problemas que você pode encontrar ao conectar um computador ao servidor que está executando o Windows Server Essentials ou o Windows Server Essentials.  
  
> [!NOTE]
>  Para obter as informações mais atuais de solução de problemas da Comunidade do Windows Server Essentials e do Windows Server Essentials, sugerimos que você visite o [Fórum do Windows Server Essentials](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserveressentials). O fórum do Windows Server Essentials é uma boa oportunidade para obter ajuda ou para fazer uma pergunta.  
  
 Este tópico fornece soluções para os seguintes problemas:  
  

-   Problema 1: [problema 1](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problema 2: [problema 2](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problema 3: [problema 3](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problema 4: [problema 4](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problema 5: [problema 5](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problema 6: [problema 6](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problema 7: [problema 7](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problema 8: [problema 8](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problema 9: [problema 9](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problema 10: [problema 10](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problema 11: [problema 11](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

-   Problema 1: [problema 1](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problema 2: [problema 2](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problema 3: [problema 3](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problema 4: [problema 4](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problema 5: [problema 5](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problema 6: [problema 6](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problema 7: [problema 7](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problema 8: [problema 8](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problema 9: [problema 9](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problema 10: [problema 10](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problema 11: [problema 11](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

  
##  <a name="issue-1"></a><a name="BMRK_Package"></a>Problema 1  
 **Problema**  
  
 Recebo uma instalação de pacote não teve sucesso. Tente instalar o Windows Server Essentials Connector novamente. Se o problema persistir, entre em contato com o erro do administrador de rede ao conectar um computador ao servidor  
  
 **Descrição**  
  
 Esse problema pode ocorrer se você conectar um computador a um servidor que esteja executando o Windows Server Essentials enquanto outras atualizações do Windows ou instalações de aplicativos estiverem pendentes e a instalação do conector for cancelada.  
  
 **Solução**  
  
 Conclua todas as outras atualizações ou instalações de aplicativos. Se solicitado, reinicie o computador.  
  
##  <a name="issue-2"></a><a name="BKMK_ConnectorIssue2"></a>Problema 2  
 **Problema**  
  
 Não é possível ingressar um computador no Windows Server Essentials  
  
 **Descrição**  
  
 Computadores que têm caracteres não ASCII no nome do computador não podem ser Unidos ao Windows Server Essentials. Se o nome do computador inclui caracteres não ASCII, você recebe a mensagem de erro “Ocorreu um erro inesperado.”  
  
 **Solução**  
  
 Renomeie o computador cliente com um nome que contenha apenas caracteres ASCII e tente adicionar o computador ao Windows Server Essentials novamente.  
  
##  <a name="issue-3"></a><a name="BKMK_ConnectorIssue2a"></a>Problema 3  
 **Problema**  
  
 Obtenho um erro de instalação do software do conector cancelada ao conectar um computador ao servidor  
  
 **Descrição**  
  
 Para poder conectar um computador ao servidor, a conta do sistema deve ter permissões de controle total em pastas do servidor exibidas no painel do Windows Server Essentials. Se as permissões requeridas não tiverem sido concedidas, você receberá a mensagem de erro “A instalação do software Connector foi cancelada”.  
  
 **Solução**  
  
 Conceda permissões de **Controle Total** à conta SISTEMA em cada pasta do servidor.  
  
#### <a name="to-grant-the-system-account-full-control-permissions-on-a-server-folder"></a>Para conceder permissões de Controle Total à conta do SISTEMA em uma pasta do servidor  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Clique em **Armazenamento** e clique em **Pastas do servidor**.  
  
3.  Clique com o botão direito do mouse na pasta do servidor e, em seguida, clique em **Abrir a pasta**. (Se você não vir a opção **Abrir a pasta**, você não precisará configurar permissões na pasta.)  
  
4.  No caminho de rede, na parte superior do Windows Explorer, clique no compartilhamento de servidor para exibir as pastas compartilhadas no servidor. Por exemplo, se o caminho for **Rede > Servidor01 > Backups de Histórico de Arquivos**, clique em **Servidor01**.  
  
5.  Clique com o botão direito do mouse em uma pasta de servidor e, em seguida, clique em **Propriedades**.  
  
6.  Clique na guia **Segurança**.  
  
7.  Se não for permitido conceder permissões de **Controle total** à conta do SISTEMA, clique em **Editar** e, em seguida, clique em **SISTEMA**. Em **Permissões para o Sistema**, marque a caixa de seleção **Permitir** ao lado de **Controle total**.  
  
8.  Clique em **OK** duas vezes para atualizar as permissões e fechar a caixa de diálogo **Propriedades**.  
  
##  <a name="issue-4"></a><a name="BKMK_ConnectorIssueNetFramework"></a>Problema 4  
 **Problema**  
  
 Obtenho um para executar este aplicativo, você deve instalar uma das seguintes versões do .NET Framework: V 4.5.50709 "ao conectar um computador ao servidor  
  
 **Descrição**  
  
 Quando você conecta um computador a um servidor que executa o Windows Server Essentials ou o Windows Server Essentials, o assistente tenta instalar .NET Framework versão 4.5.50709 no computador. No entanto, se uma versão anterior do .NET Framework versão 4,5 estiver presente, a versão atualizada não poderá ser instalada e a tentativa de conexão falhará com esta mensagem de erro: para executar este aplicativo, você deve instalar uma das seguintes versões do .NET Framework: V 4.5.50709. Entre em contato com seu editor de alocação para obter instruções sobre como obter a versão apropriada do .NET Framework.  
  
 **Solução**  
  
 Desinstale o .NET Framework 4.5 do computador e então conecte o computador ao servidor.  
  
#### <a name="to-uninstall-net-framework-45"></a>Para desinstalar o .NET Framework 4.5  
  
1.  Na página **Inicial** do computador cliente, abra o **Painel de Controle**.  
  
2.  Em Painel de Controle, em **Programas**, clique em **Desinstalar um programa**.  
  
3.  Clique com o botão direito do mouse em **Microsoft .NET Framework 4.5** e clique em **Desinstalar**.  
  
4.  Após o .NET Framework 4.5 ter sido desinstalado com êxito, conecte o computador ao servidor. A versão correta do .NET Framework 4,5 é instalada juntamente com o software Connector.  
  
##  <a name="issue-5"></a><a name="BKMK_Time"></a>Problema 5  
 **Problema**  
  
 Obtenho um servidor não está disponível. Para resolver esse problema, entre em contato com a pessoa responsável pela sua rede. ao conectar um computador ao servidor  
  
 **Descrição**  
  
 Isso pode ocorrer se a data e hora no computador conectado não estiverem sincronizadas àquelas no servidor.  O Windows Server Essentials e o Windows Server Essentials usam o serviço de sincronização de tempo para sincronizar a data e a hora dos computadores em execução em uma rede do Windows Server Essentials ou do Windows Server Essentials. A data/hora sincronizada é fundamental, porque o protocolo de autenticação padrão usa o horário do servidor como parte do processo de autenticação. Por exemplo, se o relógio em um computador cliente não estiver sincronizado com a data e a hora corretas, a autenticação do Windows Server Essentials ou do Windows Server Essentials poderá interpretar falsamente uma solicitação de logon como uma tentativa de invasão e negar acesso ao usuário.  
  
 Isso pode acontecer se a memória livre do servidor for inferior a 5%.  
  
 Isso pode ocorrer se você já tem uma conexão VPN ao Windows Essentials Server e você tenta configurar o software Connector de fora do local, usando um endereço de domínio.  
  
 **Solução**  
  
1.  Sincronize a data e hora no computador cliente com aquelas no servidor. Então, conecte o computador ao servidor.  
  
2.  Feche alguns aplicativos no lado do servidor e conecte o computador ao servidor.  
  
3.  Feche a conexão VPN, então conecte o computador ao servidor.  
  
#### <a name="to-change-the-date-and-time-on-the-client-computer"></a>Para alterar a data e hora no computador cliente  
  
1. Na página Inicial do computador cliente, abra o **Painel de Controle**.  
  
2. No Painel de Controle, clique em **Relógio, Idioma e Região** e, a seguir, em **Data e Hora**.  
  
3. Clique em **Alterar data e hora**, defina-as para a data e hora corretas e clique em **OK**.  
  
4. Clique em **OK**, então feche o Painel de Controle.  
  
5. Tente novamente para conectar o computador ao servidor. Para obter instruções, consulte Conectar computadores ao servidor.  
  
   Se você ainda não puder conectar o computador cliente ao servidor, certifique-se de que a data e hora no servidor estão certas. Se a data e hora não estão certas, modifique-as.  
  
#### <a name="to-change-the-date-and-time-on-the-server"></a>Para alterar data e hora no servidor  
  
1.  Faça logon no servidor usando a senha que você configurou durante a instalação e configuração do Windows Server Essentials ou do Windows Server Essentials.  
  
    > [!NOTE]
    >  Se estiver gerenciando o servidor remotamente, você deve efetuar logon no servidor usando a conexão de área de trabalho remota.  
  
2.  Na página **Inicial**, abra o **Painel de Controle**.  
  
3.  No Painel de Controle, clique em **Relógio, Idioma e Região** e, a seguir, em **Data e Hora**.  
  
4.  Clique em **Alterar data e hora**, defina-as para a data e hora corretas e clique em **OK**.  
  
5.  Clique em **OK**, então feche o Painel de Controle.  
  
6.  No computador cliente, tente novamente conectar o computador cliente ao servidor. Para obter instruções, consulte Conectar computadores ao servidor.  
  
##  <a name="issue-6"></a><a name="BKMK_ServiceStopped"></a>Problema 6  
 **Problema**  
  
 Recebo um erro inesperado. Para resolver esse problema, entre em contato com a pessoa responsável pela sua rede. ao conectar um computador ao servidor  
  
 **Descrição**  
  
 Se você receber essa mensagem de erro, é possível que o Serviço Web de Certificado WSS não esteja em execução.  
  
 **Solução**  
  
 Iniciar o Serviço Web de Certificado WSS.  
  
#### <a name="to-start-the-wss-certificate-web-service"></a>Para iniciar o Serviço Web de Certificado WSS  
  
1.  Na página **Inicial** do servidor, clique em **Ferramentas Administrativas**.  
  
     Em **Ferramentas Administrativas**, clique com o botão direito do mouse em **Gerenciador dos Serviços de Informações da Internet (IIS)** e, em seguida, clique em **Abrir**.  
  
2.  No painel de navegação, clique em **Serviço Web de Certificado WSS**.  
  
3.  No painel **Ações**, clique em **Iniciar**.  
  
##  <a name="issue-7"></a><a name="BKMK_ConnectorIssueReconnect"></a>Problema 7  
 **Problema**  
  
 Quando tento conectar um computador ao servidor novamente após uma tentativa de conexão malsucedida, recebo o aviso de que um computador com esse nome já está conectado ao servidor  
  
 **Descrição**  
  
 Se uma tentativa anterior de conectar um computador ao servidor foi cancelada ou interrompida, você poderá receber o seguinte aviso ao tentar se conectar novamente: um computador com esse nome já está conectado ao servidor. Isso ocorre porque um certificado foi emitido quando você tentou conectar ao servidor pela primeira vez.  
  
 **Solução** Se você tem certeza de que nenhum outro computador com o mesmo nome já está conectado ao servidor, clique em **Avançar**, então siga as instruções para concluir o assistente **Conectar meu computador ao servidor**.  
  
##  <a name="issue-8"></a><a name="BKMK_JoinWin7"></a>Problema 8  
 **Problema**  
  
 Quando tento conectar um computador cliente que está executando o Windows 7 Home ao servidor, a página da Web para executar o software do conector é aberta, mas o computador cliente não é capaz de se conectar ao servidor  
  
 **Descrição**  
  
 Se o roteador em sua rede tiver multicast habilitado, as comunicações entre o servidor e um computador cliente executando Windows 7 Home Premium ou Windows 7 Home Basic não funcionam corretamente.  
  
 **Solução**  
  
 Desabilite o multicast em seu roteador. Em alguns roteadores, isso pode incluir a desabilitação do protocolo de roteamento RIP-2M. Para obter mais informações, consulte a documentação fornecida pelo fabricante do roteador.  
  
##  <a name="issue-9"></a><a name="BKMK_ConnectorIssueAutologon"></a>Problema 9  
 **Problema**  
  
 O logon automático parou de funcionar após eu ter conectado o computador ao servidor  
  
 **Descrição**  
  
 Se o logon automático estiver definido para a conta de usuário quando você conectar o computador ao Windows Server Essentials, a configuração será substituída quando o software do conector estiver instalado no computador.  
  
 **Solução** Para resolver esse problema, quando você conectar o computador ao servidor, anote a senha usada para a conta de usuário. Após o software Connector ser instalado, configure o logon automático para utilizar essa conta.  
  
> [!NOTE]
>  A conta de domínio do Windows Server Essentials requer uma senha que atenda aos requisitos de política de senha padrão.  
  
##  <a name="issue-10"></a><a name="BKMK_ConnectorIssueOldLogs"></a>Problema 10  
 **Problema**  
  
 Desinstalar uma versão de pré-lançamento do software Connector não remove os logs existentes  
  
 **Descrição**  
  
 Depois de atualizar de uma versão de pré-lançamento (beta ou RC) do Windows Server Essentials para a versão de lançamento, você deve remover o software do conector de cada computador que estava conectado ao servidor e, em seguida, conectar o computador novamente para instalar o lançado versão do software do conector.  
  
 No entanto, quando você remove o software do conector de um computador da rede, os arquivos de log existentes na pasta %ProgramData%\Microsoft\Windows Server\Logs\ nesse computador não são excluídos. Se você não excluir a pasta logs, os arquivos de log poderão ser corrompidos quando você conectar o computador à versão de lançamento do Windows Server Essentials.  
  
 **Solução** Para evitar corrupção dos arquivos de log, exclua manualmente a pasta Logs antes de reconectar o computador cliente ao servidor atualizado.  
  
#### <a name="to-reconnect-a-computer-after-a-server-update-without-corrupting-the-log-files"></a>Para reconectar um computador após uma atualização de servidor sem corromper os arquivos de log  
  
1.  Desinstale o software Connector do computador cliente.  
  
2.  Exclua a pasta Logs da pasta %ProgramData%\Microsoft\Windows Server\.  
  
3.  Conecte o computador ao servidor novamente. Isso instala a versão de lançamento do software Connector, criando uma nova pasta Logs e novos arquivos de log.  
  
##  <a name="issue-11"></a><a name="BKMK_UpgradeClientOS"></a>Problema 11  
 **Problema**  
  
 Desejo atualizar o sistema operacional em um computador cliente  
  
 **Descrição**  
  
 Durante a instalação do software Connector, diversas verificações são executadas no sistema operacional cliente para garantir que o cliente atenda a todos os pré-requisitos do Connector. Se você atualizar o sistema operacional cliente após instalar o software Connector, alguns dos pré-requisitos podem não estar presentes e o conector cliente poderá falhar.  
  
 **Solução**  
  
 Antes de atualizar o sistema operacional cliente para uma versão diferente (por exemplo, você atualiza o Windows XP para Windows Vista ou Windows Vista para o Windows 7), você deve desinstalar o software Connector. No Painel de Controle, utilize **Adicionar ou Remover Programas**. Após a conclusão da atualização do sistema operacional do cliente, você pode reinstalar o conector do cliente abrindo o*servidor*http://< >/Connect em um navegador da Web, em que <*servidor*> é o nome do servidor do Windows Server Essentials.  
  
 Se você já atualizou o cliente com o software Connector instalado, use **Adicionar/Remover Programas** ou **Programas e Recursos** para desinstalar o software Connector. Em seguida, instale o software Connector novamente.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar o Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  
-   [Solução de problemas do Windows 2012 Server Essentials ConnectComputer (TechNet wiki)](https://social.technet.microsoft.com/wiki/contents/articles/14370.windows-2012-server-essentials-connectcomputer-troubleshooting.aspx)
