---
title: msdt
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c4342cf0fe588f5b41cd98a38a459ac41ca212a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373449"
---
# <a name="msdt"></a>msdt



Invoca um pacote de solução de problemas na linha de comando ou como parte de um script automatizado e habilita opções adicionais sem entrada do usuário.

## <a name="syntax"></a>Sintaxe

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

## <a name="parameters"></a>Parâmetros

A tabela a seguir inclui os parâmetros e as opções com suporte do MSDT. exe.


|      Parâmetro      |                                                                                            Descrição                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /ID \<nome do pacote > |        Especifica qual pacote de diagnóstico deve ser executado. Para obter uma lista de pacotes disponíveis, consulte a ID do pacote de solução de problemas na seção "pacotes de solução de problemas disponíveis" mais adiante neste tópico.         |
|  diretório \</Path  |                                                                                           arquivo. diagpkg                                                                                            |
|   > \<de chave de acesso/DCI   |                                        Popula o campo de chave de acesso na MSDT. Esse parâmetro só é usado quando um provedor de suporte forneceu uma chave de acesso.                                         |
|  > \<do diretório/DT   | Exibe o histórico de solução de problemas no diretório especificado. Os resultados de diagnóstico são armazenados nos diretórios **%LocalAppData%\Diagnostics** ou **%LocalAppData%\ElevatedDiagnostics** do usuário. |
| > \<do arquivo de resposta/AF  |                                               Especifica um arquivo de resposta em formato XML que contém respostas a uma ou mais interações de diagnóstico.                                               |

