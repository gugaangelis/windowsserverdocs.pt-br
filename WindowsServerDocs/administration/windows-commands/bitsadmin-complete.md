---
title: bitsadmin complete
description: O tópico de comandos do Windows para **Bitsadmin concluído**, que conclui o trabalho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 847f6e298ff9701064ce4e577c785f7fc78ea22c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850819"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

Conclui o trabalho. Os arquivos baixados não estarão disponíveis até você até que você use essa opção. Use essa opção depois que o trabalho for movido para o estado transferido. Caso contrário, somente os arquivos que foram transferidos com êxito estarão disponíveis.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /complete <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Quando o estado do trabalho for transferido, o BITS transferiu com êxito todos os arquivos no trabalho. No entanto, os arquivos não estarão disponíveis até que você use a opção **/Complete** 

Se vários trabalhos usarem *myDownloadJob* como seu nome, você deverá substituir *myDownloadJob* pelo GUID do trabalho para identificar exclusivamente o trabalho.

```
C:\>bitsadmin /complete myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)