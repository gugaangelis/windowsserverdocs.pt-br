---
title: fsutil objectid
description: Artigo de referência para o comando fsutil ObjectID, que gerencia identificadores de objeto para controlar outros objetos, como arquivos, diretórios e links.
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 693ab895-9d0c-47c1-9f52-df5cd287842a
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: 27b7048ebb659c29bd6aa7d41c0be9b26deca547
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037334"
---
# <a name="fsutil-objectid"></a>fsutil objectid

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Gerencia identificadores de objeto (OIDs), que são objetos internos usados pelo serviço de cliente de rastreamento de link distribuído (DLT) e serviço de replicação de arquivo (FRS), para acompanhar outros objetos, como arquivos, diretórios e links. Os identificadores de objeto são invisíveis para a maioria dos programas e nunca devem ser modificados.

> [!WARNING]
> Não exclua, defina ou modifique um identificador de objeto. A exclusão ou a definição de um identificador de objeto pode resultar na perda de dados de partes de um arquivo, até e incluindo volumes inteiros de dados. Além disso, você pode causar um comportamento adverso no serviço de cliente DLT (rastreamento de link distribuído) e no FRS (serviço de replicação de arquivo).

## <a name="syntax"></a>Sintaxe

```
fsutil objectid [create] <filename>
fsutil objectid [delete] <filename>
fsutil objectid [query] <filename>
fsutil objectid [set] <objectID> <birthvolumeID> <birthobjectID> <domainID> <filename>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| create | Cria um identificador de objeto se o arquivo especificado ainda não tiver um. Se o arquivo já tiver um identificador de objeto, esse subcomando será equivalente ao subcomando de **consulta** . |
| excluir | Exclui um identificador de objeto. |
| Consulta | Consulta um identificador de objeto. |
| set | Define um identificador de objeto. |
| `<objectID>` | Define um identificador hexadecimal de 16 bytes específico de arquivo que é garantido como exclusivo em um volume. O identificador de objeto é usado pelo serviço de cliente DLT (rastreamento de link distribuído) e pelo FRS (serviço de replicação de arquivo) para identificar arquivos. |
| `<birthvolumeID>` | Indica o volume no qual o arquivo foi localizado quando ele obteve pela primeira vez um identificador de objeto. Esse valor é um identificador hexadecimal de 16 bytes que é usado pelo serviço do cliente DLT. |
| `<birthobjectID>` | Indica o identificador de objeto original do arquivo (o *ObjectID* pode ser alterado quando um arquivo é movido). Esse valor é um identificador hexadecimal de 16 bytes que é usado pelo serviço do cliente DLT. |
| `<domainID>` | identificador de domínio hexadecimal de 16 bytes. Esse valor não é usado no momento e deve ser definido como todos os zeros. |
| `<filename>` | Especifica o caminho completo para o arquivo, incluindo o nome do arquivo e a extensão, por exemplo *C:\documents\filename.txt*. |

#### <a name="remarks"></a>Comentários

- Qualquer arquivo que tenha um identificador de objeto também tem um identificador de volume de nascimento, um identificador de objeto de nascimento e um identificador de domínio. Quando você move um arquivo, o identificador de objeto pode ser alterado, mas o volume de nascimento e os identificadores de objeto de nascimento permanecem os mesmos. Esse comportamento permite que o sistema operacional Windows sempre encontre um arquivo, independentemente de onde ele foi movido.

### <a name="examples"></a>Exemplos

Para criar um identificador de objeto, digite:

`fsutil objectid create c:\temp\sample.txt`

Para excluir um identificador de objeto, digite:

`fsutil objectid delete c:\temp\sample.txt`

Para consultar um identificador de objeto, digite:

`fsutil objectid query c:\temp\sample.txt`

Para definir um identificador de objeto, digite:

`fsutil objectid set 40dff02fc9b4d4118f120090273fa9fc f86ad6865fe8d21183910008c709d19e 40dff02fc9b4d4118f120090273fa9fc 00000000000000000000000000000000 c:\temp\sample.txt`

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
