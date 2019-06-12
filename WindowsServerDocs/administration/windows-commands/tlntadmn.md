---
title: tlntadmn
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78b61e8d-b953-44bb-8d57-f3b42da9e7a8 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b4423e35d0c26819188001dea8d3d8497add7f4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440949"
---
# <a name="tlntadmn"></a>tlntadmn

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Administra um computador local ou remoto que esteja executando o serviço do servidor telnet.   
## <a name="syntax"></a>Sintaxe  
```  
tlntadmn [<computerName>] [-u <UserName>] [-p <Password>] [{start | stop | pause | continue}] [-s {<SessionID> | all}] [-k {<SessionID> | all}] [-m {<SessionID> | all}  <Message>] [config [dom = <Domain>] [ctrlakeymap = {yes | no}] [timeout = <hh>:<mm>:<ss>] [timeoutactive = {yes | no}] [maxfail = <attempts>] [maxconn = <Connections>] [port = <Number>] [sec {+ | -}NTLM {+ | -}passwd] [mode = {console | stream}]] [-?]  
```  
### <a name="parameters"></a>Parâmetros  

|                   Parâmetro                    |                                                                                                                                                       Descrição                                                                                                                                                        |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                \<computerName>                 |                                                                                                                    Especifica o nome do servidor para se conectar ao. O padrão é o computador local.                                                                                                                    |
|         -u \<nome de usuário > -p \<senha >          |                                                Especifica as credenciais administrativas para um servidor remoto que você deseja administrar. Esse parâmetro é necessário se você quiser administrar um servidor remoto para o qual você não efetuou logon com credenciais administrativas.                                                |
|                     start                      |                                                                                                                                            Inicia o serviço do servidor telnet.                                                                                                                                             |
|                      stop                      |                                                                                                                                             Interrompe o serviço do servidor telnet                                                                                                                                              |
|                     pause                      |                                                                                                                          pausa o serviço do servidor telnet. Não há novas conexões serão aceitas.                                                                                                                          |
|                    Continuar                    |                                                                                                                                            Reinicia o serviço do servidor telnet.                                                                                                                                            |
|          -s {\<SessionID > &#124; todos os}          |                                                                                                                                             Exibe sessões de telnet de Active Directory.                                                                                                                                             |
|          -k {\<SessionID > &#124; todos os}          |                                                                                                        Encerra as sessões de telnet. Digite a ID de sessão para encerrar uma sessão específica, ou digite tudo para encerrar todas as sessões.                                                                                                         |
|    -m {\<SessionID > &#124; todos os}  <Message>     |                                                   Envia uma mensagem para uma ou mais sessões. Digite a ID de sessão para enviar uma mensagem a uma sessão específica, ou digite tudo para enviar uma mensagem para todas as sessões. Digite a mensagem que você deseja enviar entre aspas.                                                   |
|             config dom = \<Domain>             |                                                                                                                                      Configura o domínio padrão para o servidor.                                                                                                                                       |
|      config ctrlakeymap = {yes &#124; no}      |                                                                                     Especifica se você deseja que o servidor telnet para interpretar CTRL + A como ALT. tipo de **Sim** para a tecla de atalho do mapa ou um tipo **nenhuma** para impedir que o mapeamento.                                                                                     |
|       config timeout = \<hh>:\<mm>:\<ss>       |                                                                                                                                 Define o período de tempo limite em horas, minutos e segundos.                                                                                                                                 |
|     config timeoutactive = {yes &#124; no      |                                                                                                                                            Permite que o tempo limite da sessão ociosa.                                                                                                                                             |
|          config maxfail = \<attempts>          |                                                                                                                          Define o número máximo de tentativas de logon com falha antes de desconectar.                                                                                                                          |
|        config maxconn = \<Connections>         |                                                                                                                                         Define o número máximo de conexões.                                                                                                                                          |
|            config port = <\Number>             |                                                                                                                    Define a porta do telnet. Você deve especificar a porta com um número inteiro menor que 1024.                                                                                                                    |
| config sec {+ &#124; -}NTLM {+ &#124; -}passwd | Especifica se você deseja usar o NTLM, uma senha ou ambos para autenticar tentativas de logon. Para usar um determinado tipo de autenticação, digite um sinal de adição ( **+** ) antes do tipo de autenticação. Para impedir o uso de um determinado tipo de autenticação, digite um sinal de subtração ( **-** ) antes do tipo de autenticação. |
|     config mode = {console &#124; stream}      |                                                                                                                                             Especifica o modo de operação.                                                                                                                                             |
|                       -?                       |                                                                                                                                           Exibe a ajuda no prompt de comando.                                                                                                                                           |

## <a name="remarks"></a>Comentários  
-   Para exibir as configurações do servidor, digite **tlntadmn** sem parâmetros.  
-   Para usar o **tlntadmn** de comando, você deve fazer logon no computador local com credenciais administrativas. Para administrar um computador remoto, você também deve fornecer credenciais administrativas para o computador remoto. Você pode fazer isso fazendo logon no computador local com uma conta que tenha credenciais administrativas para o computador local e o computador remoto. Se você não pode usar esse método, você pode usar o **-u** e **-p** parâmetros para fornecer credenciais administrativas para o computador remoto.  

## <a name="BKMK_Examples"></a>Exemplos  
Configure o tempo limite de sessão ociosa como 30 minutos.  
```  
tlntadmn config timeout=0:30:0  
```  
Exibir sessões de telnet de Active Directory.  
```  
tlntadmn -s  
```  

## <a name="additional-references"></a>Referências adicionais  
-   [Guia de operações do Telnet](https://technet.microsoft.com/library/cc753164(v=ws.10).aspx)  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
