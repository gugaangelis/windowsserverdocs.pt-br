---
title: bitsadmin getnotifycmdline
description: Tópico de referência para o comando Bitsadmin getnotifycmdline, que recupera o comando de linha de comando que é executado quando o trabalho termina de transferência de dados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90fa33e6-aca5-4a23-82bd-19a9f13f8416
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d4c47c4a1b9ea06fd804c8f2c48e9ac0ce1b319
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717794"
---
# <a name="bitsadmin-getnotifycmdline"></a>bitsadmin getnotifycmdline

Recupera o comando de linha de comando a ser executado depois que o trabalho especificado terminar de transferir dados.

> [!NOTE]
> Esse comando não tem suporte no BITS 1,2 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getnotifycmdline <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o comando de linha de comando usado pelo serviço quando o trabalho chamado *myDownloadJob* for concluído.

```
bitsadmin /getnotifycmdline myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
