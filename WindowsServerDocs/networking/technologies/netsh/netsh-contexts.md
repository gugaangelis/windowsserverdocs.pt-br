---
title: Sintaxe do comando netsh, contextos e formatação
description: Você pode usar este tópico para saber como inserir subcontextos e contextos de netsh, compreender a sintaxe de netsh e formatação de comando e como executar comandos do netsh em computadores locais e remotos que executam o Windows Server 2016 ou Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: adb1546bc21b3209a362fd61feab0d3ee6810a66
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812163"
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Sintaxe do comando netsh, contextos e formatação

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber como inserir subcontextos e contextos de netsh, compreender a sintaxe de netsh e formatação de comando e como executar comandos do netsh em computadores locais e remotos.

O Netsh é um utilitário de linha de comando de script que lhe permite exibir ou modificar a configuração de rede de um computador que está sendo executado. Comandos do Netsh podem ser executados ao digitar comandos no prompt do netsh e eles podem ser usados em arquivos em lotes ou scripts. Computadores remotos e o computador local podem ser configurados usando comandos netsh.

O Netsh também fornece um recurso de script com o qual você pode executar um grupo de comandos no modo em lote para um computador especificado. Com o Netsh, você pode salvar um script de configuração em um arquivo de texto para fins de arquivamento ou para ajudar você a configurar outros computadores.

## <a name="netsh-contexts"></a>Contextos de netsh

Netsh interage com outros componentes do sistema operacional usando dynamic\-biblioteca de vínculo \(DLL\) arquivos. 

Cada DLL auxiliar do netsh fornece um amplo conjunto de recursos chamado um *contexto*, que é um grupo de comandos específicos para uma função de servidor de rede ou um recurso. Esses contextos estendem a funcionalidade do netsh, fornecendo a configuração e monitoramento suporte para um ou mais serviços, utilitários ou protocolos. Por exemplo, Dhcpmon fornece netsh com o contexto e o conjunto de comandos necessários para configurar e gerenciar servidores DHCP.

### <a name="obtain-a-list-of-contexts"></a>Obter uma lista de contextos

Você pode obter uma lista de contextos de netsh, abrindo o prompt de comando ou do Windows PowerShell em um computador executando o Windows Server 2016 ou Windows 10. Digite o comando **netsh** e pressione ENTER. Tipo de **/?** , e pressione ENTER.

A seguir está um exemplo de saída para que esses comandos em um computador executando o Windows Server 2016 Datacenter.

>    ```
>   PS C:\Windows\system32> netsh
>   netsh>/?
>    
>    The following commands are available:
>    
>    Commands in this context:
>    ..            - Goes up one context level.
>    ?             - Displays a list of commands.
>    abort         - Discards changes made while in offline mode.
>    add           - Adds a configuration entry to a list of entries.
>    advfirewall   - Changes to the `netsh advfirewall' context.
>    alias         - Adds an alias.
>    branchcache   - Changes to the `netsh branchcache' context.
>    bridge        - Changes to the `netsh bridge' context.
>    bye           - Exits the program.
>    commit        - Commits changes made while in offline mode.
>    delete        - Deletes a configuration entry from a list of entries.
>    dhcpclient    - Changes to the `netsh dhcpclient' context.
>    dnsclient     - Changes to the `netsh dnsclient' context.
>    dump          - Displays a configuration script.
>    exec          - Runs a script file.
>    exit          - Exits the program.
>    firewall      - Changes to the `netsh firewall' context.
>    help          - Displays a list of commands.
>    http          - Changes to the `netsh http' context.
>    interface     - Changes to the `netsh interface' context.
>    ipsec         - Changes to the `netsh ipsec' context.
>    ipsecdosprotection - Changes to the `netsh ipsecdosprotection' context.
>    lan           - Changes to the `netsh lan' context.
>    namespace     - Changes to the `netsh namespace' context.
>    netio         - Changes to the `netsh netio' context.
>    offline       - Sets the current mode to offline.
>    online        - Sets the current mode to online.
>    popd          - Pops a context from the stack.
>    pushd         - Pushes current context on stack.
>    quit          - Exits the program.
>    ras           - Changes to the `netsh ras' context.
>    rpc           - Changes to the `netsh rpc' context.
>    set           - Updates configuration settings.
>    show          - Displays information.
>    trace         - Changes to the `netsh trace' context.
>    unalias       - Deletes an alias.
>    wfp           - Changes to the `netsh wfp' context.
>    winhttp       - Changes to the `netsh winhttp' context.
>    winsock       - Changes to the `netsh winsock' context.
>    
>    The following sub-contexts are available:
>     advfirewall branchcache bridge dhcpclient dnsclient firewall http interface ipsec ipsecdosprotection lan namespace netio ras rpc trace wfp winhttp winsock
>    
>    To view help for a command, type the command, followed by a space, and then type ?.
>    ```

### <a name="subcontexts"></a>Subcontextos

Contextos de Netsh podem conter comandos e contextos adicionais, chamados *subcontextos*. Por exemplo, dentro do contexto de roteamento, você pode alterar os subcontextos IP e IPv6.

Para exibir uma lista de comandos e subcontextos que podem ser usados dentro de um contexto, no prompt do netsh, digite o nome do contexto e, em seguida, digite um **/?** ou **ajudar**. Por exemplo, para exibir uma lista de subcontextos e comandos que você pode usar o contexto de roteamento, no prompt do netsh \(ou seja, **netsh&gt;** \), digite o seguinte:

**roteamento /?**

**Ajuda de roteamento**

Para executar tarefas em outro contexto sem alterar o seu contexto atual, digite o caminho de contexto do comando que você deseja usar no prompt do netsh. Por exemplo, para adicionar uma interface chamada "Conexão de área Local" no contexto IGMP sem alterar o primeiro para o contexto IGMP, no prompt do netsh, digite:

**roteamento ip igmp adicionar a interface "Conexão de área Local" startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>Executando comandos netsh

Para executar um comando netsh, você deve iniciar o netsh do prompt de comando, digitando **netsh** e, em seguida, pressionando ENTER. Em seguida, você pode alterar o contexto que contém o comando que você deseja usar. Os contextos disponíveis dependem dos componentes de rede que você instalou. Por exemplo, se você digitar **dhcp** no prompt do netsh e pressione ENTER, netsh altera para o contexto do servidor DHCP. Se você não tiver o DHCP foi instalado, no entanto, a seguinte mensagem será exibida:

**O comando a seguir não foi encontrado: dhcp.**

## <a name="formatting-legend"></a>Formatando a legenda

Você pode usar a seguinte legenda de formatação para interpretar e usar a sintaxe do comando netsh correta quando você executa o comando no prompt do netsh ou em um arquivo em lotes ou script.

- Texto em *itálico* são informações que você deve fornecer enquanto você digita o comando. Por exemplo, se um comando tem um parâmetro chamado -*nome de usuário*, você deve digitar o nome de usuário real.
- Texto em **negrito** são informações que você deve digitar exatamente como mostrados enquanto você digita o comando.
- Texto seguido por reticências \(... \) é um parâmetro que pode ser repetido várias vezes em uma linha de comando.
- Texto entre colchetes [&nbsp;] é um item opcional.
- Texto que está entre chaves {&nbsp;} com opções separadas por uma barra vertical fornece um conjunto de opções, na qual você deve selecionar apenas um, tais como `{enable|disable}`.
- Texto formatado com a fonte de correio é a saída de programa ou código.

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>Executando comandos Netsh do prompt de comando ou do Windows PowerShell

Para iniciar o Shell de rede e digite netsh no prompt de comando ou no Windows PowerShell, você pode usar o comando a seguir.

### <a name="netsh"></a>netsh

O Netsh é um utilitário de linha de comando de script que permite que você, localmente ou remotamente, exibir ou modificar a configuração de rede de um computador em execução no momento. Usado sem parâmetros, **netsh** é aberto o prompt de comando Netsh.exe \(, ou seja, **netsh&gt;** \).

#### <a name="syntax"></a>Sintaxe

**Netsh** \[ **- um**&nbsp;*Arquivo_de_alias* \] \[ **- c** &nbsp;  *Contexto* \] \[ **- r**&nbsp;*RemoteComputer* \] \[ **- u** \[ *DomainName\\*  \] *nome de usuário* \] \[ **-p** &nbsp; *Senha*  |  \* \] \[{*ComandoNetsh* |  **-f** &nbsp; *ScriptFile*}\]

#### <a name="parameters"></a>Parâmetros

**`-a`**

Opcional. Especifica que você retorna para o **netsh** prompt após a execução *Arquivo_de_alias*.

**`AliasFile`**

Opcional. Especifica o nome do arquivo de texto que contém um ou mais **netsh** comandos.

**`-c`**

Opcional. Especifica que netsh insere especificado **netsh** contexto.

**`Context`**

Opcional. Especifica o **netsh** contexto que você deseja inserir. 

**`-r`**

Opcional. Especifica que você deseja que o comando a ser executado em um computador remoto.

> [!IMPORTANT]
> Quando você usa alguns comandos do netsh remotamente em outro computador com o **netsh – r** parâmetro, o serviço Registro remoto deve estar em execução no computador remoto. Se não estiver em execução, o Windows exibe uma mensagem de erro "Caminho de rede não encontrado".

***`RemoteComputer`***

Opcional. Especifica o computador remoto que você deseja configurar.

**`-u`**

Opcional. Especifica que você deseja executar o comando netsh sob uma conta de usuário.

***`DomainName\\`***

Opcional. Especifica o domínio onde se encontra a conta de usuário. O padrão é o domínio local, se *nome_do_domínio\\*  não for especificado.

***`UserName`***

Opcional. Especifica o nome da conta de usuário.

**`-p`**

Opcional. Especifica que você deseja fornecer uma senha para a conta de usuário.

***`Password`***

Opcional. Especifica a senha da conta de usuário que você especificou com **-u** *nome de usuário*.

***`NetshCommand`***

Opcional. Especifica o **netsh** comando que você deseja executar.

**`-f`**

Opcional. Sai **netsh** depois de executar o script que você designe com *ScriptFile*.

***`ScriptFile`***

Opcional. Especifica o script que você deseja executar.

**`/?`**

Opcional. Exibe a Ajuda no prompt do netsh.

> [!NOTE]
> Se você especificar **`-r`** seguido de outro comando, **netsh** executa o comando no computador remoto e, em seguida, retorna ao prompt de comando Cmd.exe. Se você especificar **`-r`** sem outro comando, **netsh** é aberto no modo remoto. O processo é semelhante a usar **machine conjunto** no prompt de comando Netsh. Quando você usa **`-r`** , defina o computador de destino para a instância atual do **netsh** apenas. Depois de sair e retornar **netsh**, o computador de destino será redefinido como o computador local. Você pode executar **netsh** comandos em um computador remoto, especificando um computador nome armazenado no WINS, um nome UNC, um nome a ser resolvido pelo servidor DNS ou um endereço IP da Internet.

**Digitando os valores de cadeia de caracteres de parâmetros para os comandos netsh**

Em toda a referência de comandos do Netsh, há comandos que contêm parâmetros para os quais é necessário um valor de cadeia de caracteres.

No caso em que um valor de cadeia de caracteres contém espaços entre os caracteres, como valores de cadeia de caracteres que consistem em mais de uma palavra, é necessário que você coloque o valor de cadeia de caracteres entre aspas. Por exemplo, para um parâmetro denominado **interface** com um valor de cadeia de caracteres de **Conexão de rede sem fio**, use aspas ao redor do valor de cadeia de caracteres:

**`interface="Wireless Network Connection"`**

