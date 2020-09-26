---
title: sc.exe criar
description: Artigo de referência para o comando sc.exe Create, que cria uma subchave e entradas para um serviço no registro e no banco de dados do Gerenciador de controle de serviço.
ms.topic: reference
ms.assetid: 59416460-0661-4fef-85cc-73e9d8f4beb4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2be59f0d91abdf91984985c536e45abb3cdc0d3f
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388505"
---
# <a name="scexe-create"></a>sc.exe criar

Cria uma subchave e entradas para um serviço no registro e no banco de dados do Gerenciador de controle de serviço.

## <a name="syntax"></a>Sintaxe

```
sc.exe [<servername>] create [<servicename>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto}] [error= {normal | severe | critical | ignore}] [binpath= <binarypathname>] [group= <loadordergroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<accountname> | <objectname>}] [displayname= <displayname>] [password= <password>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
| `<servername>` | Especifica o nome do servidor remoto no qual o serviço está localizado. O nome deve usar o formato UNC (Convenção de nomenclatura universal) (por exemplo, \\ meuservidor). Para executar o SC.exe localmente, não use esse parâmetro. |
| `<servicename>` | Especifica o nome do serviço retornado pela operação **GetKeyName** . |
| `type= {own | share | kernel | filesys | rec | interact type= {own | share}}` | Especifica o tipo de serviço. As opções incluem:<ul><li>**próprio** -especifica um serviço que é executado em seu próprio processo. Ele não compartilha um arquivo executável com outros serviços. Esse é o valor padrão.</li><li>**compartilhamento** – especifica um serviço que é executado como um processo compartilhado. Ele compartilha um arquivo executável com outros serviços.</li><li>**kernel** -especifica um driver.</li><li>**arquivos** -especifica um driver de sistema de arquivos.</li><li>**REC** -especifica um driver reconhecido pelo sistema de arquivos que identifica os sistemas de arquivos usados no computador.</li><li>**interagir** -especifica um serviço que pode interagir com a área de trabalho, recebendo entrada de usuários. Os serviços interativos devem ser executados na conta LocalSystem. Esse tipo deve ser usado em conjunto com **Type = próprio** ou **Type = Shared** (por exemplo, **Type = interaja** **Type =** is). Usar **Type = interagir** por si só gerará um erro.</li></ul> |
| `start= {boot | system | auto | demand | disabled | delayed-auto}` | Especifica o tipo de início para o serviço. As opções incluem:<ul><li>**boot** -especifica um driver de dispositivo que é carregado pelo carregador de inicialização.</li><li>**sistema** -especifica um driver de dispositivo que é iniciado durante a inicialização do kernel.</li><li>**automaticamente** especifica um serviço que é iniciado automaticamente cada vez que o computador é reiniciado e é executado mesmo que ninguém faça logon no computador.</li><li>**demanda** -especifica um serviço que deve ser iniciado manualmente. Esse será o valor padrão se **Start =** não for especificado.</li><li>**Disabled** -especifica um serviço que não pode ser iniciado. Para iniciar um serviço desabilitado, altere o tipo de início para algum outro valor.</li><li>**atrasada –** especifica automaticamente um serviço que inicia de forma automática um curto período após a inicialização de outros serviços automáticos.</li></ul> |
| `error= {normal | severe | critical | ignore}` | Especifica a severidade do erro se o serviço não for iniciado quando o computador for iniciado. As opções incluem:<ul><li>**normal** – especifica que o erro é registrado e uma caixa de mensagem é exibida, informando ao usuário que um serviço falhou ao iniciar. A inicialização continuará. Essa é a configuração padrão.</li><li>**grave** -especifica que o erro é registrado (se possível). O computador tenta reiniciar com a última configuração válida conhecida. Isso pode fazer com que o computador possa ser reiniciado, mas o serviço ainda pode não ser executado.</li><li>**crítico** -especifica que o erro é registrado (se possível). O computador tenta reiniciar com a última configuração válida conhecida. Se a configuração válida mais conhecida falhar, a inicialização também falhará e o processo de inicialização é interrompido com um erro de parada.</li><li>**ignorar** – especifica que o erro é registrado e a inicialização continua. Nenhuma notificação é dada ao usuário além de registrar o erro no log de eventos.</li></ul> |
| `binpath= <binarypathname>` | Especifica um caminho para o arquivo binário do serviço. Não há nenhum padrão para **BinPath =**, e essa cadeia de caracteres deve ser fornecida. |
| `group= <loadordergroup>` | Especifica o nome do grupo do qual esse serviço é membro. A lista de grupos é armazenada no registro, na subchave **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** . O valor padrão é nulo. |
| `tag= {yes | no}` | Especifica se deve ou não obter um TagID da chamada CreateService. As marcas são usadas somente para os drivers inicialização e início do sistema. |
| `depend= <dependencies>` | Especifica os nomes de serviços ou grupos que devem iniciar antes desse serviço. Os nomes são separados por barras (/). |
| `obj= {<accountname> | <objectname>}` | Especifica um nome de uma conta na qual um serviço será executado ou especifica um nome do objeto de driver do Windows no qual o driver será executado. A configuração padrão é **LocalSystem**. |
| `displayname= <displayname>` | Especifica um nome amigável para identificar o serviço em programas de interface do usuário. Por exemplo, o nome da subchave de um serviço específico é **wuauserv**, que tem um nome de exibição mais amigável de atualizações automáticas. |
| `password= <password>` | Especifica uma senha. Isso será necessário se uma conta diferente da conta LocalSystem for usada. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Cada opção de linha de comando (parâmetro) deve incluir o sinal de igual como parte do nome da opção.

- Um espaço é necessário entre uma opção e seu valor (por exemplo, **tipo = próprio**. Se o espaço for omitido, a operação falhará.

## <a name="examples"></a>Exemplos

Para criar e registrar um novo caminho binário para o serviço *NewService* , digite:

```
sc.exe \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
```

```
sc.exe create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= +TDI NetBIOS
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
