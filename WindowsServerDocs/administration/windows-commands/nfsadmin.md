---
title: nfsadmin
description: Artigo de referência para o comando nfsadmin, que gerencia tanto o servidor quanto o NFS e o Client for NFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7375b2cf-c6b8-45b5-abf6-6c10e462defd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 968c3debfafd552f295591199366c5f6c10fde47
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86956768"
---
# <a name="nfsadmin"></a>nfsadmin

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Um utilitário de linha de comando que administra o servidor para NFS ou cliente para NFS no computador local ou remoto executando os serviços da Microsoft para NFS (sistema de arquivos de rede). Usado sem parâmetros, nfsadmin server exibe o servidor atual para definições de configuração de NFS e nfsadmin client exibe o cliente atual para definições de configuração de NFS.

## <a name="syntax"></a>Sintaxe

```
nfsadmin server [computername] [-u Username [-p Password]] -l
nfsadmin server [computername] [-u Username [-p Password]] -r {client | all}
nfsadmin server [computername] [-u Username [-p Password]] {start | stop}
nfsadmin server [computername] [-u Username [-p Password]] config option[...]
nfsadmin server [computername] [-u Username [-p Password]] creategroup <name>
nfsadmin server [computername] [-u Username [-p Password]] listgroups
nfsadmin server [computername] [-u Username [-p Password]] deletegroup <name>
nfsadmin server [computername] [-u Username [-p Password]] renamegroup <oldname> <newname>
nfsadmin server [computername] [-u Username [-p Password]] addmembers <hostname>[...]
nfsadmin server [computername] [-u Username [-p Password]] listmembers
nfsadmin server [computername] [-u Username [-p Password]] deletemembers <hostname><groupname>[...]
nfsadmin client [computername] [-u Username [-p Password]] {start | stop}
nfsadmin client [computername] [-u Username [-p Password]] config option[...]
```

### <a name="general-parameters"></a>Parâmetros gerais

| Parâmetro | Descrição |
| --------- | ----------- |
| computername | Especifica o computador remoto que você deseja administrar. Você pode especificar o computador usando um nome WINS (serviço de cadastramento na Internet do Windows) ou um nome DNS (sistema de nomes de domínio) ou por endereço IP (Internet Protocol). |
| -u nome de usuário | Especifica o nome de usuário do usuário cujas credenciais devem ser usadas. Pode ser necessário adicionar o nome de domínio ao nome de usuário no formato *domínio \ nomedeusuário*. |
| -p senha | Especifica a senha do usuário especificado usando a opção **-u** . Se você especificar a opção **-u** , mas omitir a opção **-p** , será solicitada a senha do usuário. |

### <a name="server-for-nfs-related-parameters"></a>Servidor para parâmetros relacionados a NFS

| Parâmetro | Descrição |
| --------- | ----------- |
| -l | Lista todos os bloqueios mantidos pelos clientes. |
| -r`{client|all}` | Libera os bloqueios mantidos por um cliente ou, se todos forem especificados, por todos os clientes. |
| start | Inicia o servidor para o serviço NFS. |
| parar | Interrompe o serviço do servidor NFS. |
| config | Especifica as configurações gerais do servidor para NFS. Você deve fornecer pelo menos uma das seguintes opções com o argumento de comando **config** :<ul><li>**mapsvr = `<server>` ** -Define o servidor como o servidor de Mapeamento de Nomes de Usuário do servidor para NFS. Embora essa opção continue a ter suporte para compatibilidade com versões anteriores, você deve usar o utilitário sfuadmin em vez disso.</li><li>**auditlocation = `{eventlog|file|both|none}` ** -Especifica se os eventos serão auditados e onde os eventos serão registrados. Um dos argumentos a seguir é necessário:<ul><li>**EventLog** -especifica que os eventos auditados serão registrados somente no log do aplicativo visualizador de eventos.</li><li>**arquivo** -especifica que os eventos auditados serão registrados somente no arquivo especificado por `config fname` .</li><li>**ambos** -especifica que os eventos auditados serão registrados no log do aplicativo visualizador de eventos, bem como o arquivo especificado por `config fname` .</li><li>**nenhum** – especifica que os eventos não são auditados.</li></ul><li>**fname = `<file>` ** – Define o arquivo especificado pelo arquivo como o arquivo de auditoria. O padrão é **%sfudir%\log \\ nfssvr. log**.</li><li>**fsize = `<size>` ** -Define o tamanho como o tamanho máximo em megabytes do arquivo de auditoria. O tamanho máximo padrão é **7 MB**.</li><li>**`audit=[+|-]mount [+|-]read [+|-]write [+|-]create [+|-]delete [+|-]locking [+|-]all`**-Especifica os eventos a serem registrados. Para iniciar o registro em log de um evento, digite um sinal de adição ( **+** ) antes do nome do evento; para parar de registrar um evento, digite um sinal de subtração ( **-** ) antes do nome do evento. Se o sinal for omitido, o **+** sinal será assumido. Não use **todos** com qualquer outro nome de evento.</li><li>**lockperiod = `<seconds>` ** -Especifica o número de segundos que o servidor para NFS aguardará para recuperar bloqueios depois que uma conexão com o servidor para NFS for perdida e, em seguida, restabelecida ou após o servidor para o serviço NFS ter sido reiniciado.</li><li>**portmapprotocol = `{TCP|UDP|TCP+UDP}` ** -Especifica quais protocolos de transporte o portmap dá suporte. A configuração padrão é **TCP + UDP**.</li><li>**mountprotocol = `{TCP|UDP|TCP+UDP}` ** -Especifica quais protocolos de transporte a montagem oferece suporte. A configuração padrão é **TCP + UDP**.</li><li>**nfsprotocol = `{TCP|UDP|TCP+UDP}` ** -Especifica quais protocolos de transporte o sistema de arquivos de rede (NFS) dá suporte. A configuração padrão é **TCP + UDP**</li><li>**nlmprotocol = `{TCP|UDP|TCP+UDP}` ** -Especifica quais protocolos de transporte o NLM (Gerenciador de bloqueio de rede) dá suporte. A configuração padrão é **TCP + UDP**.</li><li>**nsmprotocol = `{TCP|UDP|TCP+UDP}` ** -Especifica quais protocolos de transporte o NSM (Gerenciador de status de rede) dá suporte. A configuração padrão é **TCP + UDP**.</li><li>**enableV3 = `{yes|no}` ** -Especifica se os protocolos de NFS versão 3 terão suporte. A configuração padrão é **Sim**.</li><li>**renewauth = `{yes|no}` ** -Especifica se as conexões de cliente precisarão ser reautenticadas após o período especificado por config renewauthinterval. A configuração padrão é **não**.</li><li>**renewauthinterval = `<seconds>` ** -Especifica o número de segundos decorridos antes que um cliente seja forçado a ser autenticado novamente se `config renewauth` for definido como **Sim**. O valor padrão é **600 segundos**.</li><li>**dircache = `<size>` ** -Especifica o tamanho em kilobytes do cache de diretório. O número especificado como tamanho deve ser um múltiplo de 4 entre 4 e 128. O tamanho do cache do diretório padrão é **128 KB**.</li><li>**conversãofile = `<file>` ** -Especifica um arquivo que contém informações de mapeamento para substituir caracteres nos nomes de arquivos ao movê-los de sistemas de arquivos baseados em Windows para UNIX. Se o arquivo não for especificado, a conversão de caracteres de nome de arquivo será desabilitada. Se o valor de **translationfile** for alterado, você deverá reiniciar o servidor para que a alteração entre em vigor.</li><li>**dotfileshidden = `{yes|no}` ** -Especifica se os arquivos com nomes que começam com um ponto (.) são marcados como ocultos no sistema de arquivos do Windows e, consequentemente, ocultos de clientes NFS. A configuração padrão é **não**.</li><li>**casesensitivelookups = `{yes|no}` ** -Especifica se as pesquisas de diretório diferenciam maiúsculas de minúsculas (exigem correspondência exata do caso de caractere).<p>Você também deve desabilitar a distinção entre maiúsculas e minúsculas do kernel do Windows para dar suporte a nomes de arquivos que diferenciam maiúsculas Para dar suporte à diferenciação de maiúsculas e minúsculas, altere o valor **DWORD** da chave do registro, `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\kernel` para **0**.</li><li>**ntfscase = `{lower|upper|preserve}` ** -Especifica se o caso de caracteres nos nomes de arquivos no sistema de arquivos NTFS será retornado em letras minúsculas, em letras maiúsculas ou no formato armazenado no diretório. A configuração padrão é **preserve**. Essa configuração não poderá ser alterada se **casesensitivelookups** for definido como **Sim**.</li></ul> |
| criar um`<name>` | Cria um novo grupo de clientes, dando a ele o nome especificado. |
| listgroups | Exibe os nomes de todos os grupos de clientes. |
| excluir o`<name>` | Remove o grupo de clientes especificado pelo nome. |
| Renomear `<oldname>``<newname>` | Altera o nome do grupo de clientes especificado por *OldName* para *NewName*. |
| addmembers`<hostname>[...]` | Adiciona um *host* ao grupo de clientes especificado pelo *nome*. |
| listmembers`<name>` | Lista os computadores host no grupo de clientes especificado por *nome*. |
| deletemembers`<hostname><groupname>[...]` | Remove o cliente especificado pelo *host* do grupo de clientes especificado pelo *grupo*. |

### <a name="client-for-nfs-related-parameters"></a>Cliente para parâmetros relacionados a NFS

| Parâmetro | Descrição |
| --------- | ----------- |
| start | Inicia o cliente para o serviço NFS. |
| parar | Interrompe o cliente para o serviço NFS. |
| config | Especifica as configurações gerais para o cliente para NFS. Você deve fornecer pelo menos uma das seguintes opções com o argumento de comando **config** :<ul><li>**FileAccess = `<mode>` ** -Especifica o modo de permissão padrão para arquivos criados em servidores NFS (sistema de arquivos de rede). O argumento **Mode** consiste em um número de três dígitos, de 0 a 7 (inclusivo), que representa as permissões padrão concedidas ao usuário, ao grupo e a outros. Os dígitos são convertidos em permissões em estilo UNIX da seguinte maneira: *0 = nenhum*, *1 = x (executar)*, *2 = w (somente gravação)*, *3 = WX (gravação e execução)*, *4 = r (somente leitura*), *5 = RX (leitura e execução)*, *6 = RW (leitura e gravação)* e *7 = rwx (leitura, gravação e execução)*. Por exemplo, o `fileaccess=750` fornece permissões de leitura, gravação e execução para o proprietário, permissões de leitura e execução para o grupo e nenhuma permissão de acesso para outras pessoas.</li><li>**mapsvr = `<server>` ** -Define o servidor como o Mapeamento de Nomes de Usuário Server para o cliente para NFS. Embora essa opção continue a ter suporte para compatibilidade com versões anteriores, você deve usar o utilitário sfuadmin em vez disso.</li><li>**mtype = `{hard|soft}` ** -Especifica o tipo de montagem padrão. Para uma montagem rígida, o cliente para NFS continua tentando novamente um RPC com falha até obter êxito. Para uma montagem reversível, o cliente para NFS retorna falha ao aplicativo de chamada depois de repetir a chamada do número de vezes especificado pela opção de repetição.</li><li>**repetir = `<number>` ** -Especifica o número de vezes para tentar estabelecer uma conexão para uma montagem flexível. Esse valor deve ser de 1 a 10, inclusive. O padrão é **1**.</li><li>**tempo limite `<seconds>` =** -Especifica o número de segundos a aguardar por uma conexão (chamada de procedimento remoto). Esse valor deve ser *0,8*, *0,9*ou um número inteiro de *1 a 60*, inclusive. O padrão é **0,8**.</li><li>**protocolo = `{TCP|UDP|TCP+UDP}` ** -Especifica quais protocolos de transporte o cliente dá suporte. A configuração padrão é **TCP + UDP**.</li><li>**rsize = `<size>` ** -Especifica o tamanho, em kilobytes, do buffer de leitura. Esse valor pode ser *0,5, 1, 2, 4, 8, 16* ou *32*. O padrão é **32**.</li><li>**wSize = `<size>` ** -Especifica o tamanho, em quilobytes, do buffer de gravação. Esse valor pode ser *0,5, 1, 2, 4, 8, 16* ou *32*. O padrão é **32**.</li><li>**perf = default** – restaura as seguintes configurações de desempenho para valores padrão, *mtype*, *Retry*, *Timeout*, *rsize*ou *wSize*. |

### <a name="examples"></a>Exemplos

Para parar o servidor NFS ou o cliente para NFS, digite:

```
nfsadmin server stop
nfsadmin client stop
```

Para iniciar o Server for NFS ou Client for NFS, digite:

```
nfsadmin server start
nfsadmin client start
```

Para definir que o servidor de NFS não diferencia maiúsculas de minúsculas, digite:

```
nfsadmin server config casesensitive=no
```

Para definir que o cliente para NFS não diferencia maiúsculas de minúsculas, digite:

```
nfsadmin client config casesensitive=yes
```

Para exibir todas as opções de NFS ou Client for NFS do servidor atual, digite:

```
nfsadmin server config
nfsadmin client config
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência de cmdlets do NFS](/powershell/module/nfs)
