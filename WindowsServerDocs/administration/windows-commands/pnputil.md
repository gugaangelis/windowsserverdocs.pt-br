---
title: pnputil
description: Tópico de referência para o comando pnputil, que adiciona pacotes de driver, remove pacotes de driver e lista pacotes de driver que estão no repositório de drivers, usando o utilitário pnputil.exe.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fab686b8-09d3-4f6c-afa2-630e6036f44c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6484a3e55c6e5f3b4cb51119ead5cb488dca0721
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472392"
---
# <a name="pnputil"></a>pnputil

Pnputil.exe é um utilitário de linha de comando que você pode usar para gerenciar o repositório de drivers. Você pode usar esse comando para adicionar pacotes de driver, remover pacotes de driver e listar pacotes de driver que estão na loja.

## <a name="syntax"></a>Sintaxe

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -a | Especifica a adição do arquivo INF identificado. |
| -d | Especifica para excluir o arquivo INF identificado. |
| -E | Especifica a enumeração de todos os arquivos INF de terceiros. |
| -f | Especifica a força da exclusão do arquivo INF identificado. Não pode ser usado em conjunto com o parâmetro **– i** . |
| -i | Especifica a instalação do arquivo INF identificado. Não pode ser usado em conjunto com o parâmetro **-f** . |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para adicionar um arquivo INF, chamado USBCAM. INF, digite:

```
pnputil.exe -a a:\usbcam\USBCAM.INF
```

Para adicionar todos os arquivos INF, localizados em c:\drivers, digite:

```
pnputil.exe -a c:\drivers\*.inf
```

Para adicionar e instalar o USBCAM. Driver INF, tipo:

```
pnputil.exe -i -a a:\usbcam\USBCAM.INF
```

Para enumerar todos os drivers de terceiros, digite:

```
pnputil.exe –e
```

Para excluir o arquivo INF e o driver denominado Oem0. inf, digite:

```
pnputil.exe -d oem0.inf
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando POPD](popd.md)
