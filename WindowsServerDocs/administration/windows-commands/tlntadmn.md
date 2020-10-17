---
title: tlntadmn
description: Artigo de referência para o comando tlntadmn, que administra um computador local ou remoto, executando o serviço do servidor Telnet.
ms.topic: reference
ms.assetid: 78b61e8d-b953-44bb-8d57-f3b42da9e7a8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9a4de6903d5a9979f45677176d023c694e20cdfd
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156442"
---
# <a name="tlntadmn"></a>tlntadmn

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Administra um computador local ou remoto que está executando o serviço do servidor Telnet. Se usado sem parâmetros, **tlntadmn** exibirá as configurações atuais do servidor.

Esse comando exige que você faça logon no computador local com credenciais administrativas. Para administrar um computador remoto, você também deve fornecer credenciais administrativas para o computador remoto. Você pode fazer isso fazendo logon no computador local com uma conta que tenha credenciais administrativas para o computador local e o computador remoto. Se você não puder usar esse método, poderá usar os parâmetros **-u** e **-p** para fornecer credenciais administrativas para o computador remoto.

## <a name="syntax"></a>Sintaxe

```
tlntadmn [<computername>] [-u <username>] [-p <password>] [{start | stop | pause | continue}] [-s {<sessionID> | all}] [-k {<sessionID> | all}] [-m {<sessionID> | all}  <message>] [config [dom = <domain>] [ctrlakeymap = {yes | no}] [timeout = <hh>:<mm>:<ss>] [timeoutactive = {yes | no}] [maxfail = <attempts>] [maxconn = <connections>] [port = <number>] [sec {+ | -}NTLM {+ | -}passwd] [mode = {console | stream}]] [-?]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<computername>` | Especifica o nome do servidor ao qual se conectar. O padrão é o computador local. |
| -u `<username> -p <password>` | Especifica as credenciais administrativas para um servidor remoto que você deseja administrar. Esse parâmetro será necessário se você quiser administrar um servidor remoto no qual você não está conectado com credenciais administrativas. |
| start | inicia o serviço do servidor Telnet. |
| parar | Interrompe o serviço do servidor Telnet |
| pause | Pausa o serviço do servidor Telnet. Nenhuma nova conexão será aceita. |
| continue | Retoma o serviço do servidor Telnet. |
| -s `{<sessionID> | all}` | Exibe sessões telnet ativas. |
| -k `{<sessionID> | all}` | Encerra sessões telnet. Digite a ID da sessão para encerrar uma sessão específica ou digite All para encerrar todas as sessões. |
| -m `{<sessionID> | all}  <message>` | Envia uma mensagem para uma ou mais sessões. Digite a ID da sessão para enviar uma mensagem a uma sessão específica ou digite todos para enviar uma mensagem a todas as sessões. Digite a mensagem que você deseja enviar entre aspas. |
| config dom = `<domain>` | Configura o domínio padrão para o servidor. |
| config ctrlakeymap = `{yes | no}` | Especifica se você deseja que o servidor Telnet interprete CTRL + A como ALT. Digite **Sim** para mapear a tecla de atalho ou digite **não** para impedir o mapeamento. |
| tempo limite de configuração = `<hh>:<mm>:<ss>` | Define o período de tempo limite em horas, minutos e segundos. |
| config timeoutactive = `{yes | no}` | Habilita o tempo limite da sessão ociosa. |
| config maxfail = `<attempts>` | Define o número máximo de tentativas de logon com falha antes de desconectar. |
| config maxconn = `<connections>` | Define o número máximo de conexões. |
| porta de configuração = `<number>` | Define a porta Telnet. Você deve especificar a porta com um inteiro menor que 1024. |
| configuração s `{+ | -}NTLM {+ | -}passwd` | Especifica se você deseja usar NTLM, uma senha ou ambas para autenticar tentativas de logon. Para usar um tipo específico de autenticação, digite um sinal de adição ( **+** ) antes desse tipo de autenticação. Para evitar o uso de um tipo específico de autenticação, digite um sinal de subtração ( **-** ) antes desse tipo de autenticação. |
| modo de configuração = `{console | stream}` | Especifica o modo de operação. |
| -? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para configurar o tempo limite da sessão ociosa para 30 minutos, digite:

```
tlntadmn config timeout=0:30:0
```

Para exibir sessões telnet ativas, digite:

```
tlntadmn -s
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Guia de operações de Telnet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753164(v=ws.10))
