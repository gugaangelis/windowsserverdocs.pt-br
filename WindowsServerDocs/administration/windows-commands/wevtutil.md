---
title: wevtutil
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d4c791e0-7e59-45c5-aa55-0223b77a4822 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21f3b721a2a08a7fa101ec09f1f11b5e984f0113
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362171"
---
# <a name="wevtutil"></a>wevtutil



Permite que você recupere informações sobre logs de eventos e editores. Também é possível usar esse comando para instalar e desinstalar manifestos de eventos, para executar consultas e para exportar, arquivar e limpar os logs. Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
wevtutil [{el | enum-logs}] [{gl | get-log} <Logname> [/f:<Format>]]
[{sl | set-log} <Logname> [/e:<Enabled>] [/i:<Isolation>] [/lfn:<Logpath>] [/rt:<Retention>] [/ab:<Auto>] [/ms:<MaxSize>] [/l:<Level>] [/k:<Keywords>] [/ca:<Channel>] [/c:<Config>]] 
[{ep | enum-publishers}] 
[{gp | get-publisher} <Publishername> [/ge:<Metadata>] [/gm:<Message>] [/f:<Format>]] [{im | install-manifest} <Manifest>] 
[{um | uninstall-manifest} <Manifest>] [{qe | query-events} <Path> [/lf:<Logfile>] [/sq:<Structquery>] [/q:<Query>] [/bm:<Bookmark>] [/sbm:<Savebm>] [/rd:<Direction>] [/f:<Format>] [/l:<Locale>] [/c:<Count>] [/e:<Element>]] 
[{gli | get-loginfo} <Logname> [/lf:<Logfile>]] 
[{epl | export-log} <Path> <Exportfile> [/lf:<Logfile>] [/sq:<Structquery>] [/q:<Query>] [/ow:<Overwrite>]] 
[{al | archive-log} <Logpath> [/l:<Locale>]] 
[{cl | clear-log} <Logname> [/bu:<Backup>]] [/r:<Remote>] [/u:<Username>] [/p:<Password>] [/a:<Auth>] [/uni:<Unicode>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|{El \| enum-logs}|Exibe os nomes de todos os logs.|
|{GL \| get-log} \<LogName > [/f:\<Format >]|Exibe informações de configuração para o log especificado, que inclui se o log está habilitado ou não, o limite de tamanho máximo atual do log e o caminho para o arquivo em que o log está armazenado.|
|{SL \| Set-log} \<LogName > [/e:\<habilitado >] [/i:\<> de isolamento] [/LFN:\<logPath >] [/RT:\<Retention >] [/AB:\<auto >] [/MS:\<MaxSize >] [/l:\<nível >] [/k:\<Keywords >] [/CA:\<Channel >] [/c:\<config >]|Modifica a configuração do log especificado.|
|{EP \| enum-Publishers}|Exibe os editores de eventos no computador local.|
|{GP \| Get-Publisher} \<PublisherName > [/GE:\<de metadados >] [/GM:\<Message >] [/f:\<Format >]]|Exibe as informações de configuração para o Publicador de eventos especificado.|
|{im \| instalar-manifest} \<manifesto >|Instala editores e logs de eventos de um manifesto. Para obter mais informações sobre manifestos de evento e como usar esse parâmetro, consulte o SDK do log de eventos do Windows no site do Microsoft Developers Network (MSDN) ([https://msdn.microsoft.com](https://msdn.microsoft.com)).|
|{um \| desinstalar-manifest} \<manifesto >|Desinstala todos os Publicadores e logs de um manifesto. Para obter mais informações sobre manifestos de evento e como usar esse parâmetro, consulte o SDK do log de eventos do Windows no site do Microsoft Developers Network (MSDN) ([https://msdn.microsoft.com](https://msdn.microsoft.com)).|
|{qe \| Query-Events} \<caminho > [/LF:\<logfile >] [/sq:\<Structquery >] [/q:\<consulta >] [/BM:\<Bookmark >] [/SBM:\<Savebm >] [/RD:\<direção >] [/f:\<Format >] [/l:\<locale >] [/c:\<contagem >] [/e:\<elemento >]|Lê eventos de um log de eventos, de um arquivo de log ou usando uma consulta estruturada. Por padrão, você fornece um nome de log para \<caminho >. No entanto, se você usar a opção **/LF** , \<caminho > deverá ser um caminho para um arquivo de log. Se você usar o parâmetro **/sq** , \<caminho > deverá ser um caminho para um arquivo que contenha uma consulta estruturada.|
|{Gli \| Get-loginfo} \<LogName > [/LF:\<logfile >]|Exibe informações de status sobre um log de eventos ou arquivo de log. Se a opção **/LF** for usada, \<LogName > será um caminho para um arquivo de log. Você pode executar **wevtutil El** para obter uma lista de nomes de log.|
|{EPL \| export-log} \<caminho > \<exportfile > [/LF:\<logfile >] [/sq:\<Structquery >] [/q:\<consulta >] [/ow:\<overwrite >]|Exporta eventos de um log de eventos, de um arquivo de log ou usando uma consulta estruturada para o arquivo especificado. Por padrão, você fornece um nome de log para \<caminho >. No entanto, se você usar a opção **/LF** , \<caminho > deverá ser um caminho para um arquivo de log. Se você usar a opção **/sq** , \<caminho > deverá ser um caminho para um arquivo que contenha uma consulta estruturada. \<exportfile > é um caminho para o arquivo em que os eventos exportados serão armazenados.|
|{Al \| arquivo-log} \<logPath > [/l:\<localidade >]|Arquiva o arquivo de log especificado em um formato autônomo. Um subdiretório com o nome da localidade é criado e todas as informações específicas de localidade são salvas nesse subdiretório. Depois que o diretório e o arquivo de log são criados executando **wevtutil Al**, os eventos no arquivo podem ser lidos se o Publicador estiver instalado ou não.|
|{CL \| Clear-log} \<LogName > [/BU:\<backup >]|Limpa os eventos do log de eventos especificado. A opção **/Bu** pode ser usada para fazer backup dos eventos limpos.|

## <a name="options"></a>Opções

|       Opção       |                                                                                                                                                                                                                                                                 Descrição                                                                                                                                                                                                                                                                  |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /f: formato de\<>    |                                                                                                                                                               Especifica que a saída deve ser XML ou formato de texto. Se \<formato > for XML, a saída será exibida no formato XML. Se \<formato > for texto, a saída será exibida sem marcas XML. O padrão é Text.                                                                                                                                                                |
|   /e:\<habilitado >    |                                                                                                                                                                                                                                         Habilita ou desabilita um log. \<habilitado > pode ser verdadeiro ou falso.                                                                                                                                                                                                                                          |
|  /i: > de isolamento de\<   | Define o modo de isolamento de log. o isolamento de \<> pode ser sistema, aplicativo ou personalizado. O modo de isolamento de um log determina se um log compartilha uma sessão com outros logs na mesma classe de isolamento. Se você especificar o isolamento do sistema, o log de destino compartilhará pelo menos permissões de gravação com o log do sistema. Se você especificar o isolamento do aplicativo, o log de destino compartilhará pelo menos permissões de gravação com o log do aplicativo. Se você especificar o isolamento personalizado, também deverá fornecer um descritor de segurança usando a opção **/CA** . |
|  /LFN:\<logPath >   |                                                                                                                                                                                                           Define o nome do arquivo de log. \<logPath > é um caminho completo para o arquivo em que o serviço de log de eventos armazena eventos para esse log.                                                                                                                                                                                                           |
|  /RT: > de retenção de\<  |                                                            Define o modo de retenção de log. \<> de retenção pode ser verdadeiro ou falso. O modo de retenção de log determina o comportamento do serviço log de eventos quando um log atinge seu tamanho máximo. Se um log de eventos atingir seu tamanho máximo e o modo de retenção de log for verdadeiro, os eventos existentes serão mantidos e os eventos de entrada serão descartados. Se o modo de retenção de log for false, os eventos de entrada substituirão os eventos mais antigos no log.                                                             |
|    /AB: > automático de\<     |                                                                                                                                   Especifica a política de backup automático de log. \<> automático pode ser true ou false. Se esse valor for true, o backup do log será feito automaticamente quando atingir o tamanho máximo. Se esse valor for true, a retenção (especificada com a opção **/RT** ) também deverá ser definida como true.                                                                                                                                    |
|   /MS:\<MaxSize >   |                                                                                                                                                                        Define o tamanho máximo do log em bytes. O tamanho mínimo do log é de 1048576 bytes (1024KB) e os arquivos de log são sempre múltiplos de 64 KB, portanto, o valor inserido será arredondado de acordo.                                                                                                                                                                         |
|    /l: nível de\<>     |                                                                                                                                                                     Define o filtro de nível do log. o nível de \<> pode ser qualquer valor de nível válido. Essa opção só é aplicável a logs com uma sessão dedicada. Você pode remover um filtro de nível definindo <Level> como 0.                                                                                                                                                                      |
|   /k: palavras-chave\<>   |                                                                                                                                                                                         Especifica o filtro de palavras-chave do log. \<palavras-chave > pode ser qualquer máscara de palavra-chave válida de 64 bits. Essa opção só é aplicável a logs com uma sessão dedicada.                                                                                                                                                                                         |
|   /CA: > de canal de\<   |                                                                                                                   Define a permissão de acesso para um log de eventos. \<> de canal é um descritor de segurança que usa o SDDL (Security Descriptor Definition Language). Para obter mais informações sobre o formato SDDL, consulte o site da Web do Microsoft Developers Network (MSDN) ([https://msdn.microsoft.com](https://msdn.microsoft.com)).                                                                                                                    |
|    /c: configuração do\<>    |                                                                                                                                  Especifica o caminho para um arquivo de configuração. Essa opção fará com que as propriedades de log sejam lidas do arquivo de configuração definido em \<> config. Se você usar essa opção, não deverá especificar um parâmetro <Logname>. O nome do log será lido no arquivo de configuração.                                                                                                                                   |
|  /GE: metadados de\<>   |                                                                                                                                                                                                                 Obtém informações de metadados para eventos que podem ser gerados por este Publicador. \<metadados > podem ser verdadeiros ou falsos.                                                                                                                                                                                                                 |
|   /GM: mensagem de\<>   |                                                                                                                                                                                                                       Exibe a mensagem real em vez da ID da mensagem numérica. \<mensagem > pode ser true ou false.                                                                                                                                                                                                                        |
|   /LF: > do arquivo de log de\<   |                                                                                                                                                                                  Especifica que os eventos devem ser lidos de um log ou de um arquivo de log. \<> de arquivo de log pode ser true ou false. Se for true, o parâmetro para o comando será o caminho para um arquivo de log.                                                                                                                                                                                   |
| /sq:\<Structquery > |                                                                                                                                                                                Especifica que os eventos devem ser obtidos com uma consulta estruturada. \<Structquery > pode ser true ou false. Se for true, <Path> será o caminho para um arquivo que contém uma consulta estruturada.                                                                                                                                                                                |
|    /q: consulta de\<>     |                                                                                                                                                                     Define a consulta XPath para filtrar os eventos que são lidos ou exportados. Se essa opção não for especificada, todos os eventos serão retornados ou exportados. Essa opção não estará disponível quando **/sq** for true.                                                                                                                                                                     |
|  /BM:\<indicador >   |                                                                                                                                                                                                                                 Especifica o caminho para um arquivo que contém um indicador de uma consulta anterior.                                                                                                                                                                                                                                 |
|   /SBM:\<Savebm >   |                                                                                                                                                                                                             Especifica o caminho para um arquivo que é usado para salvar um indicador dessa consulta. A extensão de nome de arquivo deve ser. xml.                                                                                                                                                                                                              |
|  /RD: direção de\<>  |                                                                                                                                                                                                   Especifica a direção na qual os eventos são lidos. \<direção > pode ser true ou false. Se for true, os eventos mais recentes serão retornados primeiro.                                                                                                                                                                                                   |
|    /l:\<> de localidade    |                                                                                                                                                                                          Define uma cadeia de caracteres de localidade que é usada para imprimir o texto do evento em uma localidade específica. Disponível somente ao imprimir eventos no formato de texto usando a opção **/f** .                                                                                                                                                                                          |
|    /c: contagem de\<>     |                                                                                                                                                                                                                                                  Define o número máximo de eventos a serem lidos.                                                                                                                                                                                                                                                  |
|   /e:\<elemento >    |                                                                                                                                                           Inclui um elemento raiz ao exibir eventos em XML. \<elemento > é a cadeia de caracteres que você deseja dentro do elemento raiz. Por exemplo, **/e: root** resultaria em XML que contém o par de elementos raiz \<</root>de > raiz.                                                                                                                                                            |
|  /ow: substituição de\<>  |                                                                                                                                                                 Especifica que o arquivo de exportação deve ser substituído. \<substituir > pode ser true ou false. Se for true, e o arquivo de exportação especificado em <Exportfile> já existir, ele será substituído sem confirmação.                                                                                                                                                                 |
|   /Bu: > de backup do\<    |                                                                                                                                                                                                      Especifica o caminho para um arquivo em que os eventos limpos serão armazenados. Inclua a extensão. evtx no nome do arquivo de backup.                                                                                                                                                                                                       |
|    /r:\<> remoto    |                                                                                                                                                                                            Executa o comando em um computador remoto. \<> remoto é o nome do computador remoto. Os parâmetros **im** e **um** não dão suporte à operação remota.                                                                                                                                                                                            |
|   /u:\<nome de usuário >   |                                                                                                                                                                          Especifica um usuário diferente para fazer logon em um computador remoto. \<username > é um nome de usuário no formato domínio/usuário. Essa opção só é aplicável quando a opção **/r** é especificada.                                                                                                                                                                          |
|   /p:\<a senha >   |                                                                                                                                               Especifica a senha do usuário. Se a opção **/u** for usada e essa opção não for especificada ou \<a senha > for " *", será solicitado que o usuário insira uma senha. Essa opção só é aplicável quando a opção \*\*/u*\* é especificada.                                                                                                                                                |
|     /a:\<> de autenticação     |                                                                                                                                                                                             Define o tipo de autenticação para se conectar a um computador remoto. \<> de autenticação podem ser default, Negotiate, Kerberos ou NTLM. O padrão é Negotiate.                                                                                                                                                                                              |
|  /Uni: > Unicode\<   |                                                                                                                                                                                                             Exibe a saída em Unicode. \<> Unicode pode ser true ou false. Se <Unicode> for true, a saída será em Unicode.                                                                                                                                                                                                             |

## <a name="remarks"></a>Comentários

-   Usando um arquivo de configuração com o parâmetro SL

    O arquivo de configuração é um arquivo XML com o mesmo formato que a saída de wevtutil GL \<LogName >/f: XML. O exemplo a seguir mostra o formato de um arquivo de configuração que habilita a retenção, habilita o backup e define o tamanho máximo do log no log do aplicativo:  
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <channel name="Application" isolation="Application"
    xmlns="https://schemas.microsoft.com/win/2004/08/events">
    <logging>
    <retention>true</retention>
    <autoBackup>true</autoBackup>
    <maxSize>9000000</maxSize>
    </logging>
    <publishing>
    </publishing>
    </channel>
    ```

## <a name="BKMK_examples"></a>Disso

Listar os nomes de todos os logs:
```
wevtutil el
```
Exibe informações de configuração sobre o log do sistema no computador local em formato XML:
```
wevtutil gl System /f:xml
```
Use um arquivo de configuração para definir atributos de log de eventos (consulte comentários para obter um exemplo de um arquivo de configuração):
```
wevtutil sl /c:config.xml
```
Exiba informações sobre o Publicador de eventos Microsoft-Windows-EventLog, incluindo metadados sobre os eventos que o Publicador pode gerar:
```
wevtutil gp Microsoft-Windows-Eventlog /ge:true
```
Instale Publicadores e logs do arquivo de manifesto mymanifest. xml:
```
wevtutil im myManifest.xml
```
Desinstalar Publicadores e logs do arquivo de manifesto mymanifest. xml:
```
wevtutil um myManifest.xml
```
Exiba os três eventos mais recentes do log do aplicativo no formato textual:
```
wevtutil qe Application /c:3 /rd:true /f:text
```
Exibir o status do log do aplicativo:
```
wevtutil gli Application 
```
Exportar eventos do log do sistema para C:\backup\system0506.evtx:
```
wevtutil epl System C:\backup\system0506.evtx
```
Limpe todos os eventos do log do aplicativo depois de salvá-los no C:\admin\backups\a10306.evtx:
```
wevtutil cl Application /bu:C:\admin\backups\a10306.evtx
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
