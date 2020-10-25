---
title: Get-myservers
description: Artigo de referência para Get-myservers, que recupera informações sobre todos os servidores de serviços de implantação do Windows.
ms.topic: reference
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 450c864bef3b3f17f3912a06aa72aa56ce6e529a
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524441"
---
# <a name="get-allservers"></a>Get-myservers

Recupera informações sobre todos os servidores dos serviços de implantação do Windows.

> [!NOTE]
> Esse comando pode levar um tempo estendido para ser concluído se houver muitos servidores de serviços de implantação do Windows em seu ambiente ou se a conexão de rede vinculando os servidores estiver lenta.

## <a name="syntax"></a>Sintaxe

```
wdsutil [Options] /Get-AllServers /Show:{Config | Images | All} [/Detailed] [/Forest:{Yes | No}]
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
wdsutil /Get-AllServers /Show:Config
```
Para exibir informações detalhadas sobre todos os servidores, digite:
```
wdsutil /Verbose /Get-AllServers /Show:All /Detailed /Forest:Yes
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)