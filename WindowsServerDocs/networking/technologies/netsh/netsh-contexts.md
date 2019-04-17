---
title: Sintaxe do comando netsh, contextos e formatação
description: Você pode usar este tópico para saber como inserir subcontextos e contextos netsh, entender netsh sintaxe e a formatação de comando e como executar comandos netsh em computadores locais e remotos que executam o Windows Server 2016 ou Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c7c16674b831431ff5bb1d0172cc2b2a939008f6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Sintaxe do comando netsh, contextos e formatação

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como inserir subcontextos e contextos netsh, entender netsh sintaxe e a formatação de comando e como executar comandos netsh em computadores locais e remotos.

Netsh é um utilitário de script de linha de comando que permite que você exibir ou modificar a configuração de rede de um computador que está sendo executado. Comandos do Netsh podem ser executados ao digitar comandos no prompt do netsh e eles podem ser usados em arquivos de lote ou scripts. Computadores remotos e o computador local podem ser configurados usando comandos netsh.

Netsh também fornece um recurso de script que permite executar um grupo de comandos no modo de lotes em um computador especificado. Com netsh, você pode salvar um script de configuração em um arquivo de texto para fins de arquivamento ou para ajudá-lo a configurar outros computadores.

## <a name="netsh-contexts"></a>Contextos Netsh

Netsh interage com outros componentes do sistema operacional usando arquivos de \(DLL\) dynamic\ link biblioteca. 

Cada DLL auxiliar do netsh fornece um extenso conjunto de recursos chamado um *contexto*, que é um grupo de comandos específicos a uma função de servidor de rede ou recurso. Esses contextos estendem a funcionalidade do netsh fornecendo configuração e monitoramento de suporte para uma ou mais serviços, utilitários ou protocolos. Por exemplo, Dhcpmon.dll fornece netsh com o contexto e o conjunto de comandos necessários para configurar e gerenciar servidores DHCP.

### <a name="obtain-a-list-of-contexts"></a>Obter uma lista de contextos

Você pode obter uma lista de contextos netsh abrindo o prompt de comando ou do Windows PowerShell em um computador executando o Windows Server 2016 ou Windows 10. Digite o comando **netsh** e pressione ENTER. Tipo **/? **, e pressione ENTER.

A seguir está um exemplo de saída para que esses comandos em um computador executando o Windows Server 2016 Datacenter.

    PS C:\Windows\system32> netsh
    netsh>/?
    
    The following commands are available:
    
    Commands in this context:
    ..            - Goes up one context level.
    ?             - Displays a list of commands.
    abort         - Discards changes made while in offline mode.
    add           - Adds a configuration entry to a list of entries.
    advfirewall   - Changes to the `netsh advfirewall' context.
    alias         - Adds an alias.
    branchcache   - Changes to the `netsh branchcache' context.
    bridge        - Changes to the `netsh bridge' context.
    bye           - Exits the program.
    commit        - Commits changes made while in offline mode.
    delete        - Deletes a configuration entry from a list of entries.
    dhcpclient    - Changes to the `netsh dhcpclient' context.
    dnsclient     - Changes to the `netsh dnsclient' context.
    dump          - Displays a configuration script.
    exec          - Runs a script file.
    exit          - Exits the program.
    firewall      - Changes to the `netsh firewall' context.
    help          - Displays a list of commands.
    http          - Changes to the `netsh http' context.
    interface     - Changes to the `netsh interface' context.
    ipsec         - Changes to the `netsh ipsec' context.
    ipsecdosprotection - Changes to the `netsh ipsecdosprotection' context.
    lan           - Changes to the `netsh lan' context.
    namespace     - Changes to the `netsh namespace' context.
    netio         - Changes to the `netsh netio' context.
    offline       - Sets the current mode to offline.
    online        - Sets the current mode to online.
    popd          - Pops a context from the stack.
    pushd         - Pushes current context on stack.
    quit          - Exits the program.
    ras           - Changes to the `netsh ras' context.
    rpc           - Changes to the `netsh rpc' context.
    set           - Updates configuration settings.
    show          - Displays information.
    trace         - Changes to the `netsh trace' context.
    unalias       - Deletes an alias.
    wfp           - Changes to the `netsh wfp' context.
    winhttp       - Changes to the `netsh winhttp' context.
    winsock       - Changes to the `netsh winsock' context.
    
    The following sub-contexts are available:
     advfirewall branchcache bridge dhcpclient dnsclient firewall http interface ipsec ipsecdosprotection lan namespace netio ras rpc trace wfp winhttp winsock
    
    To view help for a command, type the command, followed by a space, and then
     type ?.


### <a name="subcontexts"></a>Subcontextos

Contextos Netsh podem conter comandos e contextos adicionais, chamados *subcontextos*. Por exemplo, dentro do contexto de roteamento, você pode alterar os subcontextos IP e IPv6.

Para exibir uma lista de comandos e subcontextos que você pode usar em um contexto, no prompt do netsh, digite o nome do contexto e, em seguida, digite **/?** Ou **ajudar **. Por exemplo, para exibir uma lista de subcontextos e comandos que você pode usar o contexto de roteamento, no prompt do netsh \ (ou seja, **netsh&gt;**\), digite um destes procedimentos:

**Roteamento /?**

**Roteamento ajuda**

Para executar tarefas em outro contexto sem mudar de seu contexto atual, digite o caminho do contexto do comando que você deseja usar no prompt do netsh. Por exemplo, para adicionar uma interface denominada "Conexão de área Local" no contexto IGMP sem mudar primeiro ao contexto IGMP, no prompt do netsh, digite:

**Roteamento ip igmp adicionar interface "Conexão de área Local" startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>Executando comandos netsh

Para executar um comando netsh, você deve iniciar netsh no prompt de comando digitando **netsh** e pressione ENTER. Em seguida, você pode alterar o contexto que contém o comando que você deseja usar. Os contextos disponíveis dependem os componentes de rede que você instalou. Por exemplo, se você digitar **dhcp** no prompt do netsh e pressione ENTER, netsh altera para o contexto de servidor DHCP. Se você não tiver DHCP instalado, no entanto, a seguinte mensagem aparece:

**O comando a seguir não foi encontrado: dhcp.**

## <a name="formatting-legend"></a>Formatação de legenda

Você pode usar a formatação legenda a seguir para interpretar e usar a sintaxe de comando netsh correta quando você executar o comando no prompt do netsh ou em um arquivo em lotes ou script.

- Texto em *itálico* é uma informação que você deve fornecer enquanto você digita o comando. Por exemplo, se um comando tem um parâmetro chamado -*UserName*, você deve digitar o nome de usuário real.
- Texto em **Bold** informações que você deve digitar exatamente conforme mostrado enquanto você digita o comando.
- Texto, seguido por um sinal de reticências \(...\) é um parâmetro que pode ser repetido várias vezes em uma linha de comando.
- Texto que está entre colchetes [&nbsp;] é um item opcional.
- Texto que está entre chaves {&nbsp;} com opções separadas por um pipe fornece um conjunto de opções do qual você deve selecionar apenas um, como `{enable|disable}`.
- Texto formatado com a fonte Courier é código ou programa de saída.

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>Executando comandos Netsh do prompt de comando ou do Windows PowerShell

Para iniciar o Shell de rede e digite netsh no prompt de comando ou no Windows PowerShell, você pode usar o comando a seguir.

### <a name="netsh"></a>netsh

Netsh é um utilitário de script de linha de comando que permite que você, local ou remotamente, exibir ou modificar a configuração de rede de um computador em execução no momento. Usado sem parâmetros, **netsh** abre o prompt de comando Netsh.exe \ (ou seja, **netsh&gt;**\).

#### <a name="syntax"></a>Sintaxe

**netsh**\[ **-a**&nbsp;*AliasFile*\] \[ **-c**&nbsp;*Context* \] \[**-r**&nbsp;*RemoteComputer*\] \[ **-u** \[ *DomainName\\* \] *UserName* \] \[ **-p**&nbsp;*Password* | \*\] \[{*NetshCommand* | **-f**&nbsp;*ScriptFile*}\]

#### <a name="parameters"></a>Parâmetros

**`-a`**

Opcional. Especifica que você é retornado para o **netsh** prompt após a execução *Arquivo_de_alias *.

**`AliasFile`**

Opcional. Especifica o nome do arquivo de texto que contém um ou mais **netsh** comandos.

**`-c`**

Opcional. Especifica que netsh insere o especificado **netsh** contexto.

**`Context`**

Opcional. Especifica o **netsh** contexto que você deseja inserir. 

**`-r`**

Opcional. Especifica que você deseja que o comando a ser executado em um computador remoto.

>[!IMPORTANT]
>Quando você usa alguns comandos netsh remotamente em outro computador com o **netsh – r** parâmetro, o serviço Registro remoto deve ser executado no computador remoto. Se não estiver em execução, o Windows exibirá uma mensagem de erro "Caminho de rede não encontrado".

***`RemoteComputer`***

Opcional. Especifica o computador remoto que você deseja configurar.

**`-u`**

Opcional. Especifica que você deseja executar o comando netsh em uma conta de usuário.

***`DomainName\\`***

Opcional. Especifica o domínio em que a conta de usuário está localizada. O padrão é o domínio local se *DomainName\\* não for especificado.

***`UserName`***

Opcional. Especifica o nome da conta de usuário.

**`-p`**

Opcional. Especifica que você deseja fornecer uma senha da conta de usuário.

***`Password`***

Opcional. Especifica a senha da conta de usuário que você especificou com **-u***UserName *.

***`NetshCommand`***

Opcional. Especifica o **netsh** comando que você deseja executar.

**`-f`**

Opcional. Sai **netsh** depois de executar o script que você designar com *arquivo_de_script *.

***`ScriptFile`***

Opcional. Especifica o script que você deseja executar.

**`/?`**

Opcional. Exibe a Ajuda no prompt do netsh.

>[!NOTE]
>Se você especificar **`-r`**seguido por outro comando, **netsh** executa o comando no computador remoto e, em seguida, retorna ao prompt de comando Cmd.exe. Se você especificar **`-r`**sem outro comando, **netsh** é aberto no modo remoto. O processo é semelhante ao uso **conjunto máquina** no prompt de comando Netsh. Quando você usa **`-r`**, defina o computador de destino para a instância atual da **netsh** somente. Depois que você sair e digitar novamente **netsh**, o computador de destino será redefinido como o computador local. Você pode executar **netsh** comandos em um computador remoto, especificando um computador nomear armazenado no WINS, um nome UNC, um nome de Internet a ser resolvido pelo servidor DNS, ou um endereço IP.

**Valores de cadeia de caracteres de parâmetro digitação para comandos netsh**

Em toda a referência de comando Netsh existem comandos que contêm os parâmetros para o qual um valor de cadeia de caracteres é necessário.

No caso em que um valor de cadeia de caracteres contém espaços entre caracteres, como valores de cadeia de caracteres que consistem em mais de uma palavra, é necessário que você coloque o valor de cadeia de caracteres entre aspas. Por exemplo, para um parâmetro chamado **interface** com um valor de cadeia de caracteres de **Conexão de rede sem fio**, use o valor de cadeia de caracteres entre aspas:

**`interface="Wireless Network Connection"`**

