---
title: bootcfg ems
description: Tópico de comandos do Windows para **ems bootcfg** -permite que o usuário adicionar ou alterar as configurações de redirecionamento do console do EMS para um computador remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57abdc50-c64a-45f1-8470-3f8c3a51f743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 97f0126fdfa4d800692a044d33e3de0f0d4c6cdb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861637"
---
# <a name="bootcfg-ems"></a>bootcfg ems

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que o usuário adicionar ou alterar as configurações de redirecionamento do console do EMS para um computador remoto. Habilitando serviços de gerenciamento de emergência, você deve adicionar um "redirecionamento = número da porta" linha à seção [boot loader] do arquivo Boot. ini e uma opção /redirect à linha de entrada de sistema operacional especificado. O recurso de serviços de gerenciamento de emergência é habilitado somente em servidores.

## <a name="syntax"></a>Sintaxe
```
bootcfg /ems {ON | OFF | edit} [/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|{ON &#124; OFF&#124; edit}|Especifica o valor para o redirecionamento dos serviços de gerenciamento de emergência.<br /><br />**Diante** -habilita a saída remota especificado <OSEntryLineNum>. Adiciona uma opção /redirect especificado <OSEntryLineNum> e um redirecionamento CN=User1,CN=Users,DC=Domain,DC=com<X> configuração à seção [carregador de inicialização. O valor de com<X> é definido pela **/porta** parâmetro.<br /><br />**DESATIVAR** -desabilita a saída para um computador remoto. Remove a opção /redirect especificado <OSEntryLineNum> e o redirecionamento CN=User1,CN=Users,DC=Domain,DC=com<X> configuração da seção [carregador de inicialização.<br /><br />**Edite** -permite que as alterações nas configurações de porta, alterando o redirecionamento CN=User1,CN=Users,DC=Domain,DC=com<X> definindo na seção do carregador de inicialização]. O valor de com<X> é redefinido para o valor especificado pela **/porta** parâmetro.|
|/s <computer>|Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.|
|/u <Domain>\\<User>|Executa o comando com as permissões de conta do usuário especificado por <User> ou <Domain> \\ <User>. O padrão é que as permissões do usuário no computador que está emitindo o comando de logon atual.|
|/p <Password>|Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.|
|porta / {COM1 &#124; COM2 &#124; COM3 &#124; COM4 &#124; BIOSSET}|Especifica a porta COM a ser usada no redirecionamento. **BIOSSET** direciona os serviços de gerenciamento de emergência para obter as configurações de BIOS para determinar qual porta deve ser usada para redirecionamento. Não use o **/porta** parâmetro, se administrado remotamente a saída está sendo desativado.|
|/baud {9600 &#124; 19200 &#124; 38400&#124; 57600 &#124; 115200}|Especifica a taxa de transmissão a ser usado para redirecionamento. Não use o **/baud** parâmetro, se administrado remotamente a saída está sendo desativado.|
|/id <OSEntryLineNum>|Especifica o número de linha de entrada do sistema operacional para o qual a opção Emergency Management Services é adicionada na seção [operating systems] do arquivo Boot. ini. A primeira linha depois que o cabeçalho da seção [sistemas operacionais] é 1. Esse parâmetro é necessário quando o valor de serviços de gerenciamento de emergência é definido como **Diante** ou **OFF**.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="BKMK_examples"></a>Exemplos
Os exemplos a seguir mostram como você pode usar o **bootcfg /ems** comando:
```
bootcfg /ems on /port com1 /baud 19200 /id 2 
bootcfg /ems on /port biosset /id 3 
bootcfg /s srvmain /ems off /id 2 
bootcfg /ems edit /port com2 /baud 115200 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /ems off /id 2
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
