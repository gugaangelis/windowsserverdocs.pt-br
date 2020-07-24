---
title: bitsadmin
description: Artigo de referência para o comando Bitsadmin, que é uma ferramenta de linha de comando usada para criar, baixar ou carregar trabalhos e monitorar seu progresso.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4853036e-1df8-45ad-8be6-cfb097b8dd27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51a64b059dd9d07dd6bd0ecccb1cd99382bdfaa5
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955368"
---
# <a name="bitsadmin"></a>bitsadmin

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10

Bitsadmin é uma ferramenta de linha de comando usada para criar, baixar ou carregar trabalhos e para monitorar seu progresso. A ferramenta Bitsadmin usa opções para identificar o trabalho a ser executado. Você pode chamar `bitsadmin /?` ou `bitsadmin /help` obter uma lista de opções.

A maioria das opções exige um `<job>` parâmetro, que você define para o nome de exibição do trabalho ou GUID. O nome de exibição de um trabalho não precisa ser exclusivo. As opções **/Create** e **/list** retornam o GUID de um trabalho.

Por padrão, você pode acessar informações sobre seus próprios trabalhos. Para acessar informações para os trabalhos de outro usuário, você deve ter privilégios de administrador. Se o trabalho tiver sido criado em um estado elevado, você deverá executar o **Bitsadmin** em uma janela com privilégios elevados; caso contrário, você terá acesso somente leitura ao trabalho.

Muitas das opções correspondem aos métodos nas [interfaces bits](/windows/win32/bits/bits-interfaces). Para obter detalhes adicionais que podem ser relevantes para o uso de uma opção, consulte o método correspondente.

Use as seguintes opções para criar um trabalho, definir e recuperar as propriedades de um trabalho e monitorar o status de um trabalho. Para obter exemplos que mostram como usar algumas dessas opções para executar tarefas, consulte [exemplos de Bitsadmin](bitsadmin-examples.md).

## <a name="available-switches"></a>Opções disponíveis

- [Bitsadmin/AddFile](bitsadmin-addfile.md)
- [bitsadmin /addfileset](bitsadmin-addfileset.md)
- [bitsadmin /addfilewithranges](bitsadmin-addfilewithranges.md)
- [Bitsadmin/cache](bitsadmin-cache.md)
- [Bitsadmin/cache/Delete](bitsadmin-cache-and-delete.md)
- [Bitsadmin/cache/DeleteUrl](bitsadmin-cache-and-deleteurl.md)
- [Bitsadmin/cache/getexpirationtime](bitsadmin-cache-and-getexpirationtime.md)
- [Bitsadmin/cache/getlimit](bitsadmin-cache-and-getlimit.md)
- [Bitsadmin/cache/Help](bitsadmin-cache-and-help.md)
- [Bitsadmin/cache/info](bitsadmin-cache-and-info.md)
- [Bitsadmin/cache/List](bitsadmin-cache-and-list.md)
- [Bitsadmin/cache/setexpirationtime](bitsadmin-cache-and-setexpirationtime.md)
- [Bitsadmin/cache/setlimit](bitsadmin-cache-and-setlimit.md)
- [Bitsadmin/cache/Clear](bitsadmin-cache-clear.md)
- [bitsadmin /cancel](bitsadmin-cancel.md)
- [bitsadmin /complete](bitsadmin-complete.md)
- [Bitsadmin/Create](bitsadmin-create.md)
- [bitsadmin /examples](bitsadmin-examples.md)
- [bitsadmin /getaclflags](bitsadmin-getaclflags.md)
- [bitsadmin /getbytestotal](bitsadmin-getbytestotal.md)
- [bitsadmin /getbytestransferred](bitsadmin-getbytestransferred.md)
- [bitsadmin /getclientcertificate](bitsadmin-getclientcertificate.md)
- [bitsadmin /getcompletiontime](bitsadmin-getcompletiontime.md)
- [bitsadmin /getcreationtime](bitsadmin-getcreationtime.md)
- [bitsadmin /getcustomheaders](bitsadmin-getcustomheaders.md)
- [bitsadmin /getdescription](bitsadmin-getdescription.md)
- [bitsadmin /getdisplayname](bitsadmin-getdisplayname.md)
- [bitsadmin /geterror](bitsadmin-geterror.md)
- [bitsadmin /geterrorcount](bitsadmin-geterrorcount.md)
- [bitsadmin /getfilestotal](bitsadmin-getfilestotal.md)
- [bitsadmin /getfilestransferred](bitsadmin-getfilestransferred.md)
- [bitsadmin /gethelpertokenflags](bitsadmin-gethelpertokenflags.md)
- [bitsadmin /gethelpertokensid](bitsadmin-gethelpertokensid.md)
- [bitsadmin /gethttpmethod](bitsadmin-gethttpmethod.md)
- [bitsadmin /getmaxdownloadtime](bitsadmin-getmaxdownloadtime.md)
- [bitsadmin /getminretrydelay](bitsadmin-getminretrydelay.md)
- [bitsadmin /getmodificationtime](bitsadmin-getmodificationtime.md)
- [bitsadmin /getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)
- [bitsadmin /getnotifycmdline](bitsadmin-getnotifycmdline.md)
- [bitsadmin /getnotifyflags](bitsadmin-getnotifyflags.md)
- [bitsadmin /getnotifyinterface](bitsadmin-getnotifyinterface.md)
- [bitsadmin /getowner](bitsadmin-getowner.md)
- [bitsadmin /getpeercachingflags](bitsadmin-getpeercachingflags.md)
- [bitsadmin /getpriority](bitsadmin-getpriority.md)
- [bitsadmin /getproxybypasslist](bitsadmin-getproxybypasslist.md)
- [bitsadmin /getproxylist](bitsadmin-getproxylist.md)
- [bitsadmin /getproxyusage](bitsadmin-getproxyusage.md)
- [bitsadmin /getreplydata](bitsadmin-getreplydata.md)
- [bitsadmin /getreplyfilename](bitsadmin-getreplyfilename.md)
- [bitsadmin /getreplyprogress](bitsadmin-getreplyprogress.md)
- [bitsadmin /getsecurityflags](bitsadmin-getsecurityflags.md)
- [bitsadmin /getstate](bitsadmin-getstate.md)
- [bitsadmin /gettemporaryname](bitsadmin-gettemporaryname.md)
- [bitsadmin /gettype](bitsadmin-gettype.md)
- [bitsadmin /getvalidationstate](bitsadmin-getvalidationstate.md)
- [Bitsadmin/Help](bitsadmin-help.md)
- [Bitsadmin/info](bitsadmin-info.md)
- [Bitsadmin/List](bitsadmin-list.md)
- [bitsadmin /listfiles](bitsadmin-listfiles.md)
- [bitsadmin /makecustomheaderswriteonly](bitsadmin-makecustomheaderswriteonly.md)
- [Bitsadmin/monitor](bitsadmin-monitor.md)
- [bitsadmin /nowrap](bitsadmin-nowrap.md)
- [bitsadmin /peercaching](bitsadmin-peercaching.md)
- [bitsadmin /peercaching /getconfigurationflags](bitsadmin-peercaching-and-getconfigurationflags.md)
- [Bitsadmin/peercaching/Help](bitsadmin-peercaching-and-help.md)
- [bitsadmin /peercaching /setconfigurationflags](bitsadmin-peercaching-and-setconfigurationflags.md)
- [bitsadmin /peers](bitsadmin-peers.md)
- [Bitsadmin/Peers/Clear](bitsadmin-peers-and-clear.md)
- [bitsadmin /peers /discover](bitsadmin-peers-and-discover.md)
- [Bitsadmin/Peers/Help](bitsadmin-peers-and-help.md)
- [Bitsadmin/Peers/List](bitsadmin-peers-and-list.md)
- [bitsadmin /rawreturn](bitsadmin-rawreturn.md)
- [bitsadmin /removeclientcertificate](bitsadmin-removeclientcertificate.md)
- [bitsadmin /removecredentials](bitsadmin-removecredentials.md)
- [bitsadmin /replaceremoteprefix](bitsadmin-replaceremoteprefix.md)
- [Bitsadmin/Reset reiniciar](bitsadmin-reset.md)
- [Bitsadmin/resume](bitsadmin-resume.md)
- [bitsadmin /setaclflag](bitsadmin-setaclflag.md)
- [bitsadmin /setclientcertificatebyid](bitsadmin-setclientcertificatebyid.md)
- [bitsadmin /setclientcertificatebyname](bitsadmin-setclientcertificatebyname.md)
- [bitsadmin /setcredentials](bitsadmin-setcredentials.md)
- [bitsadmin /setcustomheaders](bitsadmin-setcustomheaders.md)
- [bitsadmin /setdescription](bitsadmin-setdescription.md)
- [bitsadmin /setdisplayname](bitsadmin-setdisplayname.md)
- [bitsadmin /sethelpertoken](bitsadmin-sethelpertoken.md)
- [bitsadmin /sethelpertokenflags](bitsadmin-sethelpertokenflags.md)
- [bitsadmin /sethttpmethod](bitsadmin-sethttpmethod.md)
- [bitsadmin /setmaxdownloadtime](bitsadmin-setmaxdownloadtime.md)
- [bitsadmin /setminretrydelay](bitsadmin-setminretrydelay.md)
- [bitsadmin /setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)
- [bitsadmin /setnotifycmdline](bitsadmin-setnotifycmdline.md)
- [bitsadmin /setnotifyflags](bitsadmin-setnotifyflags.md)
- [bitsadmin /setpeercachingflags](bitsadmin-setpeercachingflags.md)
- [bitsadmin /setpriority](bitsadmin-setpriority.md)
- [bitsadmin /setproxysettings](bitsadmin-setproxysettings.md)
- [bitsadmin /setreplyfilename](bitsadmin-setreplyfilename.md)
- [bitsadmin /setsecurityflags](bitsadmin-setsecurityflags.md)
- [bitsadmin /setvalidationstate](bitsadmin-setvalidationstate.md)
- [bitsadmin /suspend](bitsadmin-suspend.md)
- [bitsadmin /takeownership](bitsadmin-takeownership.md)
- [bitsadmin /transfer](bitsadmin-transfer.md)
- [bitsadmin /util](bitsadmin-util.md)
- [bitsadmin /util /enableanalyticchannel](bitsadmin-util-and-enableanalyticchannel.md)
- [bitsadmin /util /getieproxy](bitsadmin-util-and-getieproxy.md)
- [Bitsadmin/util/Help](bitsadmin-util-and-help.md)
- [bitsadmin /util /repairservice](bitsadmin-util-and-repairservice.md)
- [bitsadmin /util /setieproxy](bitsadmin-util-and-setieproxy.md)
- [Bitsadmin/util/Version](bitsadmin-util-and-version.md)
- [bitsadmin /wrap](bitsadmin-wrap.md)
