---
title: Solucionar problemas de computadores que se conectam ao servidor no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e45b3d89-c057-4c70-a627-86fb06dd22aa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9b0d11be08840ecedabab6fd4e96f5d453ea4857
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-connecting-computers-to-the-server-in-windows-server-essentials"></a>Solucionar problemas de computadores que se conectam ao servidor no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Este tópico contém diretrizes de solução de problemas para os problemas que podem ocorrer quando você conectar um computador com o servidor que está executando o Windows Server Essentials ou o Windows Server Essentials.  
  
> [!NOTE]
>  Para obter as informações mais atuais solução de problemas do Windows Server Essentials e da comunidade do Windows Server Essentials, sugerimos que você visitar o [Fórum do Windows Server Essentials](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserveressentials). O fórum do Windows Server Essentials é um ótimo lugar para procurar ajuda ou fazer uma pergunta.  
  
 Este tópico fornece soluções para os seguintes problemas:  
  

-   Problema 1: [edição 1](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problema 2: [emitir 2](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problema 3: [emitir 3](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problema 4: [emitir 4](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problema 5: [emitir 5](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problema 6: [emitir 6](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problema 7: [emitir 7](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problema 8: [emitir 8](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problema 9: [emitir 9](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problema 10: [emitir 10](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problema 11: [emitir 11](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

-   Problema 1: [edição 1](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problema 2: [emitir 2](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problema 3: [emitir 3](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problema 4: [emitir 4](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problema 5: [emitir 5](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problema 6: [emitir 6](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problema 7: [emitir 7](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problema 8: [emitir 8](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problema 9: [emitir 9](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problema 10: [emitir 10](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problema 11: [emitir 11](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

  
##  <a name="BMRK_Package"></a>Problema 1  
 **Problema**  
  
 Eu recebo um pacote de instalação não teve êxito. Tente instalar o conector do Windows Server Essentials novamente. Se o problema persistir, fale com o erro do administrador de rede ao se conectar a um computador para o servidor  
  
 **Descrição**  
  
 Esse problema pode ocorrer se você conectar um computador a um servidor que está executando o Windows Server Essentials, enquanto outras atualizações do Windows ou instalações de aplicativos estiverem pendentes e a instalação do conector é cancelada.  
  
 **Solução**  
  
 Conclua todas as outras instalações de aplicativos ou atualizações. Se solicitado, reinicie os computadores.  
  
##  <a name="BKMK_ConnectorIssue2"></a>Problema 2  
 **Problema**  
  
 Não podem ingressar em um computador com o Windows Server Essentials  
  
 **Descrição**  
  
 Computadores que têm caracteres não ASCII em nome do computador não podem ser adicionados ao Windows Server Essentials. Se o nome do computador inclui caracteres não ASCII, você receberá a mensagem de erro "Ocorreu um erro inesperado tem."  
  
 **Solução**  
  
 Renomeie o computador cliente com um nome que contém apenas caracteres ASCII e tente adicioná-lo ao Windows Server Essentials novamente.  
  
##  <a name="BKMK_ConnectorIssue2a"></a>Problema 3  
 **Problema**  
  
 Eu recebo um conector a instalação de software for cancelado erro ao se conectar a um computador para o servidor  
  
 **Descrição**  
  
 Para ser capaz de se conectar a um computador para o servidor, a conta sistema deve ter permissões de controle total sobre pastas de servidor exibidas no painel Windows Server Essentials. Se não tiverem sido concedidas as permissões necessárias, você recebe o erro "a instalação do software conector é cancelada" mensagem.  
  
 **Solução**  
  
 Conceder a conta sistema **controle total** permissões em cada pasta de servidor.  
  
#### <a name="to-grant-the-system-account-full-control-permissions-on-a-server-folder"></a>Para conceder a conta sistema permissões de controle total em uma pasta de servidor  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Clique em **armazenamento**e clique em **pastas de servidor**.  
  
3.  Clique com botão direito na pasta de servidor e, em seguida, clique em **abra a pasta**. (Se você não vir o **abra a pasta** opção, você não precisa definir as permissões na pasta.)  
  
4.  No caminho de rede na parte superior do Windows Explorer, clique no compartilhamento de servidor para exibir pastas compartilhadas no servidor. Por exemplo, se o caminho é **rede > Server01 > Backups de histórico de arquivos**, clique em **Server01**.  
  
5.  Clique com botão direito uma pasta no servidor e, em seguida, clique em **propriedades**.  
  
6.  Clique no **segurança** guia.  
  
7.  Se **controle total** não é permitidos para a conta sistema, clique em **editar**e clique em **sistema**. Em **permissões para o sistema**, selecione o **permitir** caixa de seleção ao lado de **controle total**.  
  
8.  Clique em **Okey** duas vezes para atualizar as permissões e feche **propriedades**.  
  
##  <a name="BKMK_ConnectorIssueNetFramework"></a>Problema 4  
 **Problema**  
  
 Recebi um para executar este aplicativo, você deve instalar uma das seguintes versões do .NET Framework: V4.5.50709 "Erro ao se conectar a um computador para o servidor  
  
 **Descrição**  
  
 Quando você conectar um computador para um servidor que está executando o Windows Server Essentials ou o Windows Server Essentials, o assistente tenta instalar o .NET Framework versão 4.5.50709 no computador. Entretanto, se houver uma versão anterior do .NET Framework versão 4.5, não pode ser instalado a versão atualizada e a tentativa de conexão falhar com essa mensagem de erro: para executar este aplicativo, você deve instalar uma das seguintes versões do .NET Framework: V4.5.50709. Contate seu fornecedor de alocação para obter instruções sobre como obter a versão apropriada do .NET Framework.  
  
 **Solução**  
  
 Desinstale o .NET Framework 4.5 do computador e, em seguida, conecte o computador para o servidor.  
  
#### <a name="to-uninstall-net-framework-45"></a>Para desinstalar o .NET Framework 4.5  
  
1.  Sobre o **iniciar** página do computador cliente, abrir **painel de controle**.  
  
2.  No painel de controle, em **programas**, clique em **desinstalar um programa**.  
  
3.  Clique com botão direito **do Microsoft .NET Framework 4.5**e clique em **desinstalar**.  
  
4.  Após a desinstalação com sucesso do .NET Framework 4.5, conecte o computador para o servidor. A versão correta do .NET Framework 4.5 é instalada juntamente com o software connector.  
  
##  <a name="BKMK_Time"></a>Problema 5  
 **Problema**  
  
 Recebi um servidor não está disponível. Para resolver esse problema, contate a pessoa responsável pela sua rede. Erro ao se conectar a um computador para o servidor  
  
 **Descrição**  
  
 Isso pode acontecer se a data e hora do computador conectado não são sincronizados com a data e hora no servidor.  Windows Server Essentials e Windows Server Essentials usam o serviço de sincronização de tempo para sincronizar a data e hora de computadores que executam em uma rede do Windows Server Essentials ou o Windows Server Essentials. Sincronizar o tempo é crítico, como o protocolo de autenticação padrão usa o tempo de servidor como parte do processo de autenticação. Por exemplo, se o relógio em um computador cliente não é sincronizado com a data e hora corretas, Windows Server Essentials ou autenticação do Windows Server Essentials erroneamente pode interpretar uma solicitação de logon como uma tentativa de invasão e negar o acesso ao usuário.  
  
 Isso pode acontecer se a memória livre do servidor s é menos de 5%.  
  
 Isso pode acontecer se você já tiver uma conexão de VPN para o servidor do Windows Essentials e você tentar definir a conector software fora das instalações usando um endereço de domínio.  
  
 **Solução**  
  
1.  Sincronize a data e hora no computador cliente com os do servidor. Em seguida, conecte o computador para o servidor.  
  
2.  Feche alguns aplicativos no lado do servidor e, em seguida, conecte o computador para o servidor.  
  
3.  Feche a conexão de VPN e, em seguida, conecte o computador para o servidor.  
  
#### <a name="to-change-the-date-and-time-on-the-client-computer"></a>Para alterar a data e hora no computador cliente  
  
1.  Na página inicial do computador cliente, abra **painel de controle**.  
  
2.  No painel de controle, clique em **relógio, idioma e região**e clique em **data e hora**.  
  
3.  Clique em **alterar data e hora**, defina a data e hora para a data e hora corretas e clique em **Okey**.  
  
4.  Clique em **Okey**e feche o painel de controle.  
  
5.  Tente novamente conectar o computador cliente para o servidor. Para obter instruções, consulte conectar computadores para o servidor.  
  
 Se você ainda não conseguir conectar o computador cliente para o servidor, certifique-se de que a data e hora no servidor estão corretas. Se a data e hora não estiverem corretas, alterá-los.  
  
#### <a name="to-change-the-date-and-time-on-the-server"></a>Para alterar a data e hora no servidor  
  
1.  Fazer logon no servidor usando a senha que você deseja configurar durante a instalação do Windows Server Essentials ou Windows Server Essentials e a configuração.  
  
    > [!NOTE]
    >  Se você está gerenciando o servidor remotamente, você deve fazer logon no servidor usando a Conexão de área de trabalho remota.  
  
2.  Sobre o **iniciar** página, abrir **painel de controle**.  
  
3.  No painel de controle, clique em **relógio, idioma e região**e clique em **data e hora**.  
  
4.  Clique em **alterar data e hora**, defina a data e hora para a data e hora corretas e clique em **Okey**.  
  
5.  Clique em **Okey**e feche o painel de controle.  
  
6.  No computador cliente, tente novamente conectar o computador cliente para o servidor. Para obter instruções, consulte conectar computadores para o servidor.  
  
##  <a name="BKMK_ServiceStopped"></a>Problema 6  
 **Problema**  
  
 Recebi um um erro inesperado. Para resolver esse problema, contate a pessoa responsável pela sua rede. Erro ao se conectar a um computador para o servidor  
  
 **Descrição**  
  
 Se você receber essa mensagem de erro, o serviço de Web do WSS certificado talvez não esteja executando.  
  
 **Solução**  
  
 Inicie o serviço de Web do WSS certificado.  
  
#### <a name="to-start-the-wss-certificate-web-service"></a>Para iniciar o serviço de Web do WSS certificado  
  
1.  No servidor do **iniciar** página, abrir **ferramentas administrativas**.  
  
     Em **ferramentas administrativas**, clique com botão direito **Gerenciador de serviços de informações da Internet (IIS)**e clique em **abrir**.  
  
2.  No painel de navegação, clique em **serviço de Web do WSS certificado**.  
  
3.  No **ações** painel, clique em **iniciar**.  
  
##  <a name="BKMK_ConnectorIssueReconnect"></a>Problema 7  
 **Problema**  
  
 Ao tentar se conectar a um computador para o servidor novamente após uma tentativa malsucedida de conexão, eu recebo o aviso de que um computador com esse nome já está conectado ao servidor  
  
 **Descrição**  
  
 Se uma tentativa anterior para se conectar a um computador para o servidor foi cancelada ou interrompida, você pode receber o seguinte aviso quando você tentar se conectar novamente: um computador com esse nome já está conectado ao servidor. Isso ocorre porque um certificado foi emitido quando você tentava para se conectar ao servidor na primeira vez.  
  
 **Solução** se tiver certeza de que nenhum outro computador com o mesmo nome já está conectado ao servidor, clique em **próxima**e siga as instruções para concluir o **conectar meu computador para o servidor** assistente.  
  
##  <a name="BKMK_JoinWin7"></a>Problema 8  
 **Problema**  
  
 Ao tentar se conectar a um computador cliente que está executando o Windows 7 Home para o servidor, a página da web para executar o software do conector é aberto, mas o computador cliente não conseguir se conectar ao servidor  
  
 **Descrição**  
  
 Se o roteador em sua rede tiver multicast habilitado, comunicações entre o servidor e um computador cliente com Windows 7 Home Basic ou Windows 7 Home Premium não funcionam corretamente.  
  
 **Solução**  
  
 Desabilite multicast no roteador. Em alguns roteadores, que podem incluir desabilitar o protocolo de roteamento de cópia do CD - 2M. Para obter mais informações, consulte a documentação fornecida pelo fabricante do roteador.  
  
##  <a name="BKMK_ConnectorIssueAutologon"></a>Problema 9  
 **Problema**  
  
 Logon automático parou de funcionar depois que eu conectei o computador para o servidor  
  
 **Descrição**  
  
 Se o logon automático estiver definida para a conta de usuário quando você conectar o computador com Windows Server Essentials, a configuração é substituída quando o software connector é instalado no computador.  
  
 **Solução** para resolver esse problema, quando você conectar o computador para o servidor, observe a senha que é usada para a conta de usuário. Após o conector de software é instalado, configure o logon automático para usar essa conta.  
  
> [!NOTE]
>  A conta de domínio do Windows Server Essentials requer uma senha que atenda aos requisitos de política de senha padrão.  
  
##  <a name="BKMK_ConnectorIssueOldLogs"></a>Problema 10  
 **Problema**  
  
 Desinstalar uma versão de pré-lançamento do software do conector não remove logs existentes  
  
 **Descrição**  
  
 Depois de atualizar de uma versão de pré-lançamento (Beta ou RC) do Windows Server Essentials para a versão de lançamento, você deve remover o software do conector de cada computador que foi conectado ao servidor e, em seguida, conecte o computador novamente para instalar a versão do software do conector.  
  
 No entanto, quando você remove o software do conector de um computador de rede, os arquivos de log existente na pasta Server\Logs\ %ProgramData%\Microsoft\Windows nesse computador não são excluídos. Se você não excluir a pasta Logs, os arquivos de log podem corrompido quando você conectar o computador para a versão de lançamento do Windows Server Essentials.  
  
 **Solução** para evitar corrupção dos arquivos de log, excluir manualmente a pasta Logs antes de você se reconectar o computador cliente para o servidor atualizado.  
  
#### <a name="to-reconnect-a-computer-after-a-server-update-without-corrupting-the-log-files"></a>Para se reconectar a um computador após a atualização de um servidor sem danificar o log de arquivos  
  
1.  Desinstale o software do conector do computador cliente.  
  
2.  Exclua a pasta Logs da pasta de servidor \ %ProgramData%\Microsoft\Windows.  
  
3.  Conecte o computador para o servidor novamente. Que instala a versão do software do conector, criando uma nova pasta de Logs e arquivos de log.  
  
##  <a name="BKMK_UpgradeClientOS"></a>Problema de 11  
 **Problema**  
  
 Quero atualizar o sistema operacional em um computador cliente  
  
 **Descrição**  
  
 Durante a instalação do software do conector, várias verificações são executadas no sistema operacional de cliente para garantir que o cliente atenda a todos os pré-requisitos do conector. Se você atualizar o sistema operacional cliente depois de instalar o software connector, alguns dos pré-requisitos podem não estar presente e o conector do cliente poderá falhar.  
  
 **Solução**  
  
 Antes de atualizar seu sistema operacional de cliente para uma versão diferente (por exemplo, atualizar o Windows XP para o Windows Vista ou Windows Vista para o Windows 7), você deve desinstalar o software connector. Use **adicionar ou remover programas** no painel de controle. Após a conclusão da atualização do sistema operacional do cliente, você poderá reinstalar o conector do cliente, abrindo http://<;*servidor*> / conectar-se em um navegador da Web, onde <*servidor*> é o nome do servidor Windows Server Essentials.  
  
 Se você já tiver atualizado o cliente com o software do conector instalado, use **Adicionar/remover programas** ou **programas e recursos** para desinstalar o software do conector. Em seguida, instale o software de conector novamente.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar o Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  
-   [Windows 2012 Server Essentials ConnectComputer (Wiki do TechNet) de solução de problemas](https://social.technet.microsoft.com/wiki/contents/articles/14370.windows-2012-server-essentials-connectcomputer-troubleshooting.aspx)
