---
title: Criar SC
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 59416460-0661-4fef-85cc-73e9d8f4beb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7931ddc91b91d5fce01335f4b090d0305790f65c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826497"
---
# <a name="sc-create"></a>Criar SC



Cria uma subchave e entradas para um serviço no registro e no banco de dados do Gerenciador de controle de serviço.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
sc [<ServerName>] create [<ServiceName>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ServerName>|Especifica o nome do servidor remoto no qual o serviço está localizado. O nome deve usar o formato de convenção de nomenclatura Universal (UNC) (por exemplo, \\ \\myserver). Para executar SC.exe localmente, omita este parâmetro.|
|\<ServiceName>|Especifica o nome do serviço retornado pelo **getkeyname** operação.|
|tipo = {próprios \| compartilham \| kernel \| sist \| rec \| interagir tipo = {próprio \| compartilhar}}|Especifica o tipo de serviço. A configuração padrão é **tipo = próprio**.</br>**próprio** -Especifica que o serviço é executado em seu próprio processo. Ele não compartilha um arquivo executável com outros serviços. Essa é a configuração padrão.</br>**compartilhar** -Especifica que o serviço é executado como um processo compartilhado. Ele compartilha um arquivo executável com outros serviços.</br>**kernel** -Especifica um driver.</br>**sist** -Especifica um driver de sistema de arquivos.</br>**Rec** -Especifica um driver de sistema reconhecido do arquivo (identifica os sistemas de arquivo usados no computador).</br>**interagir** -Especifica que o serviço pode interagir com a área de trabalho, recebendo entrada de usuários. Serviços interativos devem ser executados sob a conta LocalSystem. Esse tipo deve ser usado em conjunto com **tipo = próprios** ou **tipo = compartilhado**. Usando o **tipo = interagir** por si só irá gerar um erro de "parâmetro inválido".|
|Iniciar = {boot \| system \| automática \| demanda \| desabilitada}|Especifica o tipo de início para o serviço. A configuração padrão é **iniciar = demand**.</br>**inicialização** -Especifica um driver de dispositivo que é carregado pelo carregador de inicialização.</br>**sistema** -Especifica um driver de dispositivo for iniciado durante a inicialização do kernel.</br>**auto** -Especifica um serviço que inicia automaticamente cada vez que o computador for reiniciado. Observe que o serviço é executado mesmo se ninguém faz logon computador.</br>**demanda** -Especifica um serviço que deve ser iniciado manualmente. Isso é o valor padrão se **iniciar =** não for especificado.</br>**desabilitado** -Especifica um serviço que não pode ser iniciado. Para iniciar um serviço desativado, altere o tipo de início para algum outro valor.|
|erro = {normal \| graves \| crítica \| ignorar}|Especifica a gravidade do erro se o serviço falhar quando o computador é iniciado. A configuração padrão é **erro = normal**.</br>**normal** -Especifica que o erro está registrado. Uma caixa de mensagem é exibida, informando ao usuário que um serviço falhou ao iniciar. A inicialização continuará. Essa é a configuração padrão.</br>**Grave** -Especifica que o erro está registrado (se possível). O computador tenta reiniciar com a última configuração válida. Isso pode resultar em um computador que está sendo capaz de reiniciar, mas ainda pode ser não é possível executar o serviço.</br>**crítico** -Especifica que o erro está registrado (se possível). O computador tenta reiniciar com a última configuração válida. Se a última configuração válida falhar, também haverá falha e o processo de inicialização for interrompida com um erro de parada.</br>**Ignorar** -Especifica que o erro é registrado e a inicialização continua. Nenhuma notificação é fornecida para o usuário além do registro do erro no log de eventos.|
|binpath= \<BinaryPathName>|Especifica um caminho para o arquivo binário do serviço. Não há nenhum padrão para **binpath =**, e essa cadeia de caracteres deve ser fornecida.|
|group= \<LoadOrderGroup>|Especifica o nome do grupo do qual esse serviço é um membro. A lista de grupos é armazenada no registro na **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder.** subchave. O valor padrão é nulo.|
|marca = {Sim \| nenhuma}|Especifica se uma TagID deve ser obtido da chamada CreateService. Marcas são usadas somente para drivers de inicialização do computador e do sistema.|
|depend= \<dependencies>|Especifica os nomes de serviços ou grupos que devem ser iniciados antes que esse serviço é iniciado. Os nomes são separados por barras (/).|
|obj= {\<AccountName> \| \<ObjectName>}|Especifica o nome de uma conta na qual um serviço será executado, ou especifica um nome de objeto do driver do Windows no qual o driver será executado.|
|displayname= \<DisplayName>|Especifica um nome amigável que pode ser usado por programas de interface do usuário para identificar o serviço.|
|senha = \<senha >|Especifica uma senha. Isso é necessário se uma conta diferente de LocalSystem for usada.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Para cada opção de linha de comando, o sinal de igual é parte do nome da opção.
-   Um espaço é necessário entre uma opção e seu valor (por exemplo, **tipo = próprio**. Se o espaço for omitido a operação falhará.

## <a name="BKMK_examples"></a>Exemplos

Os exemplos a seguir mostram como você pode usar o **sc criar** comando:
```
sc \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
sc create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= "+TDI NetBIOS"
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
