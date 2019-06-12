---
title: wevtutil
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d99b1f1da3bcf2c59236ce9a8c41f933b100bf7a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440104"
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
|{el \| enum logs}|Exibe os nomes de todos os logs.|
|{gl \| get-log} \<Logname > [/ f:\<formato >]|Exibe informações de configuração para o log especificado, que inclui se o log está habilitado ou não, o limite de tamanho máximo atual do log e o caminho do arquivo onde o log está armazenado.|
|{sl \| log do conjunto} \<Logname > [/ e:\<habilitado >] [/ i:\<isolamento >] [/ lfn:\<Logpath >] [/ rt:\<retenção >] [/ ab:\<automática >] [/ ms:\< MaxSize >] [/ l:\<nível >] [/ nexar\<palavras-chave >] [/ autoridade de certificação:\<canal >] [/ c:\<Config >]|Modifica a configuração de log especificado.|
|{ep \| enum Publicadores}|Exibe os editores de eventos no computador local.|
|{gp \| get-publisher} \<Publishername > [/ ge:\<metadados >] [/ gm:\<mensagem >] [/ f:\<formato >]]|Exibe as informações de configuração para o publicador do evento especificado.|
|{im \| manifesto instalação} \<manifesto >|Instala os logs e editores de eventos de um manifesto. Para obter mais informações sobre manifestos de evento e o uso desse parâmetro, consulte o SDK do Log de eventos do Windows no site da Microsoft Developers Network (MSDN) ([https://msdn.microsoft.com](https://msdn.microsoft.com)).|
|{Hum \| manifesto desinstalar} \<manifesto >|Desinstala todos os logs e editores de um manifesto. Para obter mais informações sobre manifestos de evento e o uso desse parâmetro, consulte o SDK do Log de eventos do Windows no site da Microsoft Developers Network (MSDN) ([https://msdn.microsoft.com](https://msdn.microsoft.com)).|
|{qe \| eventos de consulta} \<caminho > [/ lf:\<arquivo de log >] [/ sq:\<Structquery >] [/ p:\<consulta >] [/ bm:\<indicador >] [/ sbm:\<Savebm >] [/ área de trabalho remota:\< Direção >] [/ f:\<formato >] [/ l:\<localidade >] [/ c:\<contagem >] [/ e:\<elemento >]|Lê os eventos de um log de eventos de um arquivo de log, ou usando uma consulta estruturada. Por padrão, você fornecer um nome de log para \<caminho >. No entanto, se você usar o **/lf** opção, em seguida, \<caminho > deve ser um caminho para um arquivo de log. Se você usar o **/sq** parâmetro, \<caminho > deve ser um caminho para um arquivo que contém uma consulta estruturada.|
|{gli \| get-loginfo} \<Logname > [/ lf:\<Logfile >]|Exibe informações de status sobre um log de eventos ou o arquivo de log. Se o **/lf** opção for usada, \<Logname > é um caminho para um arquivo de log. Você pode executar **wevtutil el** para obter uma lista de nomes de log.|
|{epl \| log de exportação} \<caminho > \<arquivo de exportação > [/ lf:\<Logfile >] [/ sq:\<Structquery >] [/ p:\<consulta >] [/ omo:\<substituição >]|Exporta os eventos de um log de eventos de um arquivo de log, ou usando uma consulta a estruturadas no arquivo especificado. Por padrão, você fornecer um nome de log para \<caminho >. No entanto, se você usar o **/lf** opção, em seguida, \<caminho > deve ser um caminho para um arquivo de log. Se você usar o **/sq** opção, \<caminho > deve ser um caminho para um arquivo que contém uma consulta estruturada. \<Arquivo de exportação > é um caminho para o arquivo onde os eventos exportados serão armazenados.|
|{al \| log de arquivo morto} \<Logpath > [/ l:\<localidade >]|O arquivo de log especificado em um formato independente de arquivos. Um subdiretório com o nome da localidade é criado e todas as informações específicas de localidade é salvo no subdiretório. Depois que o arquivo de log e de diretório são criados pela execução **wevtutil al**, eventos no arquivo podem ser lidos se o publicador é instalado ou não.|
|{cl \| clear-log} \<Logname > [/ bu:\<Backup >]|Limpa os eventos do log de eventos especificado. O **/bu** opção pode ser usada para fazer backup os eventos desmarcados.|

## <a name="options"></a>Opções

|       Opção       |                                                                                                                                                                                                                                                                 Descrição                                                                                                                                                                                                                                                                  |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /f:\<Format>    |                                                                                                                                                               Especifica que a saída deve ser o formato de texto ou XML. Se \<formato > é XML, a saída é exibida no formato XML. Se \<formato > é texto, a saída é exibida sem marcas XML. O padrão é texto.                                                                                                                                                                |
|   /e:\<Enabled>    |                                                                                                                                                                                                                                         Habilita ou desabilita um log. \<Habilitado > pode ser true ou false.                                                                                                                                                                                                                                          |
|  /i:\<isolamento >   | Define o modo de isolamento de log. \<Isolamento > pode ser um sistema, aplicativo ou personalizado. O modo de isolamento de um log determina se um log compartilha uma sessão com outros logs na mesma classe de isolamento. Se você especificar o isolamento do sistema, o log de destino irá compartilhar pelo menos permissões com o log do sistema de gravação. Se você especificar o isolamento de aplicativos, o log de destino irá compartilhar pelo menos permissões com o log de aplicativo de gravação. Se você especificar isolamento personalizado, você também deve fornecer um descritor de segurança usando o **/ca** opção. |
|  /lfn:\<Logpath>   |                                                                                                                                                                                                           Define o nome do arquivo de log. \<LogPath > é um caminho completo para o arquivo em que o serviço de Log de eventos armazena os eventos para esse log.                                                                                                                                                                                                           |
|  /RT:\<retenção >  |                                                            Define o modo de retenção de log. \<Retenção > pode ser true ou false. O modo de retenção de log determina o comportamento do serviço de Log de eventos quando um log atinge o tamanho máximo. Se um log de eventos atinge seu tamanho máximo e o modo de retenção de log for true, os eventos existentes são mantidos e eventos de entrada são descartados. Se o modo de retenção de log for false, os eventos de entrada substituem os eventos mais antigos no log.                                                             |
|    /ab:\<Auto>     |                                                                                                                                   Especifica a política de backup automático de log. \<Auto > pode ser true ou false. Se esse valor for true, o log será feito backup automaticamente quando ele atinge o tamanho máximo. Se esse valor for true, a retenção (especificado com o **/rt** opção) também deve ser definido como true.                                                                                                                                    |
|   /ms:\<MaxSize>   |                                                                                                                                                                        Define o tamanho máximo do log em bytes. O tamanho mínimo do log é 1048576 bytes (1024KB) e arquivos de log sempre são múltiplos de 64KB, portanto, o valor que você inserir será arredondado adequadamente.                                                                                                                                                                         |
|    /l:\<Level>     |                                                                                                                                                                     Define o filtro de nível do log. \<Nível > pode ser qualquer valor válido de nível. Essa opção só é aplicável aos logs com uma sessão dedicado. Você pode remover um filtro de nível definindo <Level> como 0.                                                                                                                                                                      |
|   /k:\<Keywords>   |                                                                                                                                                                                         Especifica o filtro de palavras-chave do log. \<Palavras-chave > pode ser qualquer máscara de palavra-chave de 64 bits válido. Essa opção só é aplicável aos logs com uma sessão dedicado.                                                                                                                                                                                         |
|   /ca:\<Channel>   |                                                                                                                   Define a permissão de acesso para um log de eventos. \<Canal > é um descritor de segurança que usa a definição de linguagem SDDL (Security Descriptor). Para obter mais informações sobre o formato SDDL, consulte o site Microsoft Developers Network (MSDN) ([https://msdn.microsoft.com](https://msdn.microsoft.com)).                                                                                                                    |
|    /c:\<Config>    |                                                                                                                                  Especifica o caminho para um arquivo de configuração. Essa opção fará com que as propriedades de log a ser lido do arquivo de configuração definido no \<Config >. Se você usa essa opção, você deve especificar um <Logname> parâmetro. O nome do log será lido do arquivo de configuração.                                                                                                                                   |
|  /GE:\<metadados >   |                                                                                                                                                                                                                 Obtém informações de metadados para eventos que podem ser gerados por este publicador. \<Metadados > pode ser true ou false.                                                                                                                                                                                                                 |
|   /GM:\<mensagem >   |                                                                                                                                                                                                                       Exibe a mensagem real em vez de ID da mensagem numérica. \<Mensagem > pode ser true ou false.                                                                                                                                                                                                                        |
|   /LF:\<Logfile >   |                                                                                                                                                                                  Especifica que os eventos devem ser lidos a partir de um log ou um arquivo de log. \<Arquivo de log > pode ser true ou false. Se for true, o parâmetro para o comando é o caminho para um arquivo de log.                                                                                                                                                                                   |
| /sq:\<Structquery> |                                                                                                                                                                                Especifica que os eventos devem ser obtidos com uma consulta estruturada. \<Structquery > pode ser true ou false. Se for true, <Path> é o caminho para um arquivo que contém uma consulta estruturada.                                                                                                                                                                                |
|    /q:\<Query>     |                                                                                                                                                                     Define a consulta XPath para filtrar os eventos que são lidas ou exportados. Se essa opção não for especificada, todos os eventos serão retornados ou exportados. Essa opção não está disponível quando **/sq** é verdadeiro.                                                                                                                                                                     |
|  /BM:\<indicador >   |                                                                                                                                                                                                                                 Especifica o caminho para um arquivo que contém um indicador de uma consulta anterior.                                                                                                                                                                                                                                 |
|   /sbm:\<Savebm>   |                                                                                                                                                                                                             Especifica o caminho para um arquivo que é usado para salvar um indicador dessa consulta. A extensão de nome de arquivo deve ser. XML.                                                                                                                                                                                                              |
|  /RD:\<direção >  |                                                                                                                                                                                                   Especifica a direção na qual os eventos são lidos. \<Direção > pode ser true ou false. Se for true, os eventos mais recentes são retornados primeiro.                                                                                                                                                                                                   |
|    /l:\<Locale>    |                                                                                                                                                                                          Define uma cadeia de caracteres de localidade que é usada para imprimir o texto do evento em uma localidade específica. Disponível apenas durante a impressão de eventos no formato de texto usando o **/f** opção.                                                                                                                                                                                          |
|    /c:\<Count>     |                                                                                                                                                                                                                                                  Define o número máximo de eventos a serem lidos.                                                                                                                                                                                                                                                  |
|   /e:\<Element>    |                                                                                                                                                           Inclui um elemento raiz ao exibir eventos em XML. \<Elemento > é a cadeia de caracteres que você deseja dentro do elemento raiz. Por exemplo, **/e:root** resultaria em XML que contém o par de elemento raiz \<raiz ></root>.                                                                                                                                                            |
|  /ow:\<Overwrite>  |                                                                                                                                                                 Especifica que o arquivo de exportação deve ser substituído. \<Substituir > pode ser true ou false. Se true e o arquivo de exportação especificado em <Exportfile> já existir, ela será substituída sem confirmação.                                                                                                                                                                 |
|   /bu:\<Backup>    |                                                                                                                                                                                                      Especifica o caminho para um arquivo onde os eventos desmarcados serão armazenados. Inclua a extensão. evtx no nome do arquivo de backup.                                                                                                                                                                                                       |
|    /r:\<Remote>    |                                                                                                                                                                                            Executa o comando em um computador remoto. \<Remoto > é o nome do computador remoto. O **im** e **Hum** parâmetros não dão suporte a operação remota.                                                                                                                                                                                            |
|   /u:\<Username >   |                                                                                                                                                                          Especifica um usuário diferente para fazer logon um computador remoto. \<Nome de usuário > é um nome de usuário no formato domínio \ usuário ou usuário. Essa opção só é aplicável quando o **/r** opção for especificada.                                                                                                                                                                          |
|   /p:\<Password>   |                                                                                                                                               Especifica a senha do usuário. Se o **/u** opção é usada e essa opção não for especificada ou \<senha > é " *", o usuário será solicitado a inserir uma senha. Essa opção só é aplicável quando o \* \*/u* \* opção for especificada.                                                                                                                                                |
|     /a:\<Auth>     |                                                                                                                                                                                             Define o tipo de autenticação para se conectar a um computador remoto. \<Autenticação > pode ser o padrão, Negotiate, Kerberos ou NTLM. O padrão é negociar.                                                                                                                                                                                              |
|  /uni:\<Unicode>   |                                                                                                                                                                                                             Exibe a saída em Unicode. \<Unicode > pode ser true ou false. Se <Unicode> for true, então a saída é em Unicode.                                                                                                                                                                                                             |

## <a name="remarks"></a>Comentários

-   Usando um arquivo de configuração com o parâmetro sl

    O arquivo de configuração é um arquivo XML com o mesmo formato que a saída do wevtutil gl \<Logname > /f:xml. O exemplo a seguir mostra o formato de um arquivo de configuração que permite a retenção, permite que o backup automático e define o tamanho máximo do log no log do aplicativo:  
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

## <a name="BKMK_examples"></a>Exemplos

Liste os nomes de todos os logs:
```
wevtutil el
```
Exibir informações de configuração sobre o log do sistema no computador local no formato XML:
```
wevtutil gl System /f:xml
```
Use um arquivo de configuração para definir atributos de log de eventos (consulte comentários para obter um exemplo de um arquivo de configuração):
```
wevtutil sl /c:config.xml
```
Exibir informações sobre o Editor de eventos Microsoft-Windows-Eventlog, incluindo metadados sobre os eventos que o publicador pode gerar:
```
wevtutil gp Microsoft-Windows-Eventlog /ge:true
```
Instale os logs e editores do arquivo de manifesto myManifest.xml:
```
wevtutil im myManifest.xml
```
Desinstale os logs e editores do arquivo de manifesto myManifest.xml:
```
wevtutil um myManifest.xml
```
Exiba os três eventos mais recentes do log de aplicativo no formato textual:
```
wevtutil qe Application /c:3 /rd:true /f:text
```
Exiba o status do log do aplicativo:
```
wevtutil gli Application 
```
Exporte eventos do log do sistema para C:\backup\system0506.evtx:
```
wevtutil epl System C:\backup\system0506.evtx
```
Limpe todos os eventos do log de aplicativo depois de salvá-los em C:\admin\backups\a10306.evtx:
```
wevtutil cl Application /bu:C:\admin\backups\a10306.evtx
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
