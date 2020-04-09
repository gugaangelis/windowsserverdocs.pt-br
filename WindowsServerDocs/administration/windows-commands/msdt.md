---
title: msdt
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87c1e4e8cd6d9de036b47de590867a6531d0335a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839249"
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
| /ID \<nome do pacote > |        Especifica qual pacote de diagnóstico deve ser executado. Para obter uma lista de pacotes disponíveis, consulte a ID do pacote de solução de problemas na seção pacotes de solução de problemas disponíveis mais adiante neste tópico.         |
|  /Path \<Directory  |                                                                                           arquivo. diagpkg                                                                                            |
|   /DCI \<chave de acesso >   |                                        Popula o campo de chave de acesso na MSDT. Esse parâmetro só é usado quando um provedor de suporte forneceu uma chave de acesso.                                         |
|  /DT \<Directory >   | Exibe o histórico de solução de problemas no diretório especificado. Os resultados de diagnóstico são armazenados nos diretórios **%LocalAppData%\Diagnostics** ou **%LocalAppData%\ElevatedDiagnostics** do usuário. |
| /AF \<arquivo de resposta >  |                                               Especifica um arquivo de resposta em formato XML que contém respostas a uma ou mais interações de diagnóstico.                                               |

