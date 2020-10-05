---
title: takeown
description: Artigo de referência para o comando takeown, que permite que um administrador recupere o acesso a um arquivo que foi negado anteriormente.
ms.topic: reference
ms.assetid: 0683cd65-a6db-4cab-962b-45a0ff61f43c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 34fce5000a0a8c91123a2ebd4765bf4cbb00a289
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718233"
---
# <a name="takeown"></a>takeown

Permite que um administrador recupere o acesso a um arquivo que foi negado anteriormente, transformando-o no proprietário do arquivo. Esse comando é normalmente usado em arquivos em lotes.

## <a name="syntax"></a>Sintaxe

```
takeown [/s <computer> [/u [<domain>\]<username> [/p [<password>]]]] /f <filename> [/a] [/r [/d {Y|N}]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /s `<computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O valor padrão é o computador local. Esse parâmetro se aplica a todos os arquivos e pastas especificados no comando. |
| /u `[<domain>\]<username>` | Executa o script com as permissões da conta de usuário especificada. O valor padrão é permissões do sistema. |
| /p `[<[password>]` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . |
| /f `<filename>` | Especifica o nome do arquivo ou padrão de nome de diretório. Você pode usar o caractere curinga `*` ao especificar o padrão. Você também pode usar a sintaxe `<sharename>\<filename>` . |
| /a | Concede a propriedade ao grupo de administradores em vez do usuário atual. Se você não especificar essa opção, a propriedade do arquivo será dada ao usuário que está conectado ao computador no momento. |
| /r | Executa uma operação recursiva em todos os arquivos no diretório e nos subdiretórios especificados. |
| /d `{Y | N}` | Suprime o prompt de confirmação exibido quando o usuário atual não tem a permissão **Listar pasta** em um diretório especificado e, em vez disso, usa o valor padrão especificado. Os valores válidos para a opção **/d** são:<ul><li>**Y** -apropriar-se do diretório.</li><li>**N** -ignorar o diretório.<p>**OBSERVAÇÃO**<br>Você deve usar essa opção em conjunto com a opção **/r** .</li></ul> |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="remarks"></a>Comentários

- Padrões mistos usando (**?** e **&#42;**) não tem suporte pelo comando **takeown** .

- Depois de excluir o bloqueio com **takeown**, talvez seja necessário usar o Windows Explorer para ter permissões completas para os arquivos e diretórios antes de poder excluí-los.

## <a name="examples"></a>Exemplos

Para apropriar-se de um *arquivo chamado*renomeado, digite:

```
takeown /f lostfile
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
