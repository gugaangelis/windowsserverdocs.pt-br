---
title: Get-myservers
description: Artigo de referência para Get-myservers, que recupera informações sobre todos os servidores de serviços de implantação do Windows.
ms.topic: reference
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b60fb7710699c4fff6656a0e2a34684a538b116d
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729658"
---
# <a name="get-allservers"></a>Get-myservers

Recupera informações sobre todos os servidores dos serviços de implantação do Windows.

> [!NOTE]
> Esse comando pode levar um tempo estendido para ser concluído se houver muitos servidores de serviços de implantação do Windows em seu ambiente ou se a conexão de rede vinculando os servidores estiver lenta.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
```

### <a name="parameters"></a>Parâmetros

|   Parâmetro   |                                                                                                                 Descrição                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Show: {config |                                                                                                                    Imagens                                                                                                                    |
|  [/Detailed]  | Quando usado em conjunto com **/show: images** ou **/show: ALL**, retorna todos os metadados de imagem de cada imagem. Se a opção **/detailed** não for especificada, o comportamento padrão será retornar o nome da imagem, a descrição e o nome do arquivo. |
| [/Forest: {Sim |                                                                                                                     Não}]                                                                                                                     |

## <a name="examples"></a>Exemplos

Para exibir informações sobre todos os servidores, digite:
```
WDSUTIL /Get-AllServers /Show:Config
```
Para exibir informações detalhadas sobre todos os servidores, digite:
```
WDSUTIL /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)