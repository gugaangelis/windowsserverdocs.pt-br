---
title: Sintaxe do comando netsh, contextos e formatação
description: Você pode usar este tópico para aprender a inserir contextos e subcontextos netsh, compreender a sintaxe do netsh e a formatação de comandos e como executar comandos Netsh em computadores locais e remotos que executam o Windows Server 2016 ou o Windows 10.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 815e59ca00d6450b8ef09a034434c4209c928e78
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405576"
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Sintaxe do comando netsh, contextos e formatação

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para saber como inserir contextos e subcontextos netsh, entender a sintaxe do netsh e a formatação de comandos e como executar comandos Netsh em computadores locais e remotos.

O netsh é um utilitário de script de linha de comando que permite exibir ou modificar a configuração de rede de um computador que está sendo executado no momento. Comandos Netsh podem ser executados digitando comandos no prompt netsh e podem ser usados em arquivos ou scripts em lotes. Os computadores remotos e o computador local podem ser configurados usando comandos netsh.

O Netsh também fornece um recurso de script com o qual você pode executar um grupo de comandos no modo em lote para um computador especificado. Com o Netsh, você pode salvar um script de configuração em um arquivo de texto para fins de arquivamento ou para ajudar você a configurar outros computadores.

## <a name="netsh-contexts"></a>Contextos netsh

O netsh interage com outros componentes do sistema operacional usando a biblioteca dinâmica @ no__t-0link \(DLL @ no__t-2 arquivos. 

Cada DLL auxiliar do netsh fornece um amplo conjunto de recursos chamado de *contexto*, que é um grupo de comandos específicos de uma função de servidor de rede ou recurso. Esses contextos estendem a funcionalidade do netsh fornecendo suporte de configuração e monitoramento para um ou mais serviços, utilitários ou protocolos. Por exemplo, Dhcpmon. dll fornece o netsh com o contexto e o conjunto de comandos necessários para configurar e gerenciar servidores DHCP.

### <a name="obtain-a-list-of-contexts"></a>Obter uma lista de contextos

Você pode obter uma lista de contextos netsh abrindo o prompt de comando ou o Windows PowerShell em um computador que esteja executando o Windows Server 2016 ou o Windows 10. Digite o comando **netsh** e pressione Enter. Digite **/?** e pressione Enter.

Veja a seguir um exemplo de saída para esses comandos em um computador que executa o Windows Server 2016 datacenter.

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

Os contextos netsh podem conter comandos e contextos adicionais, chamados *subcontextos*. Por exemplo, dentro do contexto de roteamento, você pode alterar para os subcontextos IP e IPv6.

Para exibir uma lista de comandos e subcontextos que você pode usar em um contexto, no prompt do netsh, digite o nome do contexto e digite **/?** ou **ajuda**. Por exemplo, para exibir uma lista de subcontextos e comandos que você pode usar no contexto de roteamento, no prompt do netsh \(that is, **netsh @ no__t-2**\), digite um dos seguintes:

**roteamento/?**

**ajuda de roteamento**

Para executar tarefas em outro contexto sem alterar o contexto atual, digite o caminho do contexto do comando que você deseja usar no prompt do netsh. Por exemplo, para adicionar uma interface denominada "conexão de área local" no contexto IGMP sem primeiro alterar para o contexto IGMP, no prompt netsh, digite:

**roteamento IP IGMP adicionar interface "conexão de área local" startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>Executando comandos netsh

Para executar um comando netsh, você deve iniciar o netsh no prompt de comando digitando **netsh** e, em seguida, pressionando ENTER. Em seguida, você pode alterar para o contexto que contém o comando que deseja usar. Os contextos que estão disponíveis dependem dos componentes de rede que você instalou. Por exemplo, se você digitar **DHCP** no prompt netsh e pressionar Enter, o netsh mudará para o contexto do servidor DHCP. No entanto, se você não tiver o DHCP instalado, a seguinte mensagem será exibida:

**O seguinte comando não foi encontrado: DHCP.**

## <a name="formatting-legend"></a>Legenda de formatação

Você pode usar a legenda de formatação a seguir para interpretar e usar a sintaxe correta do comando netsh ao executar o comando no prompt do netsh ou em um script ou arquivo em lotes.

- O texto em *itálico* é uma informação que você deve fornecer enquanto digita o comando. Por exemplo, se um comando tiver um parâmetro chamado-*username*, você deverá digitar o nome de usuário real.
- O texto em **negrito** é a informação que você deve digitar exatamente conforme mostrado enquanto digita o comando.
- O texto seguido por uma elipse \(... \) é um parâmetro que pode ser repetido várias vezes em uma linha de comando.
- O texto entre colchetes [&nbsp;] é um item opcional.
- O texto entre chaves {&nbsp;} com opções separadas por um pipe fornece um conjunto de opções do qual você deve selecionar apenas uma, como `{enable|disable}`.
- O texto formatado com a fonte Courier é a saída do código ou do programa.

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>Executando comandos netsh no prompt de comando ou no Windows PowerShell

Para iniciar o Shell de rede e inserir o netsh no prompt de comando ou no Windows PowerShell, você pode usar o comando a seguir.

### <a name="netsh"></a>netsh

O netsh é um utilitário de script de linha de comando que permite que você, local ou remotamente, exiba ou modifique a configuração de rede de um computador atualmente em execução. Usado sem parâmetros, **netsh** abre o prompt de comando netsh. exe @no__t-qual é, **netsh @ no__t-3**\).

#### <a name="syntax"></a>Sintaxe

**netsh**\[ **-a**&nbsp;*aliasfile*\] \[ **-c**&nbsp;*contexto* 0 1 **-r**3*computador_remoto*5 6 **-u** 8  *Nome_do_domínio @ no__t-20* 1 nome de *usuário* 3 4 **-p**6*senha*8 @ no__t-29 @ No__t-30 1 {*NetshCommand*3 **-f**5*scriptfile*} 7

#### <a name="parameters"></a>Parâmetros

**`-a`**

Opcional. Especifica que você retorna ao prompt **netsh** após a execução de *aliasfile*.

**`AliasFile`**

Opcional. Especifica o nome do arquivo de texto que contém um ou mais comandos **netsh** .

**`-c`**

Opcional. Especifica que o netsh insere o contexto **netsh** especificado.

**`Context`**

Opcional. Especifica o contexto **netsh** que você deseja inserir. 

**`-r`**

Opcional. Especifica que você deseja que o comando seja executado em um computador remoto.

> [!IMPORTANT]
> Quando você usa alguns comandos netsh remotamente em outro computador com o parâmetro **netsh – r** , o serviço de registro remoto deve estar em execução no computador remoto. Se não estiver em execução, o Windows exibirá uma mensagem de erro "caminho de rede não encontrado".

***`RemoteComputer`***

Opcional. Especifica o computador remoto que você deseja configurar.

**`-u`**

Opcional. Especifica que você deseja executar o comando netsh em uma conta de usuário.

***`DomainName\\`***

Opcional. Especifica o domínio em que a conta de usuário está localizada. O padrão é o domínio local se *DomainName @ no__t-1* não for especificado.

***`UserName`***

Opcional. Especifica o nome da conta de usuário.

**`-p`**

Opcional. Especifica que você deseja fornecer uma senha para a conta de usuário.

***`Password`***

Opcional. Especifica a senha para a conta de usuário que você especificou com **-u** *username*.

***`NetshCommand`***

Opcional. Especifica o comando **netsh** que você deseja executar.

**`-f`**

Opcional. Sai do **netsh** depois de executar o script que você designar com o *scriptfile*.

***`ScriptFile`***

Opcional. Especifica o script que você deseja executar.

**`/?`**

Opcional. Exibe a ajuda no prompt do netsh.

> [!NOTE]
> Se você especificar **`-r`** seguido por outro comando, o **netsh** executará o comando no computador remoto e retornará ao prompt de comando cmd. exe. Se você especificar **`-r`** sem outro comando, o **netsh** será aberto no modo remoto. O processo é semelhante ao uso de **set machine** no prompt de comando do netsh. Ao usar **`-r`** , você define o computador de destino somente para a instância atual do **netsh** . Depois de sair e reinserir o **netsh**, o computador de destino será redefinido como o computador local. Você pode executar comandos **netsh** em um computador remoto especificando um nome de computador armazenado no WINS, um nome UNC, um nome de Internet a ser resolvido pelo servidor DNS ou um endereço IP.

**Digitando valores de cadeia de caracteres de parâmetro para comandos netsh**

Em toda a referência de comando netsh, há comandos que contêm parâmetros para os quais um valor de cadeia de caracteres é necessário.

No caso em que um valor de cadeia de caracteres contém espaços entre caracteres, como valores de cadeia de caracteres que consistem em mais de uma palavra, é necessário que você coloque o valor da cadeia entre aspas. Por exemplo, para um parâmetro chamado **interface** com um valor de cadeia de caracteres de **conexão de rede sem fio**, use aspas ao contrário do valor da cadeia de caracteres:

**`interface="Wireless Network Connection"`**

