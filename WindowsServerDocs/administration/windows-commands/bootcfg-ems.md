---
title: bootcfg ems
description: Tópico de comandos do Windows para **Bootcfg EMS** – permite que o usuário adicione ou altere as configurações de redirecionamento do console dos serviços de gerenciamento de emergência para um computador remoto.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c0e761f3796121cc71be88d23e25371369a3ee90
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379958"
---
# <a name="bootcfg-ems"></a>bootcfg ems

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que o usuário adicione ou altere as configurações de redirecionamento do console dos serviços de gerenciamento de emergência para um computador remoto. Ao habilitar os serviços de gerenciamento de emergência, você adiciona uma linha "Redirect = Port #" à seção [boot loader] do arquivo boot. ini e uma opção/Redirect à linha de entrada do sistema operacional especificado. O recurso serviços de gerenciamento de emergência é habilitado somente em servidores.

## <a name="syntax"></a>Sintaxe
```
bootcfg /ems {ON | OFF | edit} [/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parâmetros

|                            Parâmetro                             |                                                                                                                                                                                                                                                                                                                                                              Descrição                                                                                                                                                                                                                                                                                                                                                              |
|------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    {ON &#124; off&#124; Edit}                    | Especifica o valor para redirecionamento de serviços de gerenciamento de emergência.<br /><br />**Ativa** -habilita a saída remota para o <OSEntryLineNum>especificado. Adiciona uma opção/Redirect à <OSEntryLineNum> especificada e uma configuração de<X> de com para a seção [boot loader]. O valor de<X> com é definido pelo parâmetro **/Port** .<br /><br />**Off** -desabilita a saída para um computador remoto. Remove a opção/Redirect da <OSEntryLineNum> especificada e a configuração redirecionar = com<X> da seção [boot loader].<br /><br />**Editar** – permite alterações nas configurações de porta alterando a configuração redirect = com<X> na seção [boot loader]. O valor de<X> com é redefinido para o valor especificado pelo parâmetro **/Port** . |
|                          /s <computer>                           |                                                                                                                                                                                                                                                                                                          Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                                                                                                                                                                                                                                                                                           |
|                       /u <Domain>\\<User>                        |                                                                                                                                                                                                                                                                 Executa o comando com as permissões de conta do usuário especificado por <User> ou <Domain>\\<User>. O padrão é as permissões do usuário conectado no momento no computador que emite o comando.                                                                                                                                                                                                                                                                  |
|                          /p <Password>                           |                                                                                                                                                                                                                                                                                                                         Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                                                                                                                                                                                                                                                                                         |
| /Port {COM1 &#124; COM2 &#124; com3 &#124; &#124; BIOSSET}  |                                                                                                                                                                                                                              Especifica a porta COM a ser usada para redirecionamento. O **BIOSSET** direciona os serviços de gerenciamento de emergência para obter as configurações do BIOS para determinar qual porta deve ser usada para redirecionamento. Não use o parâmetro **/Port** se a saída administrada remotamente estiver sendo desabilitada.                                                                                                                                                                                                                              |
| /baud {9600 &#124; 19200 &#124; 38400&#124; 57600 &#124; 115200} |                                                                                                                                                                                                                                                                                               Especifica a taxa de transmissão a ser usada para redirecionamento. Não use o parâmetro **/baud** se a saída administrada remotamente estiver sendo desabilitada.                                                                                                                                                                                                                                                                                               |
|                       /ID <OSEntryLineNum>                       |                                                                                                                                                                                              Especifica o número de linha da entrada do sistema operacional para o qual a opção de serviços de gerenciamento de emergência é adicionada na seção [Operating Systems] do arquivo boot. ini. A primeira linha após o cabeçalho da seção [Operating Systems] é 1. Esse parâmetro é necessário quando o valor dos serviços de gerenciamento de emergência é definido como **ativado** ou **desativado**.                                                                                                                                                                                              |
|                                /?                                |                                                                                                                                                                                                                                                                                                                                                 Exibe a ajuda no prompt de comando.                                                                                                                                                                                                                                                                                                                                                  |

## <a name="BKMK_examples"></a>Disso
Os exemplos a seguir mostram como você pode usar o comando **Bootcfg/EMS** :
```
bootcfg /ems on /port com1 /baud 19200 /id 2 
bootcfg /ems on /port biosset /id 3 
bootcfg /s srvmain /ems off /id 2 
bootcfg /ems edit /port com2 /baud 115200 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /ems off /id 2
```
#### <a name="additional-references"></a>referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
