---
title: netsh
description: Artigo de referência para o comando netsh, que é um utilitário de script de linha de comando que permite que você, local ou remotamente, exiba ou modifique a configuração de rede de um computador atualmente em execução.
ms.topic: article
ms.assetid: 96fc069d-53c0-4d0a-9f7f-f9f3d49a02bd carmonmills
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 366c7a21f44dc6545de7ba81cba8fe152c245b6b
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886036"
---
# <a name="netsh"></a>netsh

> Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

O utilitário de script de linha de comando do Shell de rede que permite que você, local ou remotamente, exiba ou modifique a configuração de rede de um computador em execução no momento. Você pode iniciar esse utilitário no prompt de comando ou no Windows PowerShell.

## <a name="syntax"></a>Sintaxe

```
netsh [-a <Aliasfile>][-c <Context>][-r <Remotecomputer>][-u [<domainname>\<username>][-p <Password> | [{<NetshCommand> | -f <scriptfile>}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -a`<Aliasfile>` | Especifica que você retorna ao prompt netsh após a execução de Aliasfile e o nome do arquivo de texto que contém um ou mais comandos netsh. |
| -c`<Context>` | Especifica que o netsh insere o contexto netsh especificado e o contexto netsh a ser inserido. |
| -r`<Remotecomputer>` | Especifica o computador remoto a ser configurado.<p>**Importante:** Se você usar esse parâmetro, deverá verificar se o serviço de registro remoto está em execução no computador remoto. Se não estiver em execução, o Windows exibirá uma mensagem de erro "caminho de rede não encontrado". |
| -u`<domainname>\<username>` | Especifica o nome de domínio e de conta de usuário a ser usado ao executar o comando netsh em uma conta de usuário. Se você omitir o domínio, o domínio local será usado por padrão. |
| -p`<Password>` | Especifica a senha para a conta de usuário especificada pelo `-u <username>` parâmetro. |
| `<NetshCommand>` | Especifica o comando netsh a ser executado. |
| -f`<scriptfile>` | Sai do comando netsh depois de executar o arquivo de script especificado. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Se você especificar **-r** seguido por outro comando, o netsh executará o comando no computador remoto e retornará ao prompt de comando do Cmd.exe. Se você especificar **-r** sem outro comando, o netsh será aberto no modo remoto. O processo é semelhante ao uso de **set machine** no prompt de comando Netsh. Ao usar **-r**, você define o computador de destino somente para a instância atual do netsh. Depois de sair e entrar novamente no netsh, o computador de destino será redefinido como o computador local. Você pode executar comandos netsh em um computador remoto especificando um nome do computador armazenado no WINS, um nome UNC, um nome de Internet a ser resolvido pelo servidor DNS ou um endereço IP.

- Se o valor da cadeia de caracteres contiver espaços entre caracteres, você deverá colocar o valor da cadeia de caracteres entre aspas. Por exemplo, `-r "contoso remote device"`

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
