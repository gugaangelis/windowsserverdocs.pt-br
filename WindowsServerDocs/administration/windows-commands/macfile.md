---
title: macfile
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e2ce586c-b316-41d3-90f8-4be0d074cc0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 194d1adaaf64ffee2a217982638ddf0661dd0369
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374172"
---
# <a name="macfile"></a>macfile

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gerencia o servidor de arquivos para servidores Macintosh, volumes, diretórios e arquivos. Você pode automatizar tarefas administrativas incluindo uma série de comandos em arquivos em lotes e iniciá-los manualmente ou em horários predeterminados. 
-   [Para modificar diretórios em volumes acessíveis ao Macintosh](#BKMK_Moddirs)
-   [Para unir os dados de um arquivo do Macintosh e as bifurcações de recursos](#BKMK_Joinforks)
-   [Para alterar a mensagem de logon e limitar as sessões](#BKMK_LogonLimit)
-   [Para adicionar, alterar ou remover volumes acessíveis ao Macintosh](#BKMK_addvol)

## <a name="BKMK_Moddirs"></a>Para modificar diretórios em volumes acessíveis ao Macintosh

### <a name="syntax"></a>Sintaxe
```
macfile directory[/server:\\<computerName>] /path:<directory> [/owner:<OwnerName>] [/group:<GroupName>] [/permissions:<Permissions>]
```

### <a name="parameters"></a>Parâmetros
-   /Server: \\ @ no__t-1 @ no__t-2 Especifica o servidor no qual alterar um diretório. Se omitido, a operação será executada no computador local.
-   /Path: <directory> Obrigatório. Especifica o caminho para o diretório que você deseja alterar. O diretório deve existir. o **diretório MacFile** não cria diretórios.
-   /Owner: <OwnerName> altera o proprietário do diretório. Se omitido, o proprietário permanecerá inalterado.
-   /Group: <GroupName> Especifica ou altera o grupo primário do Macintosh que está associado ao diretório. Se omitido, o grupo primário permanecerá inalterado.
-   /Permissions: <Permissions> Define permissões no diretório para o proprietário, o grupo primário e o mundo (todos). Um número de 11 dígitos é usado para definir permissões. O número 1 concede permissão e 0 revoga a permissão (por exemplo, 11111011000). Se omitido, as permissões permanecerão inalteradas.
    A posição do dígito determina qual permissão está definida, conforme descrito na tabela a seguir.

    |Posição|Define a permissão para|
    |------|------------|
    |First|OwnerSeeFiles|
    |Segundo|OwnerSeeFolders|
    |Terceiro|OwnerMakechanges|
    |Quarto|GroupSeeFiles|
    |Primeira|GroupSeeFolders|
    |Seis|GroupMakechanges|
    |Sétima|WorldSeeFiles|
    |1/8|WorldSeeFolders|
    |Nono|WorldMakechanges|
    |Décima|O diretório não pode ser renomeado, movido ou excluído.|
    |Décima primeira coleta|As alterações se aplicam ao diretório atual e a todos os subdiretórios.|

-   /?
    Exibe a ajuda no prompt de comando.

### <a name="remarks"></a>Comentários
- Se as informações fornecidas contiverem espaços ou caracteres especiais, use aspas ao contrário do texto (por exemplo, **"** <em>nome do computador</em> **"** ).
- Use **macfiledirectory** para tornar um diretório existente em um volume acessível para Macintosh disponível para usuários do Macintosh. O comando **macfiledirectory** não cria diretórios. Use o Gerenciador de arquivos, o prompt de comando ou o comando de **nova pasta do Macintosh** para criar um diretório em um volume acessível por Macintosh antes de usar o comando **Macfile directory** .
  ### <a name="BKMK_Examples"></a>Disso
  O exemplo a seguir altera as permissões do subdiretório pode ser vendas, nas estatísticas de volume acessíveis ao Macintosh, na unidade E do servidor local. O exemplo atribui a permissão ver arquivos, ver pastas e fazer alterações ao proprietário e ver os arquivos e ver as permissões de pastas para todos os outros usuários, ao mesmo tempo em que impede que o diretório seja renomeado, movido ou excluído.
  ```
  macfile directory /path:"e:\statistics\may sales" /permissions:11111011000
  ```

## <a name="BKMK_Joinforks"></a>Para unir os dados de um arquivo do Macintosh e as bifurcações de recursos

### <a name="syntax"></a>Sintaxe
```
macfile forkize[/server:\\<computerName>] [/creator:<CreatorName>] [/type:<typeName>]  [/datafork:<Filepath>] [/resourcefork:<Filepath>] /targetfile:<Filepath>
```

### <a name="parameters"></a>Parâmetros

|         Parâmetro          |                                                                                                           Descrição                                                                                                            |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Server: \\ @ no__t-1 @ no__t-2 |                                                            Especifica o servidor no qual os arquivos são agrupados. Se omitido, a operação será executada no computador local.                                                            |
|   /Creator: <CreatorName>   |                                      Especifica o criador do arquivo. O localizador de Macintosh usa a opção de linha de comando **/Creator** para determinar o aplicativo que criou o arquivo.                                       |
|      /Type: <typeName>      |                                 Especifica o tipo de arquivo. O localizador de Macintosh usa a opção de linha de comando **/Type** para determinar o tipo de arquivo dentro do aplicativo que criou o arquivo.                                 |
|    /datafork: <Filepath>    |                                                                   Especifica o local da bifurcação de dados que deve ser unida. Você pode especificar um caminho remoto.                                                                   |
|  /resourcefork: <Filepath>  |                                                                 Especifica o local da bifurcação de recursos que deve ser unida. Você pode especificar um caminho remoto.                                                                 |
|   /TargetFile: <Filepath>   | Obrigatório. Especifica o local do arquivo que é criado unindo-se uma bifurcação de dados e uma bifurcação de recursos, ou especifica o local do arquivo cujo tipo ou criador você está alterando. O arquivo deve estar no servidor especificado. |
|             /?             |                                                                                               Exibe a ajuda no prompt de comando.                                                                                               |

### <a name="remarks"></a>Comentários
- Se as informações fornecidas contiverem espaços ou caracteres especiais, use aspas ao contrário do texto (por exemplo, **"** <em>nome do computador</em> **"** ).

### <a name="examples"></a>Exemplos
Para criar o arquivo treeapp no volume acessível para Macintosh D:\Release, usando o C:\Cross\Mac\Appcode de bifurcação de recursos e para tornar esse novo arquivo exibido para clientes Macintosh como um aplicativo (aplicativos Macintosh usam o tipo APL) com o criador (assinatura ) definido como MAGNOLIA, digite:
```
macfile forkize /resourcefork:c:\cross\mac\appcode /type:APPL /creator:MAGNOLIA /targetfile:D:\Release\treeapp
```
Para alterar o criador do arquivo para o Microsoft Word 5,1, para o arquivo WOrd. txt no diretório D:\Word documents\Group files, no servidor \\ \ ServerName, digite:
```
macfile forkize /server:\\servera /creator:MSWD /type:TEXT /targetfile:"d:\Word documents\Group files\Word.txt"
```

## <a name="BKMK_LogonLimit"></a>Para alterar a mensagem de logon e limitar as sessões
### <a name="syntax"></a>Sintaxe
```
macfile server [/server:\\<computerName>] [/maxsessions:{Number | unlimited}] [/loginmessage:<Message>]
```

### <a name="parameters"></a>Parâmetros

|               Parâmetro                |                                                                                                                                                                           Descrição                                                                                                                                                                            |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /Server: \\ @ no__t-1 @ no__t-2       |                                                                                                                        Especifica o servidor no qual os parâmetros são alterados. Se omitido, a operação será executada no computador local.                                                                                                                         |
| /maxsessions: {número &#124; ilimitado} |                                                                                         Especifica o número máximo de usuários que podem usar simultaneamente os servidores de arquivos e de impressão para Macintosh. Se omitido, a configuração **maxsessions** para o servidor permanecerá inalterada.                                                                                         |
|        /loginmessage: <Message>         | altera a mensagem que os usuários do Macintosh veem ao fazer logon no servidor de arquivos para Macintosh Server. O número máximo de caracteres para a mensagem de logon é 199. Se omitido, a mensagem **loginmessage** para o servidor permanecerá inalterada. Para remover uma mensagem de logon existente, inclua o parâmetro **/loginmessage** , mas deixe a variável de *mensagem* em branco. |
|                   /?                   |                                                                                                                                                               Exibe a ajuda no prompt de comando.                                                                                                                                                               |

### <a name="remarks"></a>Comentários
- Se as informações fornecidas contiverem espaços ou caracteres especiais, use aspas ao contrário do texto (por exemplo, **"** <em>nome do computador</em> **"** ).

### <a name="examples"></a>Exemplos
Para alterar o número de servidores de arquivos e de impressão para as sessões do Macintosh que são permitidas no servidor local da configuração atual para cinco sessões e para adicionar a mensagem de logon "fazer logoff do servidor para Macintosh quando terminar"., digite:
```
macfile server /maxsessions:5 /loginmessage:"Log off from Server for Macintosh when you are finished."
```

## <a name="BKMK_addvol"></a>Para adicionar, alterar ou remover volumes acessíveis ao Macintosh
### <a name="syntax"></a>Sintaxe
```
macfile volume {/add|/set} [/server:\\<computerName>] /name:<volumeName>/path:<directory>[/readonly:{true | false}] [/guestsallowed:{true | false}] [/password:<Password>] [/maxusers:{<Number>>|unlimited}]
macfile volume /remove[/server:\\<computerName>] /name:<volumeName>
```

### <a name="parameters"></a>Parâmetros

|              Parâmetro               |                                                                                                                                                                       Descrição                                                                                                                                                                        |
|--------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          {/Add &#124; /Set}          |                                                                                                                      Necessário quando você está adicionando ou alterando um volume acessível para Macintosh. Adiciona ou altera o volume especificado.                                                                                                                       |
|      /Server: \\ @ no__t-1 @ no__t-2      |                                                                                                             Especifica o servidor no qual adicionar, alterar ou remover um volume. Se omitido, a operação será executada no computador local.                                                                                                              |
|          /Name: <volumeName>          |                                                                                                                                          Obrigatório. Especifica o nome do volume a ser adicionado, alterado ou removido.                                                                                                                                           |
|          /Path: <directory>           |                                                                                                                Obrigatório e válido somente quando você está adicionando um volume. Especifica o caminho para o diretório raiz do volume a ser adicionado.                                                                                                                 |
|    /ReadOnly: {true &#124; false}     | Especifica se os usuários podem alterar arquivos no volume. Digite true para especificar que os usuários não podem alterar arquivos no volume. Digite false para especificar que os usuários podem alterar arquivos no volume. Se omitido ao adicionar um volume, as alterações nos arquivos são permitidas. Se omitido ao alterar um volume, a configuração **ReadOnly** para o volume permanece inalterada. |
|  /guestsallowed: {true &#124; false}  |      Especifica se os usuários que fazem logon como convidados podem usar o volume. Digite true para especificar que os convidados podem usar o volume. Digite false para especificar que convidados não podem usar o volume. Se omitido ao adicionar um volume, os convidados poderão usar o volume. Se omitido ao alterar um volume, a configuração **guestsallowed** para o volume permanece inalterada.       |
|         /Password: <Password>         |                                                                               Especifica uma senha que será necessária para acessar o volume. Se omitido ao adicionar um volume, nenhuma senha é criada. Se omitido ao alterar um volume, a senha permanece inalterada.                                                                               |
| /maxusers: {<Number> >&#124;ilimitado} |                                                 Especifica o número máximo de usuários que podem usar simultaneamente os arquivos no volume. Se omitido ao adicionar um volume, um número ilimitado de usuários poderá usar o volume. Se omitido ao alterar um volume, o valor de **maxusers** permanece inalterado.                                                 |
|               /remove                |                                                                                                                                Necessário quando você está removendo um volume Macintosh-acessível. Remove o volume especificado.                                                                                                                                |
|                  /?                  |                                                                                                                                                           Exibe a ajuda no prompt de comando.                                                                                                                                                           |

### <a name="remarks"></a>Comentários
- Se as informações fornecidas contiverem espaços ou caracteres especiais, use aspas ao contrário do texto (por exemplo, **"** <em>nome do computador</em> **"** ).

### <a name="examples"></a>Exemplos
Para criar um volume chamado estatísticas de marketing dos EUA no servidor local, usando o diretório estatísticas na unidade E para especificar que o volume não pode ser acessado por convidados, digite:
```
macfile volume /add /name:"US Marketing Statistics" /guestsallowed:false /path:e:\Stats
```
Para alterar o volume criado acima para somente leitura e exigir uma senha e definir o número máximo de usuários como cinco, digite:
```
macfile volume /set /name:"US Marketing Statistics" /readonly:true /password:saturn /maxusers:5
```
Para adicionar um volume chamado Landscape design, no servidor \\ \ Magnolia, usando o diretório Trees na unidade E para especificar que o volume pode ser acessado por convidados, digite:
```
macfile volume /add /server:\\Magnolia /name:"Landscape Design" /path:e:\trees
```
Para remover o volume chamado relatórios de vendas no servidor local, digite:
```
macfile volume /remove /name:"Sales Reports"
```

## <a name="additional-references"></a>Referências adicionais
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
