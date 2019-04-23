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
ms.openlocfilehash: 080836e406b329cf8c15f95ef6afc99973bb3e4d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853087"
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

|Parâmetro|Descrição|
|---------|-----------|
|/Show:{Config | Imagens | Todos os}|Especifica o tipo de informação a ser retornada.</br>-   **Config** retorna informações de configuração do servidor.</br>-   **Imagens** retorna informações sobre grupos de imagens, imagens de inicialização e imagens de instalação no servidor.</br>-   **Todos os** retorna informações de configuração e a imagem do servidor.|
|[/Detailed]|Quando usado em conjunto com o **/Show:Images** ou **/Show:All**, retorna todos os metadados de cada imagem de imagem. Se o **/detalhadas** opção não for especificada, o comportamento padrão é retornar o nome da imagem, descrição e nome de arquivo.|
|[/Forest:{Yes | No}]|Especifica se é retornar informações para toda a floresta ou domínio local. Se um valor para essa opção não for especificado, o comportamento padrão é retornar os servidores no domínio local.|

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

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)