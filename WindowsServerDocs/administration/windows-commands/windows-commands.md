---
title: Comandos do Windows
description: Referência
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c703d07c-8227-4e86-94a6-8ef390f94cdc
author: jasongerend
ms.author: jgerend
manager: dongill
ms.date: 06/09/2020
ms.prod: windows-server
ms.openlocfilehash: 66de80652f764840af70e88dfd39df2398ae495c
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721109"
---
# <a name="windows-commands"></a>Comandos do Windows

Todas as versões com suporte do Windows (servidor e cliente) têm um conjunto de comandos do console do Win32 internos.

Este conjunto de documentação descreve os comandos do Windows que você pode usar para automatizar tarefas usando scripts ou ferramentas de script.

Para localizar informações sobre um comando específico, no menu a-Z a seguir, clique na letra com a qual o comando começa e, em seguida, clique no nome do comando.

[Um](#a)  |
 [B](#b)  |
 [C](#c)  |
 [D](#d)  |
 [E](#e)  |
 [F](#f)  |
 [G](#g)  |
 [H](#h)  |
 [I](#i)  |
 [J](#j)  |
 [K](#k)  |
 [L](#l)  |
 [M](#m)  |
 [N](#n)  |
 [O](#o)  |
 [P](#p)  |
 [P](#q)  |
 [R](#r)  |
 [S](#s)  |
 [T](#t)  |
 [U](#u)  |
 [V](#v)  |
 [W](#w)  |
 [X](#x) | Y | Z

## <a name="prerequisites"></a>Pré-requisitos

As informações contidas neste tópico aplicam-se a:

- Windows Server 2019
- Windows Server (Canal semestral)
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2
- Windows Server 2008
- Windows 10
- Windows 8.1

### <a name="command-shell-overview"></a>Visão geral do Shell de comando

O Shell de comando foi o primeiro Shell interno do Windows para automatizar tarefas rotineiras, como o gerenciamento de contas de usuário ou backups noturnos, com arquivos de lote (. bat). Com o Windows Script Host, você poderia executar scripts mais sofisticados no Shell de comando. Para obter mais informações, consulte [cscript](cscript.md) ou [WScript](wscript.md). Você pode executar operações com mais eficiência usando scripts do que é possível usando a interface do usuário. Os scripts aceitam todos os comandos que estão disponíveis na linha de comando.

O Windows tem dois shells de comando: o Shell de comando e o [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6). Cada shell é um programa de software que fornece comunicação direta entre você e o sistema operacional ou aplicativo, fornecendo um ambiente para automatizar operações de ti.

O PowerShell foi projetado para estender os recursos do Shell de comando para executar comandos do PowerShell chamados cmdlets. Os cmdlets são semelhantes aos comandos do Windows, mas fornecem uma linguagem de script mais extensível. Você pode executar comandos do Windows e cmdlets do PowerShell no PowerShell, mas o Shell de comando só pode executar comandos do Windows e não cmdlets do PowerShell.

Para a automação do Windows mais robusta e atualizada, recomendamos o uso do PowerShell em vez de comandos do Windows ou do Windows Script Host para automação do Windows.

> [!NOTE]
>Você também pode baixar e instalar o [PowerShell Core](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6), a versão de código aberto do PowerShell.

> [!CAUTION]
> A edição incorreta do Registro pode causar danos graves ao sistema. Antes de fazer as alterações a seguir no registro, você deve fazer backup de todos os dados importantes no computador.

> [!NOTE]
> Para habilitar ou desabilitar a conclusão de nome de arquivo e diretório no Shell de comando em um computador ou sessão de logon de usuário, execute **regedit.exe** e defina o seguinte **valor de reg_DWOrd**:
>
> HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\completionChar\ reg_DWOrd
>
> Para definir o valor de **reg_DWOrd** , use o valor hexadecimal de um caractere de controle para uma função específica (por exemplo, **0 9** é Tab e **0 08** é Backspace). As configurações especificadas pelo usuário têm precedência sobre as configurações do computador, e as opções de linha de comando têm precedência sobre as configurações do registro.

## <a name="command-line-reference-a-z"></a>Referência de linha de comando A-Z

Para encontrar informações sobre um comando específico do Windows, no menu a-Z a seguir, clique na letra com a qual o comando começa e, em seguida, clique no nome do comando.

[Um](#a)  |
 [B](#b)  |
 [C](#c)  |
 [D](#d)  |
 [E](#e)  |
 [F](#f)  |
 [G](#g)  |
 [H](#h)  |
 [I](#i)  |
 [J](#j)  |
 [K](#k)  |
 [L](#l)  |
 [M](#m)  |
 [N](#n)  |
 [O](#o)  |
 [P](#p)  |
 [P](#q)  |
 [R](#r)  |
 [S](#s)  |
 [T](#t)  |
 [U](#u)  |
 [V](#v)  |
 [W](#w)  |
 [X](#x) | Y | Z

### <a name="a"></a>Um

- [active](active.md)
- [add](add.md)
- [add alias](add-alias.md)
- [add volume](add-volume.md)
- [append](append.md)
- [arp](arp.md)
- [assign](assign.md)
- [assoc](assoc.md)
- [at](at.md)
- [atmadm](atmadm.md)
- [attach-vdisk](attach-vdisk.md)
- [attrib](attrib.md)
- [attributes](attributes.md)
  - [attributes disk](attributes-disk.md)
  - [attributes volume](attributes-volume.md)
- [auditpol](auditpol.md)
  - [auditpol backup](auditpol-backup.md)
  - [auditpol clear](auditpol-clear.md)
  - [auditpol get](auditpol-get.md)
  - [auditpol list](auditpol-list.md)
  - [auditpol remove](auditpol-remove.md)
  - [auditpol resourcesacl](auditpol-resourcesacl.md)
  - [auditpol restore](auditpol-restore.md)
  - [auditpol set](auditpol-set.md)
- [autochk](autochk.md)
- [autoconv](autoconv.md)
- [autofmt](autofmt.md)
- [automount](automount.md)

### <a name="b"></a>B

- [bcdboot](bcdboot.md)
- [bcdedit](bcdedit.md)
- [bdehdcfg](bdehdcfg.md)
  - [bdehdcfg driveinfo](bdehdcfg-driveinfo.md)
  - [bdehdcfg newdriveletter](bdehdcfg-newdriveletter.md)
  - [bdehdcfg quiet](bdehdcfg-quiet.md)
  - [bdehdcfg restart](bdehdcfg-restart.md)
  - [bdehdcfg size](bdehdcfg-size.md)
  - [bdehdcfg target](bdehdcfg-target.md)
- [begin backup](begin-backup.md)
- [begin restore](begin-restore.md)
- [bitsadmin](bitsadmin.md)
  - [bitsadmin addfile](bitsadmin-addfile.md)
  - [bitsadmin addfileset](bitsadmin-addfileset.md)
  - [bitsadmin addfilewithranges](bitsadmin-addfilewithranges.md)
  - [bitsadmin cache](bitsadmin-cache.md)
    - [bitsadmin cache and delete](bitsadmin-cache-and-delete.md)
    - [bitsadmin cache and deleteurl](bitsadmin-cache-and-deleteurl.md)
    - [bitsadmin cache and getexpirationtime](bitsadmin-cache-and-getexpirationtime.md)
    - [bitsadmin cache and getlimit](bitsadmin-cache-and-getlimit.md)
    - [bitsadmin cache and help](bitsadmin-cache-and-help.md)
    - [bitsadmin cache and info](bitsadmin-cache-and-info.md)
    - [bitsadmin cache and list](bitsadmin-cache-and-list.md)
    - [bitsadmin cache and setexpirationtime](bitsadmin-cache-and-setexpirationtime.md)
    - [bitsadmin cache and setlimit](bitsadmin-cache-and-setlimit.md)
    - [bitsadmin cache and clear](bitsadmin-cache-clear.md)
  - [bitsadmin cancel](bitsadmin-cancel.md)
  - [bitsadmin complete](bitsadmin-complete.md)
  - [bitsadmin create](bitsadmin-create.md)
  - [bitsadmin examples](bitsadmin-examples.md)
  - [bitsadmin getaclflags](bitsadmin-getaclflags.md)
  - [bitsadmin getbytestotal](bitsadmin-getbytestotal.md)
  - [bitsadmin getbytestransferred](bitsadmin-getbytestransferred.md)
  - [bitsadmin getclientcertificate](bitsadmin-getclientcertificate.md)
  - [bitsadmin getcompletiontime](bitsadmin-getcompletiontime.md)
  - [bitsadmin getcreationtime](bitsadmin-getcreationtime.md)
  - [bitsadmin getcustomheaders](bitsadmin-getcustomheaders.md)
  - [bitsadmin getdescription](bitsadmin-getdescription.md)
  - [bitsadmin getdisplayname](bitsadmin-getdisplayname.md)
  - [bitsadmin geterror](bitsadmin-geterror.md)
  - [bitsadmin geterrorcount](bitsadmin-geterrorcount.md)
  - [bitsadmin getfilestotal](bitsadmin-getfilestotal.md)
  - [bitsadmin getfilestransferred](bitsadmin-getfilestransferred.md)
  - [bitsadmin gethelpertokenflags](bitsadmin-gethelpertokenflags.md)
  - [bitsadmin gethelpertokensid](bitsadmin-gethelpertokensid.md)
  - [bitsadmin gethttpmethod](bitsadmin-gethttpmethod.md)
  - [bitsadmin getmaxdownloadtime](bitsadmin-getmaxdownloadtime.md)
  - [bitsadmin getminretrydelay](bitsadmin-getminretrydelay.md)
  - [bitsadmin getmodificationtime](bitsadmin-getmodificationtime.md)
  - [bitsadmin getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)
  - [bitsadmin getnotifycmdline](bitsadmin-getnotifycmdline.md)
  - [bitsadmin getnotifyflags](bitsadmin-getnotifyflags.md)
  - [bitsadmin getnotifyinterface](bitsadmin-getnotifyinterface.md)
  - [bitsadmin getowner](bitsadmin-getowner.md)
  - [bitsadmin getpeercachingflags](bitsadmin-getpeercachingflags.md)
  - [bitsadmin getpriority](bitsadmin-getpriority.md)
  - [bitsadmin getproxybypasslist](bitsadmin-getproxybypasslist.md)
  - [bitsadmin getproxylist](bitsadmin-getproxylist.md)
  - [bitsadmin getproxyusage](bitsadmin-getproxyusage.md)
  - [bitsadmin getreplydata](bitsadmin-getreplydata.md)
  - [bitsadmin getreplyfilename](bitsadmin-getreplyfilename.md)
  - [bitsadmin getreplyprogress](bitsadmin-getreplyprogress.md)
  - [bitsadmin getsecurityflags](bitsadmin-getsecurityflags.md)
  - [bitsadmin getstate](bitsadmin-getstate.md)
  - [bitsadmin gettemporaryname](bitsadmin-gettemporaryname.md)
  - [bitsadmin gettype](bitsadmin-gettype.md)
  - [bitsadmin getvalidationstate](bitsadmin-getvalidationstate.md)
  - [bitsadmin help](bitsadmin-help.md)
  - [bitsadmin info](bitsadmin-info.md)
  - [bitsadmin list](bitsadmin-list.md)
  - [bitsadmin listfiles](bitsadmin-listfiles.md)
  - [bitsadmin makecustomheaderswriteonly](bitsadmin-makecustomheaderswriteonly.md)
  - [bitsadmin monitor](bitsadmin-monitor.md)
  - [bitsadmin nowrap](bitsadmin-nowrap.md)
  - [bitsadmin peercaching](bitsadmin-peercaching.md)
    - [bitsadmin peercaching e getconfigurationflags](bitsadmin-peercaching-and-getconfigurationflags.md)
    - [bitsadmin peercaching e help](bitsadmin-peercaching-and-help.md)
    - [bitsadmin peercaching e setconfigurationflags](bitsadmin-peercaching-and-setconfigurationflags.md)
  - [bitsadmin peers](bitsadmin-peers.md)
    - [bitsadmin peers e clear](bitsadmin-peers-and-clear.md)
    - [bitsadmin peers e discover](bitsadmin-peers-and-discover.md)
    - [bitsadmin peers e help](bitsadmin-peers-and-help.md)
    - [bitsadmin peers e list](bitsadmin-peers-and-list.md)
  - [bitsadmin rawreturn](bitsadmin-rawreturn.md)
  - [bitsadmin removeclientcertificate](bitsadmin-removeclientcertificate.md)
  - [bitsadmin removecredentials](bitsadmin-removecredentials.md)
  - [bitsadmin replaceremoteprefix](bitsadmin-replaceremoteprefix.md)
  - [bitsadmin reset](bitsadmin-reset.md)
  - [bitsadmin resume](bitsadmin-resume.md)
  - [bitsadmin setaclflag](bitsadmin-setaclflag.md)
  - [bitsadmin setclientcertificatebyid](bitsadmin-setclientcertificatebyid.md)
  - [bitsadmin setclientcertificatebyname](bitsadmin-setclientcertificatebyname.md)
  - [bitsadmin setcredentials](bitsadmin-setcredentials.md)
  - [bitsadmin setcustomheaders](bitsadmin-setcustomheaders.md)
  - [bitsadmin setdescription](bitsadmin-setdescription.md)
  - [bitsadmin setdisplayname](bitsadmin-setdisplayname.md)
  - [bitsadmin sethelpertoken](bitsadmin-sethelpertoken.md)
  - [bitsadmin sethelpertokenflags](bitsadmin-sethelpertokenflags.md)
  - [bitsadmin sethttpmethod](bitsadmin-sethttpmethod.md)
  - [bitsadmin setmaxdownloadtime](bitsadmin-setmaxdownloadtime.md)
  - [bitsadmin setminretrydelay](bitsadmin-setminretrydelay.md)
  - [bitsadmin setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)
  - [bitsadmin setnotifycmdline](bitsadmin-setnotifycmdline.md)
  - [bitsadmin setnotifyflags](bitsadmin-setnotifyflags.md)
  - [bitsadmin setpeercachingflags](bitsadmin-setpeercachingflags.md)
  - [bitsadmin setpriority](bitsadmin-setpriority.md)
  - [bitsadmin setproxysettings](bitsadmin-setproxysettings.md)
  - [bitsadmin setreplyfilename](bitsadmin-setreplyfilename.md)
  - [bitsadmin setsecurityflags](bitsadmin-setsecurityflags.md)
  - [bitsadmin setvalidationstate](bitsadmin-setvalidationstate.md)
  - [bitsadmin suspend](bitsadmin-suspend.md)
  - [bitsadmin takeownership](bitsadmin-takeownership.md)
  - [bitsadmin transfer](bitsadmin-transfer.md)
  - [bitsadmin util](bitsadmin-util.md)
    - [bitsadmin util e enableanalyticchannel](bitsadmin-util-and-enableanalyticchannel.md)
    - [bitsadmin util e getieproxy](bitsadmin-util-and-getieproxy.md)
    - [bitsadmin util e help](bitsadmin-util-and-help.md)
    - [bitsadmin util e repairservice](bitsadmin-util-and-repairservice.md)
    - [bitsadmin util e setieproxy](bitsadmin-util-and-setieproxy.md)
    - [bitsadmin util e version](bitsadmin-util-and-version.md)
  - [bitsadmin wrap](bitsadmin-wrap.md)
- [bootcfg](bootcfg.md)
  - [bootcfg addsw](bootcfg-addsw.md)
  - [bootcfg copy](bootcfg-copy.md)
  - [bootcfg dbg1394](bootcfg-dbg1394.md)
  - [bootcfg debug](bootcfg-debug.md)
  - [bootcfg default](bootcfg-default.md)
  - [bootcfg delete](bootcfg-delete.md)
  - [bootcfg ems](bootcfg-ems.md)
  - [bootcfg query](bootcfg-query.md)
  - [bootcfg raw](bootcfg-raw.md)
  - [bootcfg rmsw](bootcfg-rmsw.md)
  - [bootcfg timeout](bootcfg-timeout.md)
- [break](break_1.md)

### <a name="c"></a>C

- [cacls](cacls.md)
- [call](call.md)
- [cd](cd.md)
- [certreq](certreq_1.md)
- [certutil](certutil.md)
- [change](change.md)
  - [change logon](change-logon.md)
  - [change port](change-port.md)
  - [change user](change-user.md)
- [chcp](chcp.md)
- [chdir](chdir.md)
- [chglogon](chglogon.md)
- [chgport](chgport.md)
- [chgusr](chgusr.md)
- [chkdsk](chkdsk.md)
- [chkntfs](chkntfs.md)
- [choice](choice.md)
- [cipher](cipher.md)
- [clean](clean.md)
- [cleanmgr](cleanmgr.md)
- [clip](clip.md)
- [cls](cls.md)
- [cmd](Cmd.md)
- [cmdkey](cmdkey.md)
- [cmstp](cmstp.md)
- [color](color.md)
- [comp](comp.md)
- [compact](compact.md)
- [Compact vdisk](compact-vdisk.md)
- [convert](convert.md)
  - [convert basic](convert-basic.md)
  - [converter dinâmico](convert-dynamic.md)
  - [convert gpt](convert-gpt.md)
  - [convert mbr](convert-mbr.md)
- [copy](copy.md)
- [cprofile](cprofile.md)
- [create](create.md)
  - [criar partição EFI](create-partition-efi.md)
  - [criar [partição estendida](create-partition-extended.md)
  - [criar partição lógica](create-partition-logical.md)
  - [criar partição MSR](create-partition-msr.md)
  - [create partition primary](create-partition-primary.md)
  - [criar espelho de volume](create-volume-mirror.md)
  - [criar RAID de volume](create-volume-raid.md)
  - [criar volume simples](create-volume-simple.md)
  - [criar faixa de volume](create-volume-stripe.md)
- [cscript](cscript.md)

### <a name="d"></a>D

- [date](date.md)
- [dcgpofix](dcgpofix.md)
- [defrag](defrag.md)
- [del](del.md)
- [delete](delete.md)
  - [excluir disco](delete-disk.md)
  - [Excluir partição](delete-partition.md)
  - [excluir sombras](delete-shadows.md)
  - [delete volume](delete-volume.md)
- [detach vdisk](detach-vdisk.md)
- [detalhes](detail.md)
  - [disco de detalhes](detail-disk.md)
  - [partição de detalhes](detail-partition.md)
  - [detalhes do VDISK](detail-vdisk.md)
  - [volume de detalhes](detail-volume.md)
- [dfsdiag](dfsdiag.md)
  - [dfsdiag testdcs](dfsdiag-testdcs.md)
  - [dfsdiag testdfsconfig](dfsdiag-testdfsconfig.md)
  - [dfsdiag testdfsintegrity](dfsdiag-testdfsintegrity.md)
  - [dfsdiag testreferral](dfsdiag-testreferral.md)
  - [dfsdiag testsites](dfsdiag-testsites.md)
- [dfsrmig](dfsrmig.md)
- [diantz](diantz.md)
- [dir](dir.md)
- [diskcomp](diskcomp.md)
- [diskcopy](diskcopy.md)
- [diskpart](diskpart.md)
- [diskperf](diskperf.md)
- [diskraid](diskraid.md)
- [diskshadow](diskshadow.md)
- [dispdiag](dispdiag.md)
- [dnscmd](dnscmd.md)
- [doskey](doskey.md)
- [driverquery](driverquery.md)

### <a name="e"></a>E

- [echo](echo.md)
- [edit](edit.md)
- [endlocal](endlocal.md)
- [finalizar restauração](end-restore.md)
- [erase](erase.md)
- [eventcreate](eventcreate.md)
- [eventquery](eventquery.md)
- [eventtriggers](eventtriggers.md)
- [Evntcmd](evntcmd.md)
- [exec](exec.md)
- [exit](exit_2.md)
- [expand](expand.md)
- [expandir vdisk](expand-vdisk.md)
- [Expo](expose.md)
- [extend](extend.md)
- [extract](extract.md)

### <a name="f"></a>F

- [fc](fc.md)
- [filesystems](filesystems.md)
- [find](find.md)
- [findstr](findstr.md)
- [finger](finger.md)
- [flattemp](flattemp.md)
- [fondue](fondue.md)
- [for](for.md)
- [forfiles](forfiles.md)
- [format](format.md)
- [freedisk](freedisk.md)
- [fsutil](fsutil.md)
  - [fsutil 8dot3name](fsutil-8dot3name.md)
  - [fsutil behavior](fsutil-behavior.md)
  - [fsutil dirty](fsutil-dirty.md)
  - [fsutil file](fsutil-file.md)
  - [fsutil fsinfo](fsutil-fsinfo.md)
  - [fsutil hardlink](fsutil-hardlink.md)
  - [fsutil objectid](fsutil-objectid.md)
  - [fsutil quota](fsutil-quota.md)
  - [fsutil repair](fsutil-repair.md)
  - [fsutil reparsepoint](fsutil-reparsepoint.md)
  - [fsutil resource](fsutil-resource.md)
  - [fsutil sparse](fsutil-sparse.md)
  - [fsutil tiering](fsutil-tiering.md)
  - [fsutil transaction](fsutil-transaction.md)
  - [fsutil usn](fsutil-usn.md)
  - [fsutil volume](fsutil-volume.md)
  - [fsutil wim](fsutil-wim.md)
- [ftp](ftp.md)
  - [ftp append](ftp-append.md)
  - [ftp ascii](ftp-ascii.md)
  - [ftp bell](ftp-bell_1.md)
  - [ftp binary](ftp-binary.md)
  - [ftp bye](ftp-bye.md)
  - [ftp cd](ftp-cd.md)
  - [ftp close](ftp-close_1.md)
  - [ftp debug](ftp-debug.md)
  - [ftp delete](ftp-delete.md)
  - [ftp dir](ftp-dir.md)
  - [ftp disconnect](ftp-disconnect_1.md)
  - [ftp get](ftp-get.md)
  - [ftp glob](ftp-glob_1.md)
  - [ftp hash](ftp-hash_1.md)
  - [ftp lcd](ftp-lcd.md)
  - [ftp literal](ftp-literal_1.md)
  - [ftp ls](ftp-ls_1.md)
  - [ftp mget](ftp-mget.md)
  - [ftp mkdir](ftp-mkdir.md)
  - [ftp mls](ftp-mls_1.md)
  - [ftp mput](ftp-mput_1.md)
  - [ftp open](ftp-open_1.md)
  - [ftp prompt](ftp-prompt_1.md)
  - [ftp put](ftp-put.md)
  - [ftp pwd](ftp-pwd_1.md)
  - [ftp quit](ftp-quit.md)
  - [ftp quote](ftp-quote.md)
  - [ftp recv](ftp-recv.md)
  - [ftp remotehelp](ftp-remotehelp_1.md)
  - [ftp rename](ftp-rename.md)
  - [ftp rmdir](ftp-rmdir.md)
  - [ftp send](ftp-send_1.md)
  - [ftp status](ftp-status.md)
  - [ftp trace](ftp-trace_1.md)
  - [ftp type](ftp-type.md)
  - [ftp user](ftp-user.md)
  - [ftp verbose](ftp-verbose_1.md)
  - [ftp mdelete](ftp-.mdelete_1.md)
  - [ftp mdir](ftp.mdir.md)
- [ftype](ftype.md)
- [fveupdate](fveupdate.md)

### <a name="g"></a>G

- [getmac](getmac.md)
- [gettype](gettype.md)
- [goto](goto.md)
- [gpfixup](gpfixup.md)
- [gpresult](gpresult.md)
- [gpt](gpt.md)
- [gpupdate](gpupdate.md)
- [graftabl](graftabl.md)

### <a name="h"></a>H

- [help](help.md)
- [helpctr](helpctr.md)
- [hostname](hostname.md)

### <a name="i"></a>I

- [icacls](icacls.md)
- [if](if.md)
- [importar (Shadowdisk)](import.md)
- [importar (DiskPart)](import_1.md)
- [inactive](inactive.md)
- [inuse](inuse.md)
- [ipconfig](ipconfig.md)
- [ipxroute](ipxroute.md)
- [irftp](irftp.md)

### <a name="j"></a>J

- [jetpack](jetpack.md)

### <a name="k"></a>K

- [klist](klist.md)
- [ksetup](ksetup.md)
  - [ksetup addenctypeattr](ksetup-addenctypeattr.md)
  - [ksetup addhosttorealmmap](ksetup-addhosttorealmmap.md)
  - [ksetup addkdc](ksetup-addkdc.md)
  - [ksetup addkpasswd](ksetup-addkpasswd.md)
  - [ksetup addrealmflags](ksetup-addrealmflags.md)
  - [ksetup changepassword](ksetup-changepassword.md)
  - [ksetup delenctypeattr](ksetup-delenctypeattr.md)
  - [ksetup delhosttorealmmap](ksetup-delhosttorealmmap.md)
  - [ksetup delkdc](ksetup-delkdc.md)
  - [ksetup delkpasswd](ksetup-delkpasswd.md)
  - [ksetup delrealmflags](ksetup-delrealmflags.md)
  - [ksetup domain](ksetup-domain.md)
  - [ksetup dumpstate](ksetup-dumpstate.md)
  - [ksetup getenctypeattr](ksetup-getenctypeattr.md)
  - [ksetup listrealmflags](ksetup-listrealmflags.md)
  - [ksetup mapuser](ksetup-mapuser.md)
  - [ksetup removerealm](ksetup-removerealm.md)
  - [ksetup server](ksetup-server.md)
  - [ksetup setcomputerpassword](ksetup-setcomputerpassword.md)
  - [ksetup setenctypeattr](ksetup-setenctypeattr.md)
  - [ksetup setrealm](ksetup-setrealm.md)
  - [ksetup setrealmflags](ksetup-setrealmflags.md)
- [ktmutil](ktmutil.md)
- [ktpass](ktpass.md)

### <a name="l"></a>L

- [label](label.md)
- [list](list.md)
  - [listar provedores](list-providers.md)
  - [listar sombras](list-shadows.md)
  - [listar gravadores](list-writers.md)
- [carregar metadados](load-metadata.md)
- [lodctr](lodctr.md)
- [logman](logman.md)
  - [logman create](logman-create.md)
  - [criar alerta de logman](logman-create-alert.md)
  - [criar API do logman](logman-create-api.md)
  - [logman Create cfg](logman-create-cfg.md)
  - [criar contador de logman](logman-create-counter.md)
  - [criar rastreamento de logman](logman-create-trace.md)
  - [logman delete](logman-delete.md)
  - [importação de Logman e exportação de logman](logman-import-export.md)
  - [logman query](logman-query.md)
  - [inicialização do Logman e parada do logman](logman-start-stop.md)
  - [logman update](logman-update.md)
  - [alerta de atualização de logman](logman-update-alert.md)
  - [API de atualização do logman](logman-update-api.md)
  - [cfg de atualização de logman](logman-update-cfg.md)
  - [contador de atualização do logman](logman-update-counter.md)
  - [rastreamento de atualização do logman](logman-update-trace.md)
- [logoff](logoff.md)
- [lpq](lpq.md)
- [lpr](lpr.md)

### <a name="m"></a>M

- [macfile](macfile.md)
- [makecab](makecab.md)
- [gerenciar bde](manage-bde.md)
  - [gerenciar status do bde](manage-bde-status.md)
  - [gerenciar o BDE em](manage-bde-on.md)
  - [gerenciar bde desativado](manage-bde-off.md)
  - [gerenciar o BDE pausar](manage-bde-pause.md)
  - [gerenciar o reinício do bde](manage-bde-resume.md)
  - [gerenciar o bloqueio do bde](manage-bde-lock.md)
  - [gerenciar o desbloqueio do bde](manage-bde-unlock.md)
  - [gerenciar o desbloqueio automático do bde](manage-bde-autounlock.md)
  - [gerenciar protetores do bde](manage-bde-protectors.md)
  - [gerenciar TPM do bde](manage-bde-tpm.md)
  - [gerenciar o BDE setidentifier](manage-bde-setidentifier.md)
  - [gerenciar forcerecovery do bde](manage-bde-forcerecovery.md)
  - [gerenciar ChangePassword do bde](manage-bde-changepassword.md)
  - [gerenciar changepin do bde](manage-bde-changepin.md)
  - [gerenciar ChangeKey do bde](manage-bde-changekey.md)
  - [gerenciar pacote keybde](manage-bde-keypackage.md)
  - [gerenciar atualização do bde](manage-bde-upgrade.md)
  - [gerenciar wipefreespace do bde](manage-bde-wipefreespace.md)
- [mapadmin](mapadmin.md)
- [MD](md.md)
- [Mesclar vdisk](merge-vdisk.md)
- [mkdir](mkdir.md)
- [mklink](mklink.md)
- [mmc](mmc.md)
- [mode](mode.md)
- [more](more.md)
- [mount](mount.md)
- [mountvol](mountvol.md)
- [move](move.md)
- [mqbkup](mqbkup.md)
- [mqsvc](mqsvc.md)
- [mqtgsvc](mqtgsvc.md)
- [msdt](msdt.md)
- [msg](msg.md)
- [msiexec](msiexec.md)
- [msinfo32](msinfo32.md)
- [mstsc](mstsc.md)

### <a name="n"></a>N

- [nbtstat](nbtstat.md)
- [netcfg](netcfg.md)
- [NET Print](net-print.md)
- [netsh](netsh.md)
- [netstat](netstat.md)
- [nfsadmin](nfsadmin.md)
- [nfsshare](nfsshare.md)
- [nfsstat](nfsstat.md)
- [nlbmgr](nlbmgr.md)
- [nslookup](nslookup.md)
  - [nslookup exit Command](nslookup-exit-command.md)
  - [nslookup finger Command](nslookup-finger-command.md)
  - [nslookup help](nslookup-help.md)
  - [nslookup ls](nslookup-ls.md)
  - [nslookup lserver](nslookup-lserver.md)
  - [nslookup root](nslookup-root.md)
  - [nslookup server](nslookup-server.md)
  - [nslookup set](nslookup-set.md)
  - [nslookup set all](nslookup-set-all.md)
  - [nslookup set class](nslookup-set-class.md)
  - [nslookup set d2](nslookup-set-d2.md)
  - [nslookup set debug](nslookup-set-debug.md)
  - [nslookup set domain](nslookup-set-domain.md)
  - [nslookup set port](nslookup-set-port.md)
  - [nslookup set querytype](nslookup-set-querytype.md)
  - [nslookup set recurse](nslookup-set-recurse.md)
  - [nslookup set retry](nslookup-set-retry.md)
  - [nslookup set root](nslookup-set-root.md)
  - [nslookup set search](nslookup-set-search.md)
  - [nslookup set srchlist](nslookup-set-srchlist.md)
  - [nslookup set timeout](nslookup-set-timeout.md)
  - [nslookup set type](nslookup-set-type.md)
  - [nslookup set vc](nslookup-set-vc.md)
  - [nslookup view](nslookup-view.md)
- [ntbackup](ntbackup.md)
- [ntcmdprompt](ntcmdprompt.md)
- [ntfrsutl](ntfrsutl.md)

### <a name="o"></a>O

- [está](offline.md)
  - [disco offline](offline-disk.md)
  - [volume offline](offline-volume.md)
- [conectar](online.md)
  - [disco online](online-disk.md)
  - [volume online](online-volume.md)
- [openfiles](openfiles.md)

### <a name="p"></a>P

- [pagefileconfig](pagefileconfig.md)
- [path](path.md)
- [pathping](pathping.md)
- [pause](pause.md)
- [pbadmin](pbadmin.md)
- [pentnt](pentnt.md)
- [perfmon](perfmon.md)
- [ping](ping.md)
- [pnpunattend](pnpunattend.md)
- [pnputil](pnputil.md)
- [popd](popd.md)
- [PowerShell](powershell.md)
- [ISE do PowerShell](powershell_ise.md)
- [print](print.md)
- [prncnfg](prncnfg.md)
- [prndrvr](prndrvr.md)
- [prnjobs](prnjobs.md)
- [prnmngr](prnmngr.md)
- [prnport](prnport.md)
- [prnqctl](prnqctl.md)
- [prompt](prompt.md)
- [pubprn](pubprn.md)
- [pushd](pushd.md)
- [pushprinterconnections](pushprinterconnections.md)
- [pwlauncher](pwlauncher.md)

### <a name="q"></a>Q

- [qappsrv](qappsrv.md)
- [qprocess](qprocess.md)
- [consulta](query.md)
  - [processo de consulta](query-process.md)
  - [sessão de consulta](query-session.md)
  - [termserver de consulta](query-termserver.md)
  - [consultar usuário](query-user.md)
- [quser](quser.md)
- [qwinsta](qwinsta.md)

### <a name="r"></a>R

- [rcp](rcp.md)
- [rd](rd.md)
- [rdpsign](rdpsign.md)
- [recover](recover.md)
- [recuperar grupo de discos](recover_1.md)
- [reg](reg.md)
  - [Adicionar Reg](reg-add.md)
  - [Reg Compare](reg-compare.md)
  - [cópia reg](reg-copy.md)
  - [REG DELETE](reg-delete.md)
  - [exportar reg](reg-export.md)
  - [importação de reg](reg-import.md)
  - [carga reg](reg-load.md)
  - [consulta reg](reg-query.md)
  - [Reg Restore](reg-restore.md)
  - [Reg salvar](reg-save.md)
  - [Reg Unload](reg-unload.md)
- [regini](regini.md)
- [regsvr32](regsvr32.md)
- [relog](relog.md)
- [arquivo em lotes REM](rem.md)
- [script de REM](rem_1.md)
- [remove](remove.md)
- [ren](ren.md)
- [rename](rename.md)
- [reparo](repair.md)
  - [reparar o BDE](repair-bde.md)
- [substitui](replace.md)
- [examinar novamente](rescan.md)
- [reset](reset.md)
  - [reset session](reset-session.md)
- [manteve](retain.md)
- [voltar](revert.md)
- [rexec](rexec.md)
- [risetup](risetup.md)
- [rmdir](rmdir.md)
- [robocopy](robocopy.md)
- [WS2008 de rota](route_ws2008.md)
- [rpcinfo](rpcinfo.md)
- [rpcping](rpcping.md)
- [rsh](rsh.md)
- [rundll32](rundll32.md)
- [rundll32 printui](rundll32-printui.md)
- [rwinsta](rwinsta.md)

### <a name="s"></a>S

- [solução](san.md)
- [configuração do SC](sc-config.md)
- [criar SC](sc-create.md)
- [excluir SC](sc-delete.md)
- [consulta SC](sc-query.md)
- [schtasks](schtasks.md)
- [scwcmd](scwcmd.md)
  - [scwcmd analyze](scwcmd-analyze.md)
  - [configuração de scwcmd](scwcmd-configure.md)
  - [scwcmd register](scwcmd-register.md)
  - [scwcmd rollback](scwcmd-rollback.md)
  - [scwcmd transform](scwcmd-transform.md)
  - [exibição de scwcmd](scwcmd-view.md)
- [secedit](secedit.md)
  - [análise de secedit](secedit-analyze.md)
  - [configurar secedit](secedit-configure.md)
  - [exportação de secedit](secedit-export.md)
  - [generaterollback secedit](secedit-generaterollback.md)
  - [importação de secedit](secedit-import.md)
  - [validação de secedit](secedit-validate.md)
- [select](select.md)
  - [select disk](select-disk.md)
  - [selecionar partição](select-partition.md)
  - [selecionar vdisk](select-vdisk.md)
  - [select volume](select-volume.md)
- [serverceipoptin](serverceipoptin.md)
- [ServerManagerCmd](servermanagercmd.md)
- [serverweroptin](serverweroptin.md)
- [definir variáveis de ambiente](set_1.md)
- [definir cópia de sombra](set_2.md)
  - [Definir contexto](set-context.md)
  - [ID do conjunto](set-id.md)
  - [setlocal](setlocal.md)
  - [definir metadados](set-metadata.md)
  - [opção set](set-option.md)
  - [Definir modo detalhado](set-verbose.md)
- [setx](setx.md)
- [sfc](sfc.md)
- [shadow](shadow.md)
- [shift](shift.md)
- [showmount](showmount.md)
- [shrink](shrink.md)
- [shutdown](shutdown.md)
- [simular restauração](simulate-restore.md)
- [sort](sort.md)
- [start](start.md)
- [dispositivo de conjunto de subcomandos](subcommand-set-device.md)
- [grupo de drivers de conjunto de subcomandos](subcommand-set-drivergroup.md)
- [conjunto de subcomandos drivergroupfilter](subcommand-set-drivergroupfilter.md)
- [conjunto de subcomandos DriverPackage](subcommand-set-driverpackage.md)
- [imagem do conjunto de subcomandos](subcommand-set-image.md)
- [conjunto de imagens de definição de subcomando](subcommand-set-imagegroup.md)
- [servidor de conjunto de subcomandos](subcommand-set-server.md)
- [conjunto de subcomandos TransportServer](subcommand-set-transportserver.md)
- [conjunto de subcomandos MulticastTransmission](subcommand-start-multicasttransmission.md)
- [namespace de início de subcomando](subcommand-start-namespace.md)
- [servidor de inicialização de subcomando](subcommand-start-server.md)
- [TransportServer de início de subcomando](subcommand-start-transportserver.md)
- [servidor de parada de subcomando](subcommand-stop-server.md)
- [TransportServer de parada de subcomando](subcommand-stop-transportserver.md)
- [subst](subst.md)
- [sxstrace](sxstrace.md)
- [sysocmgr](sysocmgr.md)
- [systeminfo](systeminfo.md)


### <a name="t"></a>T

- [takeown](takeown.md)
- [tapicfg](tapicfg.md)
- [taskkill](taskkill.md)
- [tasklist](tasklist.md)
- [tcmsetup](tcmsetup.md)
- [telnet](telnet.md)
  - [fechar Telnet](telnet-close.md)
  - [exibição de Telnet](telnet-display.md)
  - [Telnet aberto](telnet-open.md)
  - [sair do telnet](telnet-quit.md)
  - [envio de Telnet](telnet-send.md)
  - [conjunto de Telnet](telnet-set.md)
  - [status do telnet](telnet-status.md)
  - [desdefinição de Telnet](telnet-unset.md)
- [tftp](tftp.md)
- [time](time.md)
- [timeout](timeout_1.md)
- [title](title_1.md)
- [tlntadmn](tlntadmn.md)
- [tpmtool](tpmtool.md)
- [tpmvscmgr](tpmvscmgr.md)
- [tracerpt](tracerpt_1.md)
- [tracert](tracert.md)
- [tree](tree.md)
- [tscon](tscon.md)
- [tsdiscon](tsdiscon.md)
- [tsecimp](tsecimp_1.md)
- [tskill](tskill.md)
- [tsprof](tsprof.md)
- [type](type.md)
- [typeperf](typeperf.md)
- [tzutil](tzutil.md)

### <a name="u"></a>U

- [Cancele](unexpose.md)
- [UniqueId](uniqueid.md)
- [unlodctr](unlodctr_1.md)

### <a name="v"></a>V

- [ver](ver.md)
- [verifier](verifier.md)
- [verify](verify_1.md)
- [vol](vol.md)
- [vssadmin](vssadmin.md)
  - [vssadmin delete shadows](vssadmin-delete-shadows.md)
  - [vssadmin list shadows](vssadmin-list-shadows.md)
  - [vssadmin list writers](vssadmin-list-writers.md)
  - [vssadmin resize shadowstorage](vssadmin-resize-shadowstorage.md)

### <a name="w"></a>W

- [waitfor](waitfor.md)
- [wbadmin](wbadmin.md)
  - [Wbadmin excluir catálogo](wbadmin-delete-catalog.md)
  - [Wbadmin Delete systemstatebackup](wbadmin-delete-systemstatebackup.md)
  - [Wbadmin Disable backup](wbadmin-disable-backup.md)
  - [Wbadmin habilitar backup](wbadmin-enable-backup.md)
  - [Wbadmin obter discos](wbadmin-get-disks.md)
  - [Wbadmin obter itens](wbadmin-get-items.md)
  - [Wbadmin obter status](wbadmin-get-status.md)
  - [Wbadmin obter versões](wbadmin-get-versions.md)
  - [Wbadmin restore catalog](wbadmin-restore-catalog.md)
  - [Wbadmin iniciar backup](wbadmin-start-backup.md)
  - [Wbadmin iniciar recuperação](wbadmin-start-recovery.md)
  - [Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)
  - [Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)
  - [Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)
  - [Wbadmin parar trabalho](wbadmin-stop-job.md)
- [wdsutil](wdsutil.md)
- [wecutil](wecutil.md)
- [wevtutil](wevtutil.md)
- [where](where_1.md)
- [whoami](whoami.md)
- [winnt](winnt.md)
- [winnt32](winnt32.md)
- [winpop](winpop.md)
- [winrs](winrs.md)
- [WinSAT mem](winsat-mem.md)
- [mfmedia do WinSAT](winsat-mfmedia.md)
- [wmic](wmic.md)
- [escritor](writer.md)
- [wscript](wscript.md)

### <a name="x"></a>X

- [xcopy](xcopy.md)
