---
title: Comandos do Windows
description: Comandos do Windows
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c703d07c-8227-4e86-94a6-8ef390f94cdc
author: jasongerend
ms.author: jgerend
manager: dongill
ms.date: 06/26/2019
ms.prod: windows-server
ms.openlocfilehash: 5cb26bcff99d9cf3a1ee8b3a937ad6098a913c3d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362057"
---
# <a name="windows-commands"></a>Comandos do Windows

Todas as versões com suporte do Windows (servidor e cliente) têm um conjunto de comandos do console do Win32 internos.

Este conjunto de documentação descreve os comandos do Windows que você pode usar para automatizar tarefas usando scripts ou ferramentas de script.

Para localizar informações sobre um comando específico, no menu a-Z a seguir, clique na letra com a qual o comando começa e, em seguida, clique no nome do comando.

[A](#a) |
[B](#b) | 
[C](#c) | 
[D](#d) | 
[E](#e) | 
[F](#f)1[G](#g)3[H](#h)5[I](#i)7[J](#j)9[K](#k)1[L ](#l)3[M](#m)5[N](#n)7[O](#o)9[P](#p)1[Q](#q)3[R](#r)5[S](#s)7[T](#t)9[U](#u)1[V](#v)3 [W](#w)5[X](#x) | Y | Z

## <a name="prerequisites"></a>Pré-requisitos

As informações contidas neste tópico aplicam-se a:

-   Windows Server 2019
-   Windows Server (canal semestral)
-   Windows Server 2016
-   Windows Server 2012 R2
-   Windows Server 2012 
-   Windows Server 2008 R2
-   Windows Server 2008
-   Windows 10
-   Windows 8.1

### <a name="command-shell-overview"></a>Visão geral do Shell de comando

O Shell de comando foi o primeiro Shell interno do Windows para automatizar tarefas rotineiras, como o gerenciamento de contas de usuário ou backups noturnos, com arquivos de lote (. bat). Com o Windows Script Host, você poderia executar scripts mais sofisticados no Shell de comando. Para obter mais informações, consulte [cscript](cscript.md) ou [WScript](wscript.md). Você pode executar operações com mais eficiência usando scripts do que é possível usando a interface do usuário. Os scripts aceitam todos os comandos que estão disponíveis na linha de comando.

O Windows tem dois shells de comando: O Shell de comando e o [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6). Cada shell é um programa de software que fornece comunicação direta entre você e o sistema operacional ou aplicativo, fornecendo um ambiente para automatizar operações de ti.

O PowerShell foi projetado para estender os recursos do Shell de comando para executar comandos do PowerShell chamados cmdlets. Os cmdlets são semelhantes aos comandos do Windows, mas fornecem uma linguagem de script mais extensível. Você pode executar comandos do Windows e cmdlets do PowerShell no PowerShell, mas o Shell de comando só pode executar comandos do Windows e não cmdlets do PowerShell.

Para a automação do Windows mais robusta e atualizada, recomendamos o uso do PowerShell em vez de comandos do Windows ou do Windows Script Host para automação do Windows. 
> [!NOTE]
>Você também pode baixar e instalar o [PowerShell Core](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6), a versão de código aberto do PowerShell. 

> [!CAUTION]
> A edição incorreta do Registro pode causar danos graves ao sistema. Antes de fazer as alterações a seguir no registro, você deve fazer backup de todos os dados importantes no computador.

> [!NOTE]
> Para habilitar ou desabilitar a conclusão de nome de arquivo e diretório no Shell de comando em um computador ou sessão de logon de usuário, execute **regedit. exe** e defina o seguinte **valor de reg_DWOrd**:
> 
> HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\completionChar\reg_DWOrd
> 
> Para definir o valor **reg_DWOrd** , use o valor hexadecimal de um caractere de controle para uma função específica (por exemplo, **0 9** é Tab e **0 08** é Backspace). As configurações especificadas pelo usuário têm precedência sobre as configurações do computador, e as opções de linha de comando têm precedência sobre as configurações do registro.

## <a name="command-line-reference-a-z"></a>Referência de linha de comando A-Z

Para encontrar informações sobre um comando específico do Windows, no menu a-Z a seguir, clique na letra com a qual o comando começa e, em seguida, clique no nome do comando.

[A](#a) |
[B](#b) | 
[C](#c) | 
[D](#d) | 
[E](#e) | 
[F](#f)1[G](#g)3[H](#h)5[I](#i)7[J](#j)9[K](#k)1[L ](#l)3[M](#m)5[N](#n)7[O](#o)9[P](#p)1[Q](#q)3[R](#r)5[S](#s)7[T](#t)9[U](#u)1[V](#v)3 [W](#w)5[X](#x) | Y | Z

### <a name="a"></a>A
-   [append](append.md)
-   [arp](arp.md)
-   [assoc](assoc.md)
-   [at](at.md)
-   [atmadm](atmadm.md)
-   [attrib](attrib.md)
-   [auditpol](auditpol.md)
-   [autochk](autochk.md)
-   [autoconv](autoconv.md)
-   [autofmt](autofmt.md)

### <a name="b"></a>B
- [bcdboot](bcdboot.md)
- [bcdedit](bcdedit.md)
- [bdehdcfg](bdehdcfg.md)
- [bitsadmin](bitsadmin.md)
  -   [bitsadmin addfile](bitsadmin-addfile.md)
  -   [bitsadmin addfileset](bitsadmin-addfileset.md)
  -   [bitsadmin addfilewithranges](bitsadmin-addfilewithranges.md)
  -   [bitsadmin cancel](bitsadmin-cancel.md)
  -   [bitsadmin complete](bitsadmin-complete.md)
  -   [bitsadmin create](bitsadmin-create.md)
  -   [bitsadmin getaclflags](bitsadmin-getaclflags.md)
  -   [bitsadmin getbytestotal](bitsadmin-getbytestotal.md)
  -   [bitsadmin getbytestransferred](bitsadmin-getbytestransferred.md)
  -   [bitsadmin getcompletiontime](bitsadmin-getcompletiontime.md)
  -   [bitsadmin getcreationtime](bitsadmin-getcreationtime.md)
  -   [bitsadmin getdescription](bitsadmin-getdescription.md)
  -   [bitsadmin getdisplayname](bitsadmin-getdisplayname.md)
  -   [bitsadmin geterror](bitsadmin-geterror.md)
  -   [bitsadmin geterrorcount](bitsadmin-geterrorcount.md)
  -   [bitsadmin getfilestotal](bitsadmin-getfilestotal.md)
  -   [bitsadmin getfilestransferred](bitsadmin-getfilestransferred.md)
  -   [bitsadmin getminretrydelay](bitsadmin-getminretrydelay.md)
  -   [bitsadmin getmodificationtime](bitsadmin-getmodificationtime.md)
  -   [bitsadmin getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)
  -   [bitsadmin getnotifycmdline](bitsadmin-getnotifycmdline.md)
  -   [bitsadmin getnotifyflags](bitsadmin-getnotifyflags.md)
  -   [bitsadmin getnotifyinterface](bitsadmin-getnotifyinterface.md)
  -   [bitsadmin getowner](bitsadmin-getowner.md)
  -   [Bitsadmin obter prioridade](bitsadmin-getpriority.md)
  -   [bitsadmin getproxybypasslist](bitsadmin-getproxybypasslist.md)
  -   [bitsadmin getproxylist](bitsadmin-getproxylist.md)
  -   [bitsadmin getproxyusage](bitsadmin-getproxyusage.md)
  -   [bitsadmin getreplydata](bitsadmin-getreplydata.md)
  -   [bitsadmin getreplyfilename](bitsadmin-getreplyfilename.md)
  -   [bitsadmin getreplyprogress](bitsadmin-getreplyprogress.md)
  -   [bitsadmin getstate](bitsadmin-getstate.md)
  -   [bitsadmin gettype](bitsadmin-gettype.md)
  -   [bitsadmin help](bitsadmin-help.md)
  -   [bitsadmin info](bitsadmin-info.md)
  -   [bitsadmin list](bitsadmin-list.md)
  -   [bitsadmin listfiles](bitsadmin-listfiles.md)
  -   [bitsadmin monitor](bitsadmin-monitor.md)
  -   [bitsadmin nowrap](bitsadmin-nowrap.md)
  -   [bitsadmin rawreturn](bitsadmin-rawreturn.md)
  -   [bitsadmin removecredentials](bitsadmin-removecredentials.md)
  -   [bitsadmin replaceremoteprefix](bitsadmin-replaceremoteprefix.md)
  -   [bitsadmin reset](bitsadmin-reset.md)
  -   [bitsadmin resume](bitsadmin-resume.md)
  -   [bitsadmin setaclflag](bitsadmin-setaclflag.md)
  -   [bitsadmin setcredentials](bitsadmin-setcredentials.md)
  -   [bitsadmin setdescription](bitsadmin-setdescription.md)
  -   [bitsadmin setdisplayname](bitsadmin-setdisplayname.md)
  -   [bitsadmin setminretrydelay](bitsadmin-setminretrydelay.md)
  -   [bitsadmin setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)
  -   [bitsadmin setnotifycmdline](bitsadmin-setnotifycmdline.md)
  -   [bitsadmin setnotifyflags](bitsadmin-setnotifyflags.md)
  -   [bitsadmin setpriority](bitsadmin-setpriority.md)
  -   [bitsadmin setproxysettings](bitsadmin-setproxysettings.md)
  -   [bitsadmin setreplyfilename](bitsadmin-setreplyfilename.md)
  -   [bitsadmin suspend](bitsadmin-suspend.md)
  -   [bitsadmin takeownership](bitsadmin-takeownership.md)
  -   [Transferência Bitsadmin](bitsadmin-transfer.md)
  -   [bitsadmin util](bitsadmin-util.md)
  -   [bitsadmin wrap](bitsadmin-wrap.md)
- [bootcfg](bootcfg.md)
  -   [bootcfg addsw](bootcfg-addsw.md)
  -   [bootcfg copy](bootcfg-copy.md)
  -   [bootcfg dbg1394](bootcfg-dbg1394.md)
  -   [bootcfg debug](bootcfg-debug.md)  
  -   [bootcfg default](bootcfg-default.md)
  -   [bootcfg delete](bootcfg-delete.md)
  -   [bootcfg ems](bootcfg-ems.md)
  -   [bootcfg query](bootcfg-query.md)
  -   [bootcfg raw](bootcfg-raw.md)
  -   [bootcfg rmsw](bootcfg-rmsw.md)
  -   [bootcfg timeout](bootcfg-timeout.md)
- [break](break_1.md)

### <a name="c"></a>C
- [cacls](cacls_1.md)
- [call](call.md)
- [cd](cd.md)
- [certreq](certreq_1.md)
- [certutil](certutil.md)
- [change](change.md)
  -   [change logon](change-logon.md)
  -   [change port](change-port.md)
  -   [change user](change-user.md)
- [chcp](chcp.md)
- [chdir](chdir_1.md)
- [chglogon](chglogon.md)
- [chgport](chgport.md)
- [chgusr](chgusr.md)
- [chkdsk](chkdsk.md)
- [chkntfs](chkntfs.md)
- [choice](choice.md)
- [cipher](cipher.md)
- [cleanmgr](cleanmgr.md)
- [clip](clip.md)
- [cls](cls.md)
- [Cmd](Cmd.md)
- [cmdkey](cmdkey.md)
- [cmstp](cmstp.md)
- [color](color.md)
- [comp](comp.md)
- [compact](compact.md)
- [convert](convert.md)
- [copy](copy.md)
- [cprofile](cprofile.md)
- [cscript](cscript.md)

### <a name="d"></a>D
-   [date](date.md)
-   [dcgpofix](dcgpofix.md)
-   [defrag](defrag.md)
-   [del](del.md)
-   [dfsrmig](dfsrmig.md)
-   [diantz](diantz.md)
-   [dir](dir.md)
-   [diskcomp](diskcomp.md)
-   [diskcopy](diskcopy.md)
-   [diskpart](diskpart.md)
-   [diskperf](diskperf.md)
-   [diskraid](diskraid.md)
-   [diskshadow](diskshadow.md)
-   [dispdiag](dispdiag.md)
-   [dnscmd](Dnscmd.md)
-   [doskey](doskey.md)
-   [driverquery](driverquery.md)

### <a name="e"></a>E
-   [echo](echo.md)
-   [edit](edit.md)
-   [endlocal](endlocal.md)
-   [erase](erase.md)
-   [eventcreate](eventcreate.md)
-   [eventquery](eventquery.md)
-   [eventtriggers](eventtriggers.md)
-   [evntcmd](Evntcmd.md)
-   [exit](exit_2.md)
-   [expand](expand.md)
-   [extract](extract.md)

### <a name="f"></a>F
- [fc](fc.md)
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
  -   [fsutil 8dot3name](fsutil-8dot3name.md) 
  -   [fsutil behavior](fsutil-behavior.md) 
  -   [fsutil file](fsutil-file.md)
  -   [fsutil fsinfo](fsutil-fsinfo.md)
  -   [fsutil hardlink](fsutil-hardlink.md)
  -   [fsutil objectid](fsutil-objectid.md)
  -   [fsutil quota](fsutil-quota.md)
  -   [fsutil repair](fsutil-repair.md)
  -   [fsutil reparsepoint](fsutil-reparsepoint.md)
  -   [fsutil resource](fsutil-resource.md)
  -   [fsutil sparse](fsutil-sparse.md)
  -   [fsutil tiering](fsutil-tiering.md)
  -   [fsutil transaction](fsutil-transaction.md)
  -   [fsutil usn](fsutil-usn.md)
  -   [fsutil volume](fsutil-volume.md)
  -   [fsutil wim](fsutil-wim.md)
- [FTP](ftp.md)
- [ftype](ftype.md)
- [fveupdate](fveupdate.md)

### <a name="g"></a>G
-   [getmac](getmac.md)
-   [gettype](gettype.md)
-   [goto](goto.md)
-   [gpfixup](gpfixup.md)
-   [gpresult](gpresult.md)
-   [gpupdate](gpupdate.md)
-   [graftabl](graftabl.md)

### <a name="h"></a>H
-   [help](help.md)
-   [helpctr](helpctr.md)
-   [hostname](hostname.md)

### <a name="i"></a>ENCONTREI
-   [icacls](icacls.md)
-   [if](if.md)
-   [inuse](inuse.md)
-   [ipconfig](ipconfig.md)
-   [ipxroute](ipxroute.md)
-   [irftp](irftp.md)

### <a name="j"></a>J
-   [jetpack](jetpack.md)

### <a name="k"></a>K
- [klist](klist.md)
- [ksetup](ksetup.md)
  -   [ksetup: setreale](ksetup-setrealm.md)
  -   [ksetup: mapuser](ksetup-mapuser.md)
  -   [ksetup: addkdc](ksetup-addkdc.md)
  -   [ksetup: delkdc](ksetup-delkdc.md)
  -   [ksetup: addkpasswd](ksetup-addkpasswd.md)
  -   [ksetup: delkpasswd](ksetup-delkpasswd.md)
  -   [ksetup: servidor](ksetup-server.md)
  -   [ksetup: setcomputerpassword](ksetup-setcomputerpassword.md)
  -   [ksetup: removerealm](ksetup-removerealm.md)
  -   [ksetup: domínio](ksetup-domain.md)
  -   [ksetup: ChangePassword](ksetup-changepassword.md)
  -   [ksetup: listrealmflags](ksetup-listrealmflags.md)
  -   [ksetup: setrealmflags](ksetup-setrealmflags.md)
  -   [ksetup: addrealmflags](ksetup-addrealmflags.md)
  -   [ksetup: delrealmflags](ksetup-delrealmflags.md)
  -   [ksetup: dumpstate](ksetup-dumpstate.md)
  -   [ksetup: addhosttorealmmap](ksetup-addhosttorealmmap.md)
  -   [ksetup: delhosttorealmmap](ksetup-delhosttorealmmap.md)
  -   [ksetup: setenctypeattr](ksetup-setenctypeattr.md)
  -   [ksetup: getenctypeattr](ksetup-getenctypeattr.md)
  -   [ksetup: addenctypeattr](ksetup-addenctypeattr.md)
  -   [ksetup: delenctypeattr](ksetup-delenctypeattr.md) 
- [ktmutil](ktmutil.md)
- [ktpass](ktpass.md)

### <a name="l"></a>L
- [label](label.md)
- [lodctr](lodctr.md)
- [logman](logman.md)
  -   [logman create](logman-create.md)
  -   [logman query](logman-query.md)
  -   [logman iniciar & 124; deixar](logman-start-stop.md)
  -   [logman delete](logman-delete.md)
  -   [logman update](logman-update.md)
  -   [importação de logman & 124; Export](logman-import-export.md)
- [logoff](logoff.md)
- [lpq](lpq.md)
- [lpr](lpr.md)

### <a name="m"></a>M
- [macfile](macfile.md)
- [makecab](makecab.md)
- [manage-bde](manage-bde.md)
  -   [Manage-bde: status](manage-bde-status.md)
  -   [Manage-bde: on](manage-bde-on.md)
  -   [Manage-bde: off](manage-bde-off.md)
  -   [Manage-bde: pausar](manage-bde-pause.md)
  -   [Manage-bde: retomar](manage-bde-resume.md)
  -   [Manage-bde: Lock](manage-bde-lock.md)
  -   [Manage-bde: desbloquear](manage-bde-unlock.md)
  -   [Manage-bde: desbloqueio automático](manage-bde-autounlock.md)
  -   [Manage-bde: protetores](manage-bde-protectors.md)
  -   [Manage-bde: TPM](manage-bde-tpm.md)
  -   [Manage-bde: setidentifier](manage-bde-setidentifier.md)
  -   [manage-bde: ForceRecovery](manage-bde-forcerecovery.md)
  -   [Manage-bde: ChangePassword](manage-bde-changepassword.md)
  -   [Manage-bde: changepin](manage-bde-changepin.md)
  -   [Manage-bde: ChangeKey](manage-bde-changekey.md)
  -   [manage-bde: KeyPackage](manage-bde-keypackage.md)
  -   [Manage-bde: atualizar](manage-bde-upgrade.md)
  -   [manage-bde: WipeFreeSpace](manage-bde-wipefreespace.md)
- [mapadmin](mapadmin.md)
- [Md](Md.md)
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
- [netsh](netsh.md)
- [netstat](netstat.md)
- [Net print](net-print.md)
- [nfsadmin](nfsadmin.md)
- [nfsshare](nfsshare.md)
- [nfsstat](nfsstat.md)
- [nlbmgr](nlbmgr.md)
- [nslookup](nslookup.md)
  -   [comando de saída do nslookup](nslookup-exit-command.md)
  -   [comando Finger nslookup](nslookup-finger-command.md)
  -   [nslookup help](nslookup-help.md)
  -   [nslookup ls](nslookup-ls.md)
  -   [nslookup lserver](nslookup-lserver.md)
  -   [nslookup root](nslookup-root.md)
  -   [nslookup server](nslookup-server.md)
  -   [nslookup set](nslookup-set.md)
  -   [nslookup set all](nslookup-set-all.md)
  -   [nslookup set class](nslookup-set-class.md)
  -   [nslookup set d2](nslookup-set-d2.md)
  -   [nslookup set debug](nslookup-set-debug.md)
  -   [nslookup set domain](nslookup-set-domain.md)
  -   [nslookup set port](nslookup-set-port.md)
  -   [nslookup set querytype](nslookup-set-querytype.md)
  -   [nslookup set recurse](nslookup-set-recurse.md)
  -   [nslookup set retry](nslookup-set-retry.md)
  -   [nslookup set root](nslookup-set-root.md)
  -   [nslookup set search](nslookup-set-search.md)
  -   [nslookup set srchlist](nslookup-set-srchlist.md)
  -   [nslookup set timeout](nslookup-set-timeout.md)
  -   [nslookup set type](nslookup-set-type.md)
  -   [nslookup set vc](nslookup-set-vc.md)
  -   [nslookup view](nslookup-view.md)
- [ntbackup](ntbackup.md)
- [ntcmdprompt](ntcmdprompt.md)
- [ntfrsutl](ntfrsutl.md)

### <a name="o"></a>O
-   [openfiles](openfiles.md)

### <a name="p"></a>P
-   [pagefileconfig](pagefileconfig.md)
-   [path](path.md)
-   [pathping](pathping.md)
-   [pause](pause.md)
-   [pbadmin](pbadmin.md)
-   [pentnt](pentnt.md)
-   [perfmon](perfmon.md)
-   [ping](ping.md)
-   [pnpunattend](pnpunattend.md)
-   [pnputil](pnputil.md)
-   [popd](popd.md)
-   [PowerShell](PowerShell.md)
-   [PowerShell_ise](PowerShell_ise.md)
-   [print](print.md)
-   [prncnfg](prncnfg.md)
-   [prndrvr](prndrvr.md)
-   [prnjobs](prnjobs.md)
-   [prnmngr](prnmngr.md)
-   [prnport](prnport.md)
-   [prnqctl](prnqctl.md)
-   [prompt](prompt.md)
-   [pubprn](pubprn.md)
-   [pushd](pushd.md)
-   [pushprinterconnections](pushprinterconnections.md)

### <a name="q"></a>Q
-   [qappsrv](qappsrv.md)
-   [qprocess](qprocess.md)
-   [query](query.md)
-   [quser](quser.md)
-   [qwinsta](qwinsta.md)

### <a name="r"></a>R
- [rcp](rcp.md)
- [rd](rd.md)
- [rdpsign](rdpsign.md)
- [recover](recover.md)
- [reg](reg.md)
  -   [Adicionar Reg](reg-add.md)
  -   [Reg Compare](reg-compare.md)
  -   [cópia reg](reg-copy.md)
  -   [REG DELETE](reg-delete.md)
  -   [exportar reg](reg-export.md)
  -   [importação de reg](reg-import.md)
  -   [carga reg](reg-load.md)
  -   [consulta reg](reg-query.md)
  -   [Reg Restore](reg-restore.md)
  -   [Reg salvar](reg-save.md)
  -   [Reg Unload](reg-unload.md)
- [regini](regini.md)
- [regsvr32](regsvr32.md)
- [relog](relog.md)
- [rem](rem.md)
- [ren](ren.md)
- [rename](rename.md)
- [repair-bde](repair-bde.md)
- [replace](replace.md)
- [reset session](reset-session.md)
- [rexec](rexec.md)
- [risetup](risetup.md)
- [rmdir](rmdir.md)
- [robocopy](robocopy.md)
- [route_ws2008](route_ws2008.md)
- [rpcinfo](rpcinfo.md)
- [rpcping](rpcping.md)
- [rsh](rsh.md)
- [rundll32](rundll32.md)
- [rwinsta](rwinsta.md)

### <a name="s"></a>S
- [schtasks](schtasks.md)
- [scwcmd](Scwcmd.md)
  -   [scwcmd: analisar](scwcmd-analyze.md)
  -   [scwcmd: configurar](scwcmd-configure.md)
  -   [scwcmd: Register](scwcmd-register.md) 
  -   [scwcmd: Rollback](scwcmd-rollback.md) 
  -   [scwcmd: Transform](scwcmd-transform.md) 
  -   [scwcmd: exibir](scwcmd-view.md) 
- [secedit](secedit.md)
  -   [secedit: analisar](secedit-analyze.md)
  -   [secedit: configurar](secedit-configure.md)
  -   [secedit: exportar](secedit-export.md)
  -   [secedit: generaterollback](secedit-generaterollback.md)
  -   [secedit: importar](secedit-import.md)
  -   [secedit: validar](secedit-validate.md)
- [serverceipoptin](serverceipoptin.md)
- [Servermanagercmd](Servermanagercmd.md)
- [serverweroptin](serverweroptin.md)
- [set](set_1.md)
- [setlocal](setlocal.md)
- [setx](setx.md)
- [sfc](sfc.md)
- [shadow](shadow.md)
- [shift](shift.md)
- [showmount](showmount.md)
- [shutdown](shutdown.md)
- [sort](sort.md)
- [start](start.md)
- [subst](subst.md)
- [sxstrace](sxstrace.md)
- [sysocmgr](sysocmgr.md)
- [systeminfo](systeminfo.md)

### <a name="t"></a>T
-   [takeown](takeown.md)
-   [tapicfg](tapicfg.md)
-   [taskkill](taskkill.md)
-   [tasklist](tasklist.md)
-   [tcmsetup](tcmsetup.md)
-   [telnet](telnet.md)
-   [tftp](tftp.md)
-   [time](time.md)
-   [timeout](timeout_1.md)
-   [title](title_1.md)
-   [tlntadmn](tlntadmn.md)
-   [tpmvscmgr](tpmvscmgr.md)
-   [tracerpt](tracerpt_1.md)
-   [tracert](tracert.md)
-   [tree](tree.md)
-   [tscon](tscon.md)
-   [tsdiscon](tsdiscon.md)
-   [tsecimp](tsecimp_1.md)
-   [tskill](tskill.md)
-   [tsprof](tsprof.md)
-   [type](type.md)
-   [typeperf](typeperf.md)
-   [tzutil](tzutil.md)

### <a name="u"></a>T
-   [unlodctr](unlodctr_1.md)

### <a name="v"></a>V
-   [ver](ver.md)
-   [verifier](verifier.md)
-   [verify](verify_1.md)
-   [vol](vol.md)
-   [vssadmin](vssadmin.md)- 

### <a name="w"></a>W
- [waitfor](waitfor.md)
- [wbadmin](wbadmin.md)
  -   [Wbadmin habilitar backup](wbadmin-enable-backup.md)
  -   [Wbadmin Disable backup](wbadmin-disable-backup.md)
  -   [Wbadmin iniciar backup](wbadmin-start-backup.md)
  -   [Wbadmin parar trabalho](wbadmin-stop-job.md)
  -   [Wbadmin obter versões](wbadmin-get-versions.md)
  -   [Wbadmin obter itens](wbadmin-get-items.md)
  -   [Wbadmin iniciar recuperação](wbadmin-start-recovery.md)
  -   [Wbadmin obter status](wbadmin-get-status.md)
  -   [Wbadmin obter discos](wbadmin-get-disks.md)
  -   [Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)
  -   [Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)
  -   [Wbadmin Delete systemstatebackup](wbadmin-delete-systemstatebackup.md)
  -   [Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)
  -   [Wbadmin restore catalog](wbadmin-restore-catalog.md)
  -   [Wbadmin excluir catálogo](wbadmin-delete-catalog.md)
- [wdsutil](wdsutil.md)
- [wecutil](wecutil.md)
- [wevtutil](wevtutil.md)
- [where](where_1.md)
- [whoami](whoami.md)
- [winnt](winnt.md)
- [winnt32](winnt32.md)
- [winpop](winpop.md)
- [winrs](winrs.md)
- [WLBS](wlbs_1.md)
- [wmic](wmic.md)
- [wscript](wscript.md)

### <a name="x"></a>X
-   [xcopy](xcopy.md)
