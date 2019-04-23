---
title: macfile
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c50c53fc277626d1b83268f5e0c8dcc95161f35a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854047"
---
# <a name="macfile"></a>macfile

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gerencia o servidor de arquivos para servidores, volumes, pastas e arquivos Macintosh. Você pode automatizar tarefas administrativas, incluindo uma série de comandos em arquivos em lotes e iniciá-los manualmente ou em horários predeterminados. 
-   [Para modificar pastas nos volumes acessível a Macintosh](#BKMK_Moddirs)
-   [Para unir as bifurcações de recursos e dados de um arquivo de Macintosh](#BKMK_Joinforks)
-   [Para alterar a mensagem de logon e limitar as sessões](#BKMK_LogonLimit)
-   [Para adicionar, alterar ou remover volumes acessível a Macintosh](#BKMK_addvol)

## <a name="BKMK_Moddirs"></a>Para modificar pastas nos volumes acessível a Macintosh

### <a name="syntax"></a>Sintaxe
```
macfile directory[/server:\\<computerName>] /path:<directory> [/owner:<OwnerName>] [/group:<GroupName>] [/permissions:<Permissions>]
```

### <a name="parameters"></a>Parâmetros
-   /server:\\\\<computerName> Especifica o servidor no qual alterar um diretório. Se omitido, a operação é executada no computador local.
-   /path:<directory> Obrigatório. Especifica o caminho para o diretório que você deseja alterar. O diretório deve existir. **diretório MacFile** não cria pastas.
-   /Owner:<OwnerName> altera o proprietário do diretório. Se omitido, o proprietário permanecerá inalterado.
-   /group:<GroupName> Especifica ou altera o grupo primário do Macintosh que está associado com o diretório. Se omitido, o grupo primário permanece inalterado.
-   /Permissions:<Permissions> Define as permissões no diretório para o proprietário, grupo primário e todos. Um número de 11 dígitos é usado para definir permissões. O número 1 concede permissão e 0 nega permissão (por exemplo, 11111011000). Se omitido, as permissões permanecem inalteradas.
    A posição do dígito determina o conjunto de permissões, conforme descrito na tabela a seguir.
    
    |Posição|Define a permissão para|
    |------|------------|
    |First|OwnerSeeFiles|
    |Segundo|OwnerSeeFolders|
    |Terceiro|OwnerMakechanges|
    |Quarto|GroupSeeFiles|
    |Quinta|GroupSeeFolders|
    |Sexto|GroupMakechanges|
    |Sétimo|WorldSeeFiles|
    |Oitavo|WorldSeeFolders|
    |Nono|WorldMakechanges|
    |Décimo|O diretório não pode ser renomeado, movido ou excluído.|
    |Décima primeira|As alterações se aplicam para o diretório atual e todos os subdiretórios.|
    
-   /?
    Exibe a ajuda no prompt de comando.
    
### <a name="remarks"></a>Comentários
-   Se as informações que você fornece contiverem espaços ou caracteres especiais, use aspas ao redor do texto (por exemplo, **"***nome do computador***"**).
-   Use **macfiledirectory** para disponibilizar um diretório existente em um volume Macintosh acessível aos usuários do Macintosh. O **macfiledirectory** comando não cria pastas. Use o Gerenciador de arquivos, o prompt de comando, ou o **nova pasta macintosh** comando para criar um diretório em um volume acessível a Macintosh antes de usar o **macfile directory** comando.
### <a name="BKMK_Examples"></a>Exemplos
O exemplo a seguir altera as permissões do subdiretório maio de vendas, no volume acessível a Macintosh estatísticas, na unidade E do servidor local. O exemplo atribui ver arquivos, pastas e faça alterações permissões para o proprietário e ver arquivos e pastas Consulte permissões para todos os outros usuários, enquanto impede que o diretório que está sendo renomeado, movido ou excluído.
```
macfile directory /path:"e:\statistics\may sales" /permissions:11111011000
```

## <a name="BKMK_Joinforks"></a>Para unir as bifurcações de recursos e dados de um arquivo de Macintosh

### <a name="syntax"></a>Sintaxe
```
macfile forkize[/server:\\<computerName>] [/creator:<CreatorName>] [/type:<typeName>]  [/datafork:<Filepath>] [/resourcefork:<Filepath>] /targetfile:<Filepath>
```

### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/server:\\\\<computerName>|Especifica o servidor no qual se unir a arquivos. Se omitido, a operação é executada no computador local.|
|/creator:<CreatorName>|Especifica o criador do arquivo. O Macintosh finder usa o **/creator** a opção de linha de comando para determinar o aplicativo que criou o arquivo.|
|/type:<typeName>|Especifica o tipo de arquivo. O Macintosh finder usa o **/tipo** opção de linha de comando para determinar o tipo de arquivo dentro do aplicativo que criou o arquivo.|
|/datafork:<Filepath>|Especifica o local da bifurcação de dados que deve ser unida. Você pode especificar um caminho remoto.|
|/resourcefork:<Filepath>|Especifica o local da bifurcação de recurso que deve ser unida. Você pode especificar um caminho remoto.|
|/targetfile:<Filepath>|Obrigatório. Especifica o local do arquivo que é criado unindo uma bifurcação de dados e uma bifurcação de recursos ou especifica o local do arquivo cujo tipo ou criador você está alterando. O arquivo deve estar no servidor especificado.|
|/?|Exibe a ajuda no prompt de comando.|

### <a name="remarks"></a>Comentários
-   Se as informações que você fornece contiverem espaços ou caracteres especiais, use aspas ao redor do texto (por exemplo, **"***nome do computador***"**).

### <a name="examples"></a>Exemplos
Para criar o arquivo de lote no volume acessível a Macintosh D:\Release, usando o recurso de bifurcação C:\Cross\Mac\Appcode e fazer com que esse novo arquivo aparecem para clientes Macintosh como um aplicativo (aplicativos Macintosh utilizam o tipo APPL) com o criador (assinatura ) definido como MAGNOLIA, tipo:
```
macfile forkize /resourcefork:c:\cross\mac\appcode /type:APPL /creator:MAGNOLIA /targetfile:D:\Release\treeapp
```
Para alterar o criador de arquivo para Microsoft Word 5.1 para o arquivo S_gomes nos arquivos de Word\Arquivos D:\Word de diretório, no servidor \\\SERverA, digite:
```
macfile forkize /server:\\servera /creator:MSWD /type:TEXT /targetfile:"d:\Word documents\Group files\Word.txt"
```

## <a name="BKMK_LogonLimit"></a>Para alterar a mensagem de logon e limitar as sessões
### <a name="syntax"></a>Sintaxe
```
macfile server [/server:\\<computerName>] [/maxsessions:{Number | unlimited}] [/loginmessage:<Message>]
```

### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/server:\\\\<computerName>|Especifica o servidor no qual alterar parâmetros. Se omitido, a operação é executada no computador local.|
|/maxsessions:{Number &#124; unlimited}|Especifica o número máximo de usuários que podem usar o arquivo e servidores de impressão para Macintosh simultaneamente. Se omitido, o **maxsessions** configuração para o servidor permanece inalterado.|
|/loginmessage:<Message>|Altera os mensagem que os usuários de Macintosh verão ao fazer logon no servidor de arquivo para Macintosh. O número máximo de caracteres para a mensagem de logon é 199. Se omitido, o **loginmessage** de mensagem para o servidor permanece inalterado. Para remover uma mensagem de logon existente, inclua o **/loginmessage** parâmetro, mas deixe o *mensagem* variável em branco.|
|/?|Exibe a ajuda no prompt de comando.|

### <a name="remarks"></a>Comentários
-   Se as informações que você fornece contiverem espaços ou caracteres especiais, use aspas ao redor do texto (por exemplo, **"***nome do computador***"**).

### <a name="examples"></a>Exemplos
Para alterar o número de arquivo e servidor de impressão para sessões Macintosh que são permitidas no servidor local da configuração atual para cinco sessões e para adicionar a mensagem de logon "Fazer logoff do servidor para Macintosh quando tiver terminado.", digite:
```
macfile server /maxsessions:5 /loginmessage:"Log off from Server for Macintosh when you are finished."
```

## <a name="BKMK_addvol"></a>Para adicionar, alterar ou remover volumes acessível a Macintosh
### <a name="syntax"></a>Sintaxe
```
macfile volume {/add|/set} [/server:\\<computerName>] /name:<volumeName>/path:<directory>[/readonly:{true | false}] [/guestsallowed:{true | false}] [/password:<Password>] [/maxusers:{<Number>>|unlimited}]
macfile volume /remove[/server:\\<computerName>] /name:<volumeName>
```

### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|{/add &#124; /set}|Necessário quando você está adicionando ou alterando um volume acessível a Macintosh. Adiciona ou altera o volume especificado.|
|/server:\\\\<computerName>|Especifica o servidor no qual você deseja adicionar, alterar ou remover um volume. Se omitido, a operação é executada no computador local.|
|/name:<volumeName>|Obrigatório. Especifica o nome do volume a ser adicionado, alterado ou removido.|
|/path:<directory>|Necessária e é válido somente quando você estiver adicionando um volume. Especifica o caminho para o diretório raiz do volume a ser adicionado.|
|/readonly:{true &#124; false}|Especifica se os usuários podem alterar os arquivos no volume. Digite true para especificar que os usuários não podem alterar os arquivos no volume. Digite falso para especificar que os usuários podem alterar os arquivos no volume. Se omitido ao adicionar um volume, são permitidas alterações nos arquivos. Se for omitido ao alterar um volume, o **readonly** configuração para o volume permanece inalterado.|
|/guestsallowed:{true &#124; false}|Especifica se os usuários que fizerem logon como convidados podem usar o volume. Digite true para especificar que convidados podem usar o volume. Digite falso para especificar que os convidados não podem usar o volume. Se omitido ao adicionar um volume, os convidados podem usar o volume. Se for omitido ao alterar um volume, o **guestsallowed** configuração para o volume permanece inalterado.|
|/password:<Password>|Especifica uma senha que serão necessários para acessar o volume. Se omitido ao adicionar um volume, nenhuma senha é criada. Se omitido ao alterar um volume, a senha permanece inalterada.|
|/maxusers:{<Number>>&#124;unlimited}|Especifica o número máximo de usuários que podem usar simultaneamente os arquivos no volume. Se omitido ao adicionar um volume, um número ilimitado de usuários pode usar o volume. Se for omitido ao alterar um volume, o **maxusers** valor permanece inalterado.|
|/remove|Necessário quando você estiver removendo um volume acessível por Macintosh. Remove o volume especificado.|
|/?|Exibe a ajuda no prompt de comando.|

### <a name="remarks"></a>Comentários
-   Se as informações que você fornece contiverem espaços ou caracteres especiais, use aspas ao redor do texto (por exemplo, **"***nome do computador***"**).

### <a name="examples"></a>Exemplos
Para criar um volume chamado estatísticas de Marketing dos EUA no servidor local, usando o diretório de estatísticas na unidade e para especificar que o volume não pode ser acessado por convidados, digite:
```
macfile volume /add /name:"US Marketing Statistics" /guestsallowed:false /path:e:\Stats
```
Para alterar o volume criado acima para ser somente leitura e exigir uma senha e definir o número de máximo de usuários a cinco, tipo:
```
macfile volume /set /name:"US Marketing Statistics" /readonly:true /password:saturn /maxusers:5
```
Para adicionar um volume chamado modo paisagem, no servidor \\\Magnolia, usando o diretório de árvores na unidade E e para especificar que o volume pode ser acessado por convidados, digite:
```
macfile volume /add /server:\\Magnolia /name:"Landscape Design" /path:e:\trees
```
Para remover o volume denominado relatórios de vendas no servidor local, digite:
```
macfile volume /remove /name:"Sales Reports"
```

## <a name="additional-references"></a>Referências adicionais
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
