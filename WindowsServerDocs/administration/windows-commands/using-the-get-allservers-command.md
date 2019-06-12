---
title: Usando o comando get-AllServers
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbccb834f9058f2c3cca097cdf998455f2a6892e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440490"
---
# <a name="using-the-get-allservers-command"></a>Usando o comando get-AllServers



Recupera informações sobre todos os servidores de serviços de implantação do Windows.

> [!NOTE]
> Esse comando pode demorar um longo período de tempo para ser concluída se houver muitos servidores de serviços de implantação do Windows em seu ambiente ou se a conexão de rede os servidores de vinculação é lenta.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
```

## <a name="parameters"></a>Parâmetros

|   Parâmetro   |                                                                                                                 Descrição                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Show:{Config |                                                                                                                    Imagens                                                                                                                    |
|  [/Detailed]  | Quando usado em conjunto com o **/Show:Images** ou **/Show:All**, retorna todos os metadados de cada imagem de imagem. Se o **/detalhadas** opção não for especificada, o comportamento padrão é retornar o nome da imagem, descrição e nome de arquivo. |
| [/Forest:{Yes |                                                                                                                     No}]                                                                                                                     |

## <a name="BKMK_examples"></a>Exemplos

Para exibir informações sobre todos os servidores, digite:
```
WDSUTIL /Get-AllServers /Show:Config
```
Para exibir informações detalhadas sobre todos os servidores, digite:
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)