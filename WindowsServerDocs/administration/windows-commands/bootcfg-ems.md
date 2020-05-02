---
title: bootcfg ems
description: Tópico de referência para o comando Bootcfg EMS, que permite ao usuário adicionar ou alterar as configurações de redirecionamento do console dos serviços de gerenciamento de emergência para um computador remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57abdc50-c64a-45f1-8470-3f8c3a51f743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c85231e094852feb673eb4f99b183f06014234b2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709518"
---
# <a name="bootcfg-ems"></a>bootcfg ems

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que o usuário adicione ou altere as configurações de redirecionamento do console dos serviços de gerenciamento de emergência para um computador remoto. Habilitar os serviços de gerenciamento de emergência `redirect=Port#` , adiciona uma linha à seção [boot loader] do arquivo boot. ini junto com uma opção/Redirect à linha de entrada do sistema operacional especificada. O recurso serviços de gerenciamento de emergência é habilitado somente em servidores.

## <a name="syntax"></a>Sintaxe

```
bootcfg /ems {on | off | edit}[/s <computer> [/u <domain>\<user> /p <password>]] [/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <osentrylinenum>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `{on | off | edit}` | Especifica o valor para redirecionamento de serviços de gerenciamento de emergência, incluindo:<ul><li>**no.** Habilita a saída remota para o `<osentrylinenum>`especificado. Também adiciona uma opção/redirect ao especificado <osentrylinenum> e uma `redirect=com<X>` configuração à seção [boot loader]. O valor de `com<X>` é definido pelo parâmetro **/Port** .</li><li>**desconto.** Desabilita a saída para um computador remoto. Também remove a opção/Redirect para o especificado <osentrylinenum> e a `redirect=com<X>` configuração da seção [boot loader].</li><li>**editar.** Permite alterações nas configurações de porta alterando `redirect=com<X>` a configuração na seção [carregador de inicialização]. O valor de `com<X>` é definido pelo parâmetro **/Port** .</li></ul> |
| `/s <computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. |
| `/u <domain>\<user>`  | Executa o comando com as permissões de conta do usuário especificado por `<user>` ou `<domain>\<user>`. O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
| `/p <password>` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . |
| `/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}` |  Especifica a porta COM a ser usada para redirecionamento. O parâmetro BIOSset direciona os serviços de gerenciamento de emergência para obter as configurações do BIOS para determinar qual porta deve ser usada para redirecionamento. Não use esse parâmetro se a saída administrada remotamente estiver desabilitada. |
| `/baud {9600 | 19200 | 38400 | 57600 | 115200}` | Especifica a taxa de transmissão a ser usada para redirecionamento. Não use esse parâmetro se a saída administrada remotamente estiver desabilitada. |
| `/id <osentrylinenum>` | Especifica o número de linha da entrada do sistema operacional para o qual a opção de serviços de gerenciamento de emergência é adicionada na seção [Operating Systems] do arquivo boot. ini. A primeira linha após o cabeçalho da seção [Operating Systems] é 1. Esse parâmetro é necessário quando o valor dos serviços de gerenciamento de emergência é definido como **ativado** ou **desativado**. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para usar o comando **Bootcfg/EMS** :

```
bootcfg /ems on /port com1 /baud 19200 /id 2
bootcfg /ems on /port biosset /id 3
bootcfg /s srvmain /ems off /id 2
bootcfg /ems edit /port com2 /baud 115200
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /ems off /id 2
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bootcfg](bootcfg.md)
