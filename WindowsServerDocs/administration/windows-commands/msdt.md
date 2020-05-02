---
title: msdt
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5d31b1b5a73d975aec08d675aaff04ee29c7d3c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723871"
---
# <a name="msdt"></a>msdt



Invoca um pacote de solução de problemas na linha de comando ou como parte de um script automatizado e habilita opções adicionais sem entrada do usuário.

## <a name="syntax"></a>Sintaxe

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

### <a name="parameters"></a>Parâmetros

A tabela a seguir inclui os parâmetros e as opções com suporte do MSDT. exe.


|      Parâmetro      |                                                                                            Descrição                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /ID \<nome do pacote> |        Especifica qual pacote de diagnóstico deve ser executado. Para obter uma lista de pacotes disponíveis, consulte a ID do pacote de solução de problemas na seção pacotes de solução de problemas disponíveis mais adiante neste tópico.         |
|  diretório \</Path  |                                                                                           arquivo. diagpkg                                                                                            |
|   > \<de chave de acesso/DCI   |                                        Popula o campo de chave de acesso na MSDT. Esse parâmetro só é usado quando um provedor de suporte forneceu uma chave de acesso.                                         |
|  > \<do diretório/DT   | Exibe o histórico de solução de problemas no diretório especificado. Os resultados de diagnóstico são armazenados nos diretórios **%LocalAppData%\Diagnostics** ou **%LocalAppData%\ElevatedDiagnostics** do usuário. |
| > \<do arquivo de resposta/AF  |                                               Especifica um arquivo de resposta em formato XML que contém respostas a uma ou mais interações de diagnóstico.                                               |

