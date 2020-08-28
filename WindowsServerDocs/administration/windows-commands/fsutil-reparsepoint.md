---
title: fsutil reparsepoint
description: Artigo de referência para o comando fsutil reparsepoint, que consulta ou exclui pontos de nova análise.
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: fb95c8ee-a418-4520-a12a-7754ae947c3c
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: 94feb56f4c5ed590c87b20d475f4aefa7ba11dd4
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037314"
---
# <a name="fsutil-reparsepoint"></a>fsutil reparsepoint

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Consulta ou exclui pontos de nova análise.  O comando **fsutil reparsepoint** normalmente é usado por profissionais de suporte.

Pontos de nova análise são objetos do sistema de arquivos NTFS que têm um atributo definíveis, que contém dados definidos pelo usuário. Eles são usados para:

- Estenda a funcionalidade no subsistema de entrada/saída (e/s).

- Agir como pontos de junção de diretório e pontos de montagem de volume.

- Marque determinados arquivos como especiais para um driver de filtro do sistema de arquivos.

## <a name="syntax"></a>Sintaxe

```
fsutil reparsepoint [query] <filename>
fsutil reparsepoint [delete] <filename>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| Consulta | Recupera os dados do ponto de nova análise que estão associados ao arquivo ou diretório identificado pelo identificador especificado. |
| excluir | Exclui um ponto de nova análise do arquivo ou diretório que é identificado pelo identificador especificado, mas não exclui o arquivo ou diretório. |
| `<filename>` | Especifica o caminho completo para o arquivo, incluindo o nome do arquivo e a extensão, por exemplo *C:\documents\filename.txt*. |

#### <a name="remarks"></a>Comentários

- Quando um programa define um ponto de nova análise, ele armazena esses dados, além de uma marca de nova análise, que identifica exclusivamente os dados que estão sendo armazenados. Quando o sistema de arquivos abre um arquivo com um ponto de nova análise, ele tenta localizar o filtro do sistema de arquivos associado. Se o filtro do sistema de arquivos for encontrado, o filtro processará o arquivo conforme direcionado pelos dados de nova análise. Se nenhum filtro do sistema de arquivos for encontrado, a operação de **abertura de arquivo** falhará.

### <a name="examples"></a>Exemplos

Para recuperar os dados do ponto de nova análise associados ao *c:\Server*, digite:

```
fsutil reparsepoint query c:\server
```

Para excluir um ponto de nova análise de um arquivo ou diretório especificado, use o seguinte formato:

```
fsutil reparsepoint delete c:\server
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
