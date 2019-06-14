---
title: msdt
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ba411cf73026afe9990e5c32824e3dc277507891
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437233"
---
# <a name="msdt"></a>msdt



Invoca um pacote de solução de problemas na linha de comando ou como parte de um script automatizado e permite que as opções adicionais sem entrada do usuário.

## <a name="syntax"></a>Sintaxe

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

## <a name="parameters"></a>Parâmetros

A tabela a seguir inclui os parâmetros e opções com suporte pelo msdt.exe.


|      Parâmetro      |                                                                                            Descrição                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /ID \<nome do pacote > |        Especifica qual pacote de diagnóstico será executado. Para obter uma lista de pacotes disponíveis, consulte a identificação do pacote de solução de problemas na "disponível pacotes de solução de problemas? seção mais adiante neste tópico.         |
|  /Path \<diretório  |                                                                                           arquivo .diagpkg                                                                                            |
|   /dci \<passkey>   |                                        Predefine o campo de chave de acesso no msdt. Esse parâmetro é usado somente quando um provedor de suporte tiver fornecido uma chave de acesso.                                         |
|  /dt \<directory>   | Exibe o histórico de solução de problemas no diretório especificado. Resultados do diagnóstico são armazenados do usuário **%LOCALAPPDATA%\Diagnostics** ou **%LOCALAPPDATA%\ElevatedDiagnostics** diretórios. |
| /af \<answer file>  |                                               Especifica um arquivo de resposta no formato XML que contém as respostas às interações do diagnóstico de um ou mais.                                               |

