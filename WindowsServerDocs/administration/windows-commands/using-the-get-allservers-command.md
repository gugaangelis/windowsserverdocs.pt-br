---
title: Get-myservers
description: Artigo de referência para Get-myservers, que recupera informações sobre todos os servidores de serviços de implantação do Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe2e3c69-8f2e-457d-af55-d249ebf70f53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a02515b138c9db6a1d320a4ad466700c15b84749
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935058"
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