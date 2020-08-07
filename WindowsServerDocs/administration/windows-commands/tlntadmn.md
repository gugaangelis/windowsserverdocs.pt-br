---
title: tlntadmn
description: Artigo de referência para o tlntadmn, que administra um computador local ou remoto, executando o serviço do servidor Telnet.
ms.topic: article
ms.assetid: 78b61e8d-b953-44bb-8d57-f3b42da9e7a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4dee63c0233bab5341dae724ccd9779c3917054c
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87897149"
---
# <a name="tlntadmn"></a>tlntadmn

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Administra um computador local ou remoto que está executando o serviço do servidor Telnet.

## <a name="syntax"></a>Sintaxe
```
tlntadmn [<computerName>] [-u <UserName>] [-p <Password>] [{start | stop | pause | continue}] [-s {<SessionID> | all}] [-k {<SessionID> | all}] [-m {<SessionID> | all}  <Message>] [config [dom = <Domain>] [ctrlakeymap = {yes | no}] [timeout = <hh>:<mm>:<ss>] [timeoutactive = {yes | no}] [maxfail = <attempts>] [maxconn = <Connections>] [port = <Number>] [sec {+ | -}NTLM {+ | -}passwd] [mode = {console | stream}]] [-?]
```
#### <a name="parameters"></a>Parâmetros

|                   Parâmetro                    |                                                                                                                                                       Descrição                                                                                                                                                        |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                \<computerName>                 |                                                                                                                    Especifica o nome do servidor ao qual se conectar. O padrão é o computador local.                                                                                                                    |
|         -u \<UserName> -p\<Password>          |                                                Especifica as credenciais administrativas para um servidor remoto que você deseja administrar. Esse parâmetro será necessário se você quiser administrar um servidor remoto no qual você não está conectado com credenciais administrativas.                                                |
|                     start                      |                                                                                                                                            inicia o serviço do servidor Telnet.                                                                                                                                             |
|                      parar                      |                                                                                                                                             Interrompe o serviço do servidor Telnet                                                                                                                                              |
|                     pause                      |                                                                                                                          pausa o serviço do servidor Telnet. Nenhuma nova conexão será aceita.                                                                                                                          |
|                    continue                    |                                                                                                                                            Retoma o serviço do servidor Telnet.                                                                                                                                            |
|          -s { \<SessionID> &#124; todos}          |                                                                                                                                             Exibe sessões telnet ativas.                                                                                                                                             |
|          -k { \<SessionID> &#124; todos}          |                                                                                                        Encerra sessões telnet. Digite a ID da sessão para encerrar uma sessão específica ou digite All para encerrar todas as sessões.                                                                                                         |
|    -m { \<SessionID> &#124; todos}<Message>     |                                                   Envia uma mensagem para uma ou mais sessões. Digite a ID da sessão para enviar uma mensagem a uma sessão específica ou digite todos para enviar uma mensagem a todas as sessões. Digite a mensagem que você deseja enviar entre aspas.                                                   |
|             config dom =\<Domain>             |                                                                                                                                      Configura o domínio padrão para o servidor.                                                                                                                                       |
|      config ctrlakeymap = {Sim &#124; não}      |                                                                                     Especifica se você deseja que o servidor Telnet interprete CTRL + A como ALT. Digite **Sim** para mapear a tecla de atalho ou digite **não** para impedir o mapeamento.                                                                                     |
|       tempo limite de configuração = \<hh> : \<mm> :\<ss>       |                                                                                                                                 Define o período de tempo limite em horas, minutos e segundos.                                                                                                                                 |
|     config timeoutactive = {Sim &#124; não      |                                                                                                                                            Habilita o tempo limite da sessão ociosa.                                                                                                                                             |
|          config maxfail =\<attempts>          |                                                                                                                          Define o número máximo de tentativas de logon com falha antes de desconectar.                                                                                                                          |
|        config maxconn =\<Connections>         |                                                                                                                                         Define o número máximo de conexões.                                                                                                                                          |
|            porta de configuração = < \Number>             |                                                                                                                    Define a porta Telnet. Você deve especificar a porta com um inteiro menor que 1024.                                                                                                                    |
| config SEC {+ &#124;-} NTLM {+ &#124;-} passwd | Especifica se você deseja usar NTLM, uma senha ou ambas para autenticar tentativas de logon. Para usar um tipo específico de autenticação, digite um sinal de adição ( **+** ) antes desse tipo de autenticação. Para evitar o uso de um tipo específico de autenticação, digite um sinal de subtração ( **-** ) antes desse tipo de autenticação. |
|     modo de configuração = {fluxo de &#124; do console}      |                                                                                                                                             Especifica o modo de operação.                                                                                                                                             |
|                       -?                       |                                                                                                                                           Exibe a ajuda no prompt de comando.                                                                                                                                           |

## <a name="remarks"></a>Comentários
-   Para exibir as configurações do servidor, digite **tlntadmn** sem nenhum parâmetro.
-   Para usar o comando **tlntadmn** , você deve fazer logon no computador local com credenciais administrativas. Para administrar um computador remoto, você também deve fornecer credenciais administrativas para o computador remoto. Você pode fazer isso fazendo logon no computador local com uma conta que tenha credenciais administrativas para o computador local e o computador remoto. Se você não puder usar esse método, poderá usar os parâmetros **-u** e **-p** para fornecer credenciais administrativas para o computador remoto.

## <a name="examples"></a>Exemplos
Configure o tempo limite da sessão ociosa para 30 minutos.
```
tlntadmn config timeout=0:30:0
```
Exibir sessões telnet ativas.
```
tlntadmn -s
```

## <a name="additional-references"></a>Referências adicionais
-   [Guia de operações de Telnet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753164(v=ws.10))
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
