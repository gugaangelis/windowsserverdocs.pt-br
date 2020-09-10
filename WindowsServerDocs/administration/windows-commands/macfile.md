---
title: macfile
description: Artigo de referência para o comando MacFile, que gerencia o servidor de arquivos para servidores Macintosh, volumes, diretórios e arquivos.
ms.topic: reference
ms.assetid: e2ce586c-b316-41d3-90f8-4be0d074cc0e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6ed5af42108d56016a4b5793993cf19c80a87ff3
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633835"
---
# <a name="macfile"></a>macfile

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gerencia o servidor de arquivos para servidores Macintosh, volumes, diretórios e arquivos. Você pode automatizar tarefas administrativas incluindo uma série de comandos em arquivos em lotes e iniciá-los manualmente ou em horários predeterminados.

## <a name="modify-directories-in-macintosh-accessible-volumes"></a>Modificar diretórios em volumes acessíveis ao Macintosh

Para alterar o nome do diretório, o local, o proprietário, o grupo e as permissões para volumes acessíveis ao Macintosh.

### <a name="syntax"></a>Sintaxe

```
macfile directory[/server:\\<computername>] /path:<directory> [/owner:<ownername>] [/group:<groupname>] [/permissions:<permissions>]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /server:`\\<computername>` | Especifica o servidor no qual alterar um diretório. Se omitido, a operação será executada no computador local. |
| /Path`<directory>` | Especifica o caminho para o diretório que você deseja alterar. Este parâmetro é necessário. **Observação:** O diretório deve existir, usando o **Macfile directory** não criará diretórios. |
| /Owner`<ownername>` | Altera o proprietário do diretório. Se omitido, o nome do proprietário não será alterado. |
| Group`<groupname>` | Especifica ou altera o grupo primário do Macintosh que está associado ao diretório. Se omitido, o grupo primário permanecerá inalterado. |
| permissões`<permissions>` | Define permissões no diretório para o proprietário, o grupo primário e o mundo (todos). Deve ser um número de 11 dígitos, em que o número 1 concede permissão e 0 revoga a permissão (por exemplo, 11111011000). Se esse parâmetro for omitido, as permissões permanecerão inalteradas. |
| /? | Exibe a ajuda no prompt de comando. |

##### <a name="position-of-permissions-digit"></a>Posição do dígito de permissões

A posição do dígito de permissões determina qual permissão está definida, incluindo:

| Posição | Define a permissão |
| -------- | --------------- |
| Primeiro | OwnerSeeFiles |
| Segundo | OwnerSeeFolders |
| Terceiro | OwnerMakechanges |
| Quarto | GroupSeeFiles |
| Quinto | GroupSeeFolders |
| Sexto | GroupMakechanges |
| Sétima | WorldSeeFiles |
| 1/8 | WorldSeeFolders |
| Nono | WorldMakechanges |
| Décima | O diretório não pode ser renomeado, movido ou excluído. |
| Décima primeira coleta | As alterações se aplicam ao diretório atual e a todos os subdiretórios. |

##### <a name="remarks"></a>Comentários

- Se as informações fornecidas contiverem espaços ou caracteres especiais, use aspas ao contrário do texto (por exemplo, " `<computer name>` ").

- Use o **diretório MacFile** para tornar um diretório existente em um volume acessível para Macintosh disponível para usuários do Macintosh. O comando **Macfile directory** não cria diretórios.

- Use o Gerenciador de arquivos, o prompt de comando ou o comando de **nova pasta do Macintosh** para criar um diretório em um volume acessível por Macintosh antes de usar o comando **Macfile directory** .

#### <a name="examples"></a>Exemplos

Para atribuir as permissões *ver arquivos*, *ver pastas*e *fazer alterações* ao proprietário, para definir as permissões de *pasta* para todos os outros usuários e para impedir que o diretório seja renomeado, movido ou excluído, digite:

```
macfile directory /path:e:\statistics\may sales /permissions:11111011000
```

Onde o subdiretório *pode ser vendas*, localizado nas *estatísticas*de volume acessíveis ao Macintosh, no E:\ unidade do servidor local.

## <a name="join-a-macintosh-files-data-and-resource-forks"></a>Unir as bifurcações de dados e recursos de um arquivo de Macintosh

Para especificar o servidor no qual os arquivos serão agrupados, quem criou o arquivo, o tipo de arquivo, onde a bifurcação de dados está localizada, onde a bifurcação de recursos está localizada e onde o arquivo de saída deve estar localizado.

### <a name="syntax"></a>Sintaxe

```
macfile forkize[/server:\\<computername>] [/creator:<creatorname>] [/type:<typename>]  [/datafork:<filepath>] [/resourcefork:<filepath>] /targetfile:<filepath>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /server:`\\<computername>` | Especifica o servidor no qual os arquivos são agrupados. Se omitido, a operação será executada no computador local. |
| necessitam`<creatorname>` | Especifica o criador do arquivo. O localizador de Macintosh usa a opção de linha de comando **/Creator** para determinar o aplicativo que criou o arquivo. |
| /Type`<typename>` | Especifica o tipo de arquivo. O localizador de Macintosh usa a opção de linha de comando **/Type** para determinar o tipo de arquivo dentro do aplicativo que criou o arquivo. |
| /datafork:`<filepath>` | Especifica o local da bifurcação de dados que deve ser unida. Você pode especificar um caminho remoto. |
| /resourcefork:`<filepath>` | Especifica o local da bifurcação de recursos que deve ser unida. Você pode especificar um caminho remoto. |
| TargetFile`<filepath>` | Especifica o local do arquivo que é criado unindo-se uma bifurcação de dados e uma bifurcação de recursos, ou especifica o local do arquivo cujo tipo ou criador você está alterando. O arquivo deve estar no servidor especificado. Este parâmetro é necessário. |
| /? | Exibe a ajuda no prompt de comando. |

##### <a name="remarks"></a>Comentários

- Se as informações fornecidas contiverem espaços ou caracteres especiais, use aspas ao contrário do texto (por exemplo, " `<computer name>` ").

#### <a name="examples"></a>Exemplos

Para criar o arquivo *tree_app* no volume acessível para Macintosh *D:\release*, usando o *C:\Cross\Mac\Appcode*de bifurcação de recursos e para tornar esse novo arquivo exibido para clientes Macintosh como um aplicativo (aplicativos Macintosh usam o tipo *APL*) com o criador (assinatura) definido como *Magnolia*, digite:

```
macfile forkize /resourcefork:c:\cross\mac\appcode /type:APPL /creator:MAGNOLIA /targetfile:D:\Release\tree_app
```

Para alterar o criador do arquivo para o *Microsoft Word 5,1*, para o arquivo *Word.txt* no diretório *D:\Word documents\Group files* *, no servidor ServerName \\ *, digite:

```
macfile forkize /server:\\ServerA /creator:MSWD /type:TEXT /targetfile:d:\Word documents\Group files\Word.txt
```

## <a name="change-the-sign-in-message-and-limit-sessions"></a>Alterar a mensagem de entrada e limitar as sessões

Para alterar a mensagem de logon que aparece quando um usuário entra no servidor de arquivos para o servidor Macintosh e para limitar o número de usuários que podem usar simultaneamente os servidores de arquivos e de impressão para Macintosh.

### <a name="syntax"></a>Sintaxe

```
macfile server [/server:\\<computername>] [/maxsessions:{number | unlimited}] [/loginmessage:<message>]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- |------------ |
| /server:`\\<computername>` | Especifica o servidor no qual os parâmetros são alterados. Se omitido, a operação será executada no computador local. |
| /maxsessions:`{number | unlimited}` | Especifica o número máximo de usuários que podem usar simultaneamente os servidores de arquivos e de impressão para Macintosh. Se omitido, a configuração **maxsessions** para o servidor permanecerá inalterada. |
| /loginmessage:`<message>` | Altera a mensagem que os usuários do Macintosh veem ao entrar no servidor de arquivos para o servidor Macintosh. O número máximo de caracteres para a mensagem de entrada é 199. Se omitido, a mensagem **loginmessage** para o servidor permanecerá inalterada. Para remover uma mensagem de entrada existente, inclua o parâmetro **/loginmessage** , mas deixe a variável de *mensagem* em branco. |
| /? | Exibe a ajuda no prompt de comando. |

##### <a name="remarks"></a>Comentários

- Se as informações fornecidas contiverem espaços ou caracteres especiais, use aspas ao contrário do texto (por exemplo, " `<computer name>` ").

#### <a name="examples"></a>Exemplos

Para alterar o número de arquivos permitidos e o servidor de impressão para sessões do Macintosh no servidor local para cinco sessões e para adicionar a mensagem de entrada "sair do servidor para Macintosh quando terminar", digite:

```
macfile server /maxsessions:5 /loginmessage:Sign off from Server for Macintosh when you are finished
```

## <a name="add-change-or-remove-macintosh-accessible-volumes"></a>Adicionar, alterar ou remover volumes acessíveis ao Macintosh

Para adicionar, alterar ou remover um volume acessível ao Macintosh.

### <a name="syntax"></a>Sintaxe

```
macfile volume {/add|/set} [/server:\\<computername>] /name:<volumename>/path:<directory>[/readonly:{true | false}] [/guestsallowed:{true | false}] [/password:<password>] [/maxusers:{<number>>|unlimited}]
macfile volume /remove[/server:\\<computername>] /name:<volumename>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `{/add | /set}` | Necessário ao adicionar ou alterar um volume acessível ao Macintosh. Adiciona ou altera o volume especificado. |
| /server:`\\<computername>` | Especifica o servidor no qual adicionar, alterar ou remover um volume. Se omitido, a operação será executada no computador local. |
| /Name`<volumename>` | Obrigatórios. Especifica o nome do volume a ser adicionado, alterado ou removido. |
| /Path`<directory>` | Obrigatório e válido somente quando você está adicionando um volume. Especifica o caminho para o diretório raiz do volume a ser adicionado. |
| leitura`{true | false}` | Especifica se os usuários podem alterar arquivos no volume. Use **true** para especificar que os usuários não podem alterar arquivos no volume. Use **false** para especificar que os usuários podem alterar arquivos no volume. Se omitido ao adicionar um volume, as alterações nos arquivos são permitidas. Se omitido ao alterar um volume, a configuração **ReadOnly** para o volume permanece inalterada. |
| /guestsallowed:`{true | false}` | Especifica se os usuários que fazem logon como convidados podem usar o volume. Use **true** para especificar que os convidados podem usar o volume. Use **false** para especificar que os convidados não podem usar o volume. Se omitido ao adicionar um volume, os convidados poderão usar o volume. Se omitido ao alterar um volume, a configuração **guestsallowed** para o volume permanece inalterada. |
| /Password`<password>` | Especifica uma senha que será necessária para acessar o volume. Se omitido ao adicionar um volume, nenhuma senha é criada. Se omitido ao alterar um volume, a senha permanece inalterada. |
| /maxusers:`{<number>> | unlimited}` | Especifica o número máximo de usuários que podem usar simultaneamente os arquivos no volume. Se omitido ao adicionar um volume, um número ilimitado de usuários poderá usar o volume. Se omitido ao alterar um volume, o valor de **maxusers** permanece inalterado.                                                 |
| /remove | Necessário quando você está removendo um volume acessível para Macintosh. Remove o volume especificado. |
| /? | Exibe a ajuda no prompt de comando. |

##### <a name="remarks"></a>Comentários

- Se as informações fornecidas contiverem espaços ou caracteres especiais, use aspas ao contrário do texto (por exemplo, " `<computer name>` ").

#### <a name="examples"></a>Exemplos

Para criar um volume chamado *Estatísticas de marketing dos EUA* no servidor local, usando o diretório *estatísticas* na unidade e para especificar que o volume não pode ser acessado por convidados, digite:

```
macfile volume /add /name:US Marketing Statistics /guestsallowed:false /path:e:\Stats
```

Para alterar o volume criado acima para somente leitura, para exigir uma senha e para definir o número máximo de usuários como cinco, digite:

```
macfile volume /set /name:US Marketing Statistics /readonly:true /password:saturn /maxusers:5
```

Para adicionar um volume chamado *Landscape design*, no servidor * \\ Magnolia*, usando o diretório *Trees* na unidade e para especificar que o volume pode ser acessado por convidados, digite:

```
macfile volume /add /server:\\Magnolia /name:Landscape Design /path:e:\trees
```

Para remover o volume chamado *relatórios de vendas* no servidor local, digite:

```
macfile volume /remove /name:Sales Reports
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
