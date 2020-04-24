---
title: Sintaxe, Contextos e Formatação do Comando Netsh
description: Você pode usar este tópico para aprender a inserir contextos e subcontextos netsh, a entender a formatação da sintaxe e do comando netsh e a executar comandos netsh em computadores locais e remotos que estão executando Windows Server 2016 ou Windows 10.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 061d7252d5a7bbe09d3dca245d9b77ed20a4dedf
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80854759"
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Sintaxe, Contextos e Formatação do Comando Netsh

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para aprender a inserir contextos e subcontextos netsh, a entender a formatação da sintaxe e do comando netsh e a executar comandos netsh em computadores locais e remotos.

O Netsh é um utilitário de script de linha de comando que permite exibir ou modificar a configuração de rede de um computador em execução no momento. Os comandos netsh podem ser executados digitando comandos no prompt netsh e podem ser usados em arquivos ou scripts em lotes. Os computadores remotos e o computador local podem ser configurados usando comandos netsh.

O Netsh também fornece um recurso de script com o qual você pode executar um grupo de comandos no modo em lote para um computador especificado. Com o Netsh, você pode salvar um script de configuração em um arquivo de texto para fins de arquivamento ou para ajudar você a configurar outros computadores.

## <a name="netsh-contexts"></a>Contextos netsh

O netsh interage com outros componentes do sistema operacional usando arquivos \(DLL\) da biblioteca de vínculo\-dinâmico. 

Cada DLL auxiliar do netsh fornece um amplo conjunto de recursos chamado de *contexto*, que é um grupo de comandos específicos a uma função de servidor de rede ou recurso. Esses contextos ampliam a funcionalidade do netsh fornecendo suporte de configuração e monitoramento para um ou mais serviços, utilitários ou protocolos. Por exemplo, Dhcpmon.dll fornece o netsh com o contexto e o conjunto de comandos necessários para configurar e gerenciar servidores DHCP.

### <a name="obtain-a-list-of-contexts"></a>Obter uma lista de contextos

Você pode obter uma lista de contextos netsh abrindo o prompt de comando ou o Windows PowerShell em um computador que executa Windows Server 2016 ou Windows 10. Digite o comando **netsh** e pressione ENTER. Digite **/?** e pressione ENTER.

Veja a seguir um exemplo de saída desses comandos em um computador que executa o Windows Server 2016 Datacenter.

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

Os contextos netsh podem conter comandos e contextos adicionais, chamados *subcontextos*. Por exemplo, dentro do contexto de roteiros, você pode alterar para os subcontextos IP e IPv6.

Para exibir uma lista de comandos e subcontextos que você pode usar em um contexto, no prompt do netsh, digite o nome do contexto e digite **/?** ou **help**. Por exemplo, para exibir uma lista de subcontextos e comandos que podem ser usados no contexto de roteiros, no prompt do netsh \(ou seja, **netsh&gt;** \), digite um dos seguintes:

**routing /?**

**routing help**

Para executar tarefas em outro contexto sem alterar o contexto atual, digite o caminho de contexto do comando que você deseja usar no prompt do netsh. Por exemplo, para adicionar uma interface denominada "Conexão de Área Local" no contexto IGMP sem primeiro alterar para o contexto IGMP, no prompt do netsh, digite:

**routing ip igmp add interface "Local Area Connection" startupqueryinterval=21**

## <a name="running-netsh-commands"></a>Executar comandos netsh

Para executar um comando netsh, você deve iniciar o netsh no prompt de comando digitando **netsh** e pressionando ENTER. Em seguida, você pode mudar para o contexto que contém o comando que deseja usar. Os contextos disponíveis dependem dos componentes de rede instalados. Por exemplo, se você digitar **dhcp** no prompt do netsh e pressionar ENTER, o netsh mudará para o contexto do servidor DHCP. No entanto, se você não tiver o DHCP instalado, a seguinte mensagem será exibida:

**O seguinte comando não foi encontrado: dhcp.**

## <a name="formatting-legend"></a>Legenda de formatação

Você pode usar a legenda de formatação para interpretar e usar a sintaxe de comando netsh correta quando você executa o comando no prompt do netsh ou em um arquivo ou script em lotes.

- O texto em *itálico* é a informação que você deve fornecer enquanto digita o comando. Por exemplo, se um comando tiver um parâmetro denominado -*UserName*, você deverá digitar o nome de usuário real.
- O texto em **negrito** é a informação que você deve digitar exatamente conforme mostrado enquanto você digita o comando.
- O texto seguido por um reticências \(...\) é um parâmetro que pode ser repetido várias vezes em uma linha de comando.
- O texto entre colchetes [&nbsp;] é um item opcional.
- O texto entre chaves {&nbsp;} com opções separadas por um pipe fornece um conjunto de opções do qual você deve selecionar apenas uma, como `{enable|disable}`.
- O texto formatado com a fonte Courier é a saída do código ou do programa.

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>Executar comandos Netsh no prompt de comando ou no Windows PowerShell

Para iniciar o Shell de Rede e inserir o netsh no prompt de comando ou no Windows PowerShell, você pode usar o comando a seguir.

### <a name="netsh"></a>netsh

O Netsh é um utilitário de script de linha de comando que permite, local ou remotamente, exibir ou modificar a configuração de rede de um computador em execução no momento. Usado sem parâmetros, o **netsh** abre o prompt de comando Netsh.exe \(ou seja, **netsh&gt;** \).

#### <a name="syntax"></a>Sintaxe

**netsh**\[ **-a**&nbsp;*AliasFile*\] \[ **-c**&nbsp;*Context* \] \[ **-r**&nbsp;*RemoteComputer*\] \[ **-u** \[ *DomainName\\* \] *UserName* \] \[ **-p**&nbsp;*Password* | \*\] \[{*NetshCommand* |  **-f**&nbsp;*ScriptFile*}\]

##### <a name="parameters"></a>Parâmetros

**`-a`**

Opcional. Especifica que você é levado de volta ao prompt do **netsh** depois de executar *AliasFile*.

**`AliasFile`**

Opcional. Especifica o nome do arquivo de texto que contém um ou mais comandos **netsh**.

**`-c`**

Opcional. Especifica que o netsh insere o contexto do **netsh** especificado.

**`Context`**

Opcional. Especifica o contexto do **netsh** que você deseja inserir. 

**`-r`**

Opcional. Especifica que você deseja que o comando seja executado em um computador remoto.

> [!IMPORTANT]
> Quando você usa alguns comandos netsh remotamente ou outro computador com o parâmetro **netsh –r**, o serviço do Registro Remoto deve ser executado no computador remoto. Se não estiver em execução, o Windows exibirá uma mensagem de erro "Caminho de Rede não encontrado".

***`RemoteComputer`***

Opcional. Especifica o computador remoto que você deseja configurar.

**`-u`**

Opcional. Especifica que você deseja executar o comando netsh em uma conta de usuário.

***`DomainName\\`***

Opcional. Especifica o domínio em que a conta de usuário está localizada. O padrão será o domínio local se *DomainName\\* não estiver especificado.

***`UserName`***

Opcional. Especifica o nome da conta de usuário.

**`-p`**

Opcional. Especifica que você deseja fornecer uma senha para a conta de usuário.

***`Password`***

Opcional. Especifica a senha da conta de usuário que você especificou com **-u** *UserName*.

***`NetshCommand`***

Opcional. Especifica o comando **netsh** que você deseja executar.

**`-f`**

Opcional. Sai do **netsh** depois de executar o script que você designa com *ScriptFile*.

***`ScriptFile`***

Opcional. Especifica o script que você deseja executar.

**`/?`**

Opcional. Exibe a ajuda no prompt do netsh.

> [!NOTE]
> Se você especificar **`-r`** seguido por outro comando, **netsh** executará o comando no computador remoto e voltará para o prompt de comando Cmd.exe. Se você especificar **`-r`** sem outro comando, o **netsh** será aberto no modo remoto. O processo é semelhante ao uso de **set machine** no prompt de comando Netsh. Quando você usa **`-r`** , você define o computador de destino da instância atual do **netsh** somente. Depois de sair e entrar novamente no **netsh**, o computador de destino será redefinido como o computador local. Você pode executar comandos **netsh** em um computador remoto especificando um nome do computador armazenado no WINS, um nome UNC, um nome de Internet a ser resolvido pelo servidor DNS ou um endereço IP.

**Digitar valores de cadeia de caracteres de parâmetro para comandos netsh**

Em toda a referência do comando Netsh, há comandos que contêm parâmetros para os quais um valor de cadeia de caracteres é necessário.

Caso um valor de cadeia de caracteres contenha espaços entre caracteres, como valores de cadeia de caracteres compostos por mais de uma palavra, é necessário colocar o valor da cadeia de caracteres entre aspas. Por exemplo, para um parâmetro denominado **interface** com um valor de cadeia de caracteres de **Conexão de Rede Sem Fio**, use o valor de cadeia de caracteres entre aspas:

**`interface="Wireless Network Connection"`**

