---
title: Sc config
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ad4d68a6-efe5-452b-8501-7f1f1c552a4a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: 361a8407aae3b5e823b58cd71b97b043159146e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837217"
---
# <a name="sc-config"></a>Sc config



Modifica o valor de entradas de um serviço no registro e no banco de dados do Gerenciador de controle de serviço.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
sc [<ServerName>] config [<ServiceName>] [type= {own | share | kernel | filesys | rec | adapt | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ServerName>|Especifica o nome do servidor remoto no qual o serviço está localizado. O nome deve usar o formato de convenção de nomenclatura Universal (UNC) (por exemplo, \\ \\myserver). Para executar SC.exe localmente, omita este parâmetro.|
|\<ServiceName>|Especifica o nome do serviço retornado pelo **getkeyname** operação.|
|type= {own\| share \| kernel \| filesys \| rec \| adapt \| interact type= {own \| share}} | Especifica o tipo de serviço.</br>**próprio** -Especifica um serviço que é executado em seu próprio processo. Ele não compartilha um arquivo executável com outros serviços. Este é o valor padrão.</br>**compartilhar** -Especifica um serviço que é executado como um processo compartilhado. Ele compartilha um arquivo executável com outros serviços.</br>**kernel** -Especifica um driver.</br>**sist** -Especifica um driver de sistema de arquivos.</br>**Rec** -Especifica um driver reconhecido pelo sistema de arquivos que identifica os sistemas de arquivos usados no computador.</br>**adaptar** - Especifica um driver de adaptador que identifica os dispositivos de hardware, como teclados, mouses, e unidades de disco.</br>**interagir** -Especifica um serviço que pode interagir com a área de trabalho, recebendo entrada de usuários. Serviços interativos devem ser executados sob a conta LocalSystem. Esse tipo deve ser usado em conjunto com **tipo = próprios** ou **tipo = compartilhado** (por exemplo, **tipo = interagir** **tipo = próprio**). Usando o **tipo = interagir** por si só irá gerar um erro.|
|Iniciar = {boot \| system \| automática \| demanda \| desabilitado \| automático atrasado}|Especifica o tipo de início para o serviço.</br>**inicialização** -Especifica um driver de dispositivo que é carregado pelo carregador de inicialização.</br>**sistema** -Especifica um driver de dispositivo for iniciado durante a inicialização do kernel.</br>**auto** - Especifica um serviço que automaticamente inicia cada vez que o computador for reiniciado e é executado mesmo se ninguém faz logon computador.</br>**demanda** -Especifica um serviço que deve ser iniciado manualmente. Isso é o valor padrão se **iniciar =** não for especificado.</br>**desabilitado** -Especifica um serviço que não pode ser iniciado. Para iniciar um serviço desativado, altere o tipo de início para algum outro valor.</br>**automático atrasado** -Especifica um serviço que inicia automaticamente um curto período depois de outros serviços automático são iniciados.|
|erro = {normal \| graves \| crítica \| ignorar}|Especifica a gravidade do erro se o serviço não for iniciado no momento da inicialização.</br>**normal** -Especifica que o erro é registrado e uma caixa de mensagem é exibida, informando ao usuário que um serviço falhou ao iniciar. A inicialização continuará. Essa é a configuração padrão.</br>**Grave** -Especifica que o erro está registrado (se possível). O computador tenta reiniciar com a última configuração válida. Isso pode resultar em um computador que está sendo capaz de reiniciar, mas ainda pode ser não é possível executar o serviço.</br>**crítico** -Especifica que o erro está registrado (se possível). O computador tenta reiniciar com a última configuração válida. Se a última configuração válida falhar, também haverá falha e o processo de inicialização for interrompida com um erro de parada.</br>**Ignorar** -Especifica que o erro é registrado e a inicialização continua. Nenhuma notificação é fornecida para o usuário além do registro do erro no Log de eventos.|
|binpath= \<BinaryPathName>|Especifica um caminho para o arquivo binário do serviço.|
|group= \<LoadOrderGroup>|Especifica o nome do grupo do qual esse serviço é um membro. A lista de grupos é armazenada no registro, na **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder.** subchave. O valor padrão é nulo.|
|marca = {Sim \| nenhuma}|Especifica se deve ou não obter um TagID da chamada CreateService. Marcas são usadas somente para drivers de inicialização do computador e do sistema.|
|depend= \<dependencies>|Especifica os nomes de serviços ou grupos que devem ser iniciados antes deste serviço. Os nomes são separados por barras (/).|
|obj= {\<AccountName> \| \<ObjectName>}|Especifica um nome de uma conta na qual um serviço será executado, ou especifica um nome de objeto do driver do Windows no qual o driver será executado. A configuração padrão é **LocalSystem**.|
|displayname= \<DisplayName>|Especifica um nome descritivo para identificar o serviço em programas de interface do usuário. Por exemplo, é o nome de um serviço específico da subchave **wuauserv**, que tem um nome mais amigável para exibição de atualizações automáticas.|
|senha = \<senha >|Especifica uma senha. Isso é necessário se for usada uma conta diferente da conta LocalSystem.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Para cada opção de linha de comando (parâmetro), o sinal de igual é parte do nome da opção.
-   Um espaço é necessário entre uma opção e seu valor (por exemplo, **tipo = próprio**. Se o espaço for omitido, a operação falhará.

## <a name="BKMK_examples"></a>Exemplos

Para especificar um caminho binário para o serviço NEWSERVICE, digite:
```
sc config NewService binpath= "ntsd -d c:\windows\system32\NewServ.exe"
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)