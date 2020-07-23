---
title: mqbkup
description: Artigo de referência para o comando Mqbkup, que faz backup de arquivos de mensagens do MSMQ e configurações do registro para um dispositivo de armazenamento e restaura as mensagens e configurações armazenadas anteriormente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bdd41c4-75ef-455f-b241-1d64a4c7acf5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 00518ab36f1886ccb3a1221a065715668fb02f47
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86956918"
---
# <a name="mqbkup"></a>mqbkup

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Faz backup de arquivos de mensagens do MSMQ e configurações do registro em um dispositivo de armazenamento e restaura as mensagens e configurações armazenadas anteriormente.

As operações de backup e restauração param o serviço MSMQ local. Se o serviço MSMQ foi iniciado com antecedência, o utilitário tentará reiniciar o serviço MSMQ no final do backup ou na operação de restauração. Se o serviço já tiver sido interrompido antes da execução do utilitário, nenhuma tentativa de reiniciar o serviço será feita.

Antes de usar o utilitário de backup/restauração de mensagens MSMQ, você deve fechar todos os aplicativos locais que estão usando o MSMQ.

## <a name="syntax"></a>Sintaxe

```
mqbkup {/b | /r} <folder path_to_storage_device>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| /b | Especifica a operação de backup. |
| /r | Especifica a operação de restauração. |
| `<folder path_to_storage_device>` | Especifica o caminho onde os arquivos de mensagem MSMQ e as configurações do registro são armazenados. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Se uma pasta especificada não existir durante a execução da operação de backup ou restauração, a pasta será criada automaticamente pelo utilitário.

- Se você optar por especificar uma pasta existente, ela deverá estar vazia. Se você especificar uma pasta não vazia, o utilitário excluirá todos os arquivos e subpastas contidos nela. Nesse caso, você será solicitado a conceder permissão para excluir arquivos e subpastas existentes. Você pode usar o parâmetro **/y** para indicar que concorda antecipadamente com a exclusão de todos os arquivos e subpastas existentes na pasta especificada.

- Os locais de pastas usados para armazenar arquivos de mensagens MSMQ são armazenados no registro. Portanto, o utilitário restaura os arquivos de mensagem do MSMQ para as pastas especificadas no registro e não para as pastas de armazenamento usadas antes da operação de restauração.

### <a name="examples"></a>Exemplos

Para fazer backup de todos os arquivos de mensagens MSMQ e configurações do registro e armazená-los na pasta *msmqbkup* na unidade C:, digite:

```
mqbkup /b c:\msmqbkup
```

Para excluir todos os arquivos e subpastas existentes na pasta *oldbkup* na unidade C: e, em seguida, armazenar os arquivos de mensagem do MSMQ e as configurações do registro na pasta, digite:

```
mqbkup /b /y c:\oldbkup
```

Para restaurar mensagens MSMQ e configurações do registro, digite:

```
mqbkup /r c:\msmqbkup
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência do PowerShell do MSMQ](/powershell/module/msmq/?view=win10-ps)
