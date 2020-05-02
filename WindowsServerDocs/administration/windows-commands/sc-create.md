---
title: Criar SC
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 59416460-0661-4fef-85cc-73e9d8f4beb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0406e82afbe1f70dc27236a29c3c20489f46f44e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722204"
---
# <a name="sc-create"></a>Criar SC



Cria uma subchave e entradas para um serviço no registro e no banco de dados do Gerenciador de controle de serviço.



## <a name="syntax"></a>Sintaxe

```
sc [<ServerName>] create [<ServiceName>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto }] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ServerName>|Especifica o nome do servidor remoto no qual o serviço está localizado. O nome deve usar o formato UNC (Convenção de nomenclatura universal) (por exemplo \\ \\, meuservidor). Para executar o SC. exe localmente, omita esse parâmetro.|
|\<> do ServiceName|Especifica o nome do serviço retornado pela operação **GetKeyName** .|
|Type = {próprio \| compartilhamento \| de \| arquivos do \| kernel \| . rec interagir tipo = \| {próprio compartilhamento}}|Especifica o tipo de serviço. A configuração padrão é **Type =** is.</br>**próprio** -especifica que o serviço é executado em seu próprio processo. Ele não compartilha um arquivo executável com outros serviços. Essa é a configuração padrão.</br>**compartilhamento** – especifica que o serviço é executado como um processo compartilhado. Ele compartilha um arquivo executável com outros serviços.</br>**kernel** -especifica um driver.</br>**arquivos** -especifica um driver de sistema de arquivos.</br>**REC** -especifica um driver reconhecido pelo sistema de arquivos (identifica os sistemas de arquivos usados no computador).</br>**interagir** -especifica que o serviço pode interagir com a área de trabalho, recebendo entrada de usuários. Os serviços interativos devem ser executados na conta LocalSystem. Esse tipo deve ser usado em conjunto com **Type = próprio** ou **Type = Shared**. Usar **Type = interagir** por si só gerará um erro de parâmetro inválido.|
|início = {a \| demanda \| \| \| automática do sistema \| de inicialização foi desabilitada com atraso-automática}|Especifica o tipo de início para o serviço. A configuração padrão é **Iniciar = demanda**.</br>**boot** -especifica um driver de dispositivo que é carregado pelo carregador de inicialização.</br>**sistema** -especifica um driver de dispositivo que é iniciado durante a inicialização do kernel.</br>**automaticamente** especifica um serviço que é iniciado automaticamente cada vez que o computador é reiniciado. Observe que o serviço é executado mesmo que não haja um logon no computador.</br>**demanda** -especifica um serviço que deve ser iniciado manualmente. Esse será o valor padrão se **Start =** não for especificado.</br>**Disabled** -especifica um serviço que não pode ser iniciado. Para iniciar um serviço desabilitado, altere o tipo de início para algum outro valor.</br>**atrasada –** especifica automaticamente um serviço que inicia de forma automática um curto período após a inicialização de outros serviços automáticos.|
|erro = {ignorar \| crítico \| \| normal grave}|Especifica a severidade do erro se o serviço falhar quando o computador for iniciado. A configuração padrão é **Error = normal**.</br>**normal** -especifica que o erro é registrado. Uma caixa de mensagem é exibida, informando ao usuário que um serviço falhou ao ser iniciado. A inicialização continuará. Essa é a configuração padrão.</br>**grave** -especifica que o erro é registrado (se possível). O computador tenta reiniciar com a última configuração válida conhecida. Isso pode fazer com que o computador possa ser reiniciado, mas o serviço ainda pode não ser executado.</br>**crítico** -especifica que o erro é registrado (se possível). O computador tenta reiniciar com a última configuração válida conhecida. Se a configuração válida mais conhecida falhar, a inicialização também falhará e o processo de inicialização é interrompido com um erro de parada.</br>**ignorar** – especifica que o erro é registrado e a inicialização continua. Nenhuma notificação é dada ao usuário além de registrar o erro no log de eventos.|
|BinPath = \<BinaryPathName>|Especifica um caminho para o arquivo binário do serviço. Não há nenhum padrão para **BinPath =**, e essa cadeia de caracteres deve ser fornecida.|
|Group = \<> do grupo de ordens|Especifica o nome do grupo do qual esse serviço é membro. A lista de grupos é armazenada no registro na subchave **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** . O valor padrão é nulo.|
|marca = {sim \| não}|Especifica se um TagID deve ser obtido ou não da chamada CreateService. As marcas são usadas somente para os drivers inicialização e início do sistema.|
|depend = \<dependências>|Especifica os nomes de serviços ou grupos que devem ser iniciados antes do início desse serviço. Os nomes são separados por barras (/).|
|obj = {\<AccountName> \| \<ObjectName>}|Especifica o nome de uma conta na qual um serviço será executado ou especifica um nome do objeto de driver do Windows no qual o driver será executado.|
|DisplayName = \<DisplayName>|Especifica um nome amigável que pode ser usado por programas de interface do usuário para identificar o serviço.|
|senha = \<senha>|Especifica uma senha. Isso será necessário se uma conta diferente de LocalSystem for usada.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Para cada opção de linha de comando, o sinal de igual faz parte do nome da opção.
-   Um espaço é necessário entre uma opção e seu valor (por exemplo, **tipo = próprio**. Se o espaço for omitido, a operação falhará.

## <a name="examples"></a>Exemplos

Os exemplos a seguir mostram como você pode usar o comando **SC Create** :
```
sc \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
sc create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= +TDI NetBIOS
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
