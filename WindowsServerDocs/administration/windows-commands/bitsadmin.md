---
title: bitsadmin
description: Tópico de comandos do Windows para **bitsadmin** -bitsadmin é uma ferramenta de linha de comando que você pode usar para criar, baixar, ou trabalhos de carregamento e monitorar seu andamento.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4853036e-1df8-45ad-8be6-cfb097b8dd27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da0f05ec716cffb7d7532ebac50a091729a6bb18
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821067"
---
# <a name="bitsadmin"></a>bitsadmin

> **Aplica-se a**: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10

Bitsadmin é uma ferramenta de linha de comando que você pode usar para criar o download ou trabalhos de carregamento e monitorar seu andamento. A ferramenta bitsadmin usa comutadores para identificar o trabalho a ser executado.  Você pode chamar `bitsadmin /?` ou `bitsadmin /HELP` para obter uma lista de opções.

A maioria dos comutadores exige uma \<trabalho\> parâmetro que você definir como o nome de exibição ou GUID do trabalho. Observe que o nome de exibição do trabalho pode não ser exclusivo. O **/Create** e **/lista** comutadores retornam o GUID de um trabalho.

Por padrão, você pode acessar informações sobre seus próprios trabalhos. Para acessar informações de trabalhos de outro usuário, você deve ter privilégios de administrador. Se o trabalho foi criado em um estado com privilégios elevados, em seguida, você deve executar bitsadmin em uma janela elevada; Caso contrário, você terá acesso somente leitura para o trabalho.

Muitas das opções correspondem aos métodos na [interfaces BITS](/windows/desktop/bits/bits-interfaces). Para obter detalhes adicionais que podem ser relevantes usando um comutador, consulte o método correspondente.

Use as seguintes opções para criar um trabalho, definir e recuperar as propriedades de um trabalho e monitorar o status de um trabalho. Para obter exemplos que mostram como usar algumas dessas opções para executar tarefas, consulte [bitsadmin exemplos](bitsadmin-examples.md).

## <a name="switches"></a>Comutadores

[Bitsadmin addfile](bitsadmin-addfile.md)  
[Bitsadmin addfileset](bitsadmin-addfileset.md)  
[Bitsadmin addfilewithranges](bitsadmin-addfilewithranges.md)  
[cache Bitsadmin](bitsadmin-cache.md)  
[Cancelar Bitsadmin](bitsadmin-cancel.md)  
[Bitsadmin concluída](bitsadmin-complete.md)  
[Criar Bitsadmin](bitsadmin-create.md)  
[Bitsadmin getaclflags](bitsadmin-getaclflags.md)  
[Bitsadmin getbytestotal](bitsadmin-getbytestotal.md)  
[Bitsadmin getbytestransferred](bitsadmin-getbytestransferred.md)  
[Bitsadmin getclientcertificate](bitsadmin-getclientcertificate.md)  
[Bitsadmin getcompletiontime](bitsadmin-getcompletiontime.md)  
[getcreationtime Bitsadmin](bitsadmin-getcreationtime.md)  
[Bitsadmin getcustomheaders](bitsadmin-getcustomheaders.md)  
[Bitsadmin getdescription](bitsadmin-getdescription.md)  
[Bitsadmin getdisplayname](bitsadmin-getdisplayname.md)  
[Bitsadmin geterror](bitsadmin-geterror.md)  
[Bitsadmin geterrorcount](bitsadmin-geterrorcount.md)  
[Bitsadmin getfilestotal](bitsadmin-getfilestotal.md)  
[Bitsadmin getfilestransferred](bitsadmin-getfilestransferred.md)  
[Bitsadmin gethelpertokenflags](bitsadmin-gethelpertokenflags.md)  
[Bitsadmin gethelpertokensid](bitsadmin-gethelpertokensid.md)  
[Bitsadmin gethttpmethod](bitsadmin-gethttpmethod.md)
[getmaxdownloadtime bitsadmin](bitsadmin-getmaxdownloadtime.md)  
[Bitsadmin getminretrydelay](bitsadmin-getminretrydelay.md)  
[Bitsadmin getmodificationtime](bitsadmin-getmodificationtime.md)  
[Bitsadmin getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)  
[Bitsadmin getnotifycmdline](bitsadmin-getnotifycmdline.md)  
[Bitsadmin getnotifyflags](bitsadmin-getnotifyflags.md)  
[Bitsadmin getnotifyinterface](bitsadmin-getnotifyinterface.md)  
[Bitsadmin getowner](bitsadmin-getowner.md)  
[Bitsadmin getpeercachingflags](bitsadmin-getpeercachingflags.md)  
[Bitsadmin getpriority](bitsadmin-getpriority.md)  
[Bitsadmin getproxybypasslist](bitsadmin-getproxybypasslist.md)  
[Bitsadmin getproxylist](bitsadmin-getproxylist.md)  
[Bitsadmin getproxyusage](bitsadmin-getproxyusage.md)  
[Bitsadmin getreplydata](bitsadmin-getreplydata.md)  
[Bitsadmin getreplyfilename](bitsadmin-getreplyfilename.md)  
[Bitsadmin getreplyprogress](bitsadmin-getreplyprogress.md)  
[Bitsadmin getsecurityflags](bitsadmin-getsecurityflags.md)  
[Bitsadmin getstate](bitsadmin-getstate.md)  
[Bitsadmin gettemporaryname](bitsadmin-gettemporaryname.md)  
[Bitsadmin gettype](bitsadmin-gettype.md)  
[Bitsadmin getvalidationstate](bitsadmin-getvalidationstate.md)  
[Ajuda de Bitsadmin](bitsadmin-help.md)  
[informações de Bitsadmin](bitsadmin-info.md)  
[lista de Bitsadmin](bitsadmin-list.md)  
[Bitsadmin listfiles](bitsadmin-listfiles.md)  
[Bitsadmin makecustomheaderswriteonly](bitsadmin-makecustomheaderswriteonly.md)
[bitsadmin monitor](bitsadmin-monitor.md)  
[Bitsadmin nowrap](bitsadmin-nowrap.md)  
[cache par do Bitsadmin](bitsadmin-peercaching.md)  
[Bitsadmin pares](bitsadmin-peers.md)  
[Bitsadmin rawreturn](bitsadmin-rawreturn.md)  
[Bitsadmin removeclientcertificate](bitsadmin-removeclientcertificate.md)  
[Bitsadmin removecredentials](bitsadmin-removecredentials.md)  
[Bitsadmin replaceremoteprefix](bitsadmin-replaceremoteprefix.md)  
[redefinição Bitsadmin](bitsadmin-reset.md)  
[retomar Bitsadmin](bitsadmin-resume.md)  
[Bitsadmin setaclflag](bitsadmin-setaclflag.md)  
[Bitsadmin setclientcertificatebyid](bitsadmin-setclientcertificatebyid.md)  
[Bitsadmin setclientcertificatebyname](bitsadmin-setclientcertificatebyname.md)  
[Bitsadmin setcredentials](bitsadmin-setcredentials.md)  
[Bitsadmin setcustomheaders](bitsadmin-setcustomheaders.md)  
[Bitsadmin setdescription](bitsadmin-setdescription.md)  
[Bitsadmin setdisplayname](bitsadmin-setdisplayname.md)  
[Bitsadmin sethelpertoken](bitsadmin-sethelpertoken.md)  
[Bitsadmin sethelpertokenflags](bitsadmin-sethelpertokenflags.md)  
[Bitsadmin sethttpmethod](bitsadmin-sethttpmethod.md)
[setmaxdownloadtime bitsadmin](bitsadmin-setmaxdownloadtime.md)  
[Bitsadmin setminretrydelay](bitsadmin-setminretrydelay.md)  
[Bitsadmin setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)  
[Bitsadmin setnotifycmdline](bitsadmin-setnotifycmdline.md)  
[Bitsadmin setnotifyflags](bitsadmin-setnotifyflags.md)  
[bitsadmin setpeercachingflags](bitsadmin-setpeercachingflags.md)  
[Bitsadmin setpriority](bitsadmin-setpriority.md)  
[Bitsadmin setproxysettings](bitsadmin-setproxysettings.md)  
[Bitsadmin setreplyfilename](bitsadmin-setreplyfilename.md)  
[Bitsadmin setsecurityflags](bitsadmin-setsecurityflags.md)  
[Bitsadmin setvalidationstate](bitsadmin-setvalidationstate.md)  
[Suspender Bitsadmin](bitsadmin-suspend.md)  
[Bitsadmin takeownership](bitsadmin-takeownership.md)  
[transferência de Bitsadmin](bitsadmin-transfer.md)  
[util Bitsadmin](bitsadmin-util.md)  
[encapsulamento Bitsadmin](bitsadmin-wrap.md)  
