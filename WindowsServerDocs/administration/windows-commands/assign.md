---
title: assign
description: Tópico de comandos do Windows para **atribuir** -atribui um ponto de montagem ou letra de unidade ao volume com foco.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57912b73-622e-489b-b053-a369021ba8e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e7f680fe93e846f5b916cf3210a7ca61f190674
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435305"
---
# <a name="assign"></a>assign

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

atribui um ponto de montagem ou letra de unidade ao volume com foco.

## <a name="syntax"></a>Sintaxe
```
assign [{letter=<d> | mount=<path>}] [noerr]
```
## <a name="parameters"></a>Parâmetros

|  Parâmetro   |                                                                                                                                 Descrição                                                                                                                                 |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  letter=<d>  |                                                                                                             A letra de unidade que você deseja atribuir ao volume.                                                                                                              |
| mount=<path> | O caminho do ponto de montagem que deseja atribuir ao volume.<br /><br />Para obter instruções sobre como usar esse comando, consulte [atribua um caminho de pasta do ponto de montagem para uma unidade](https://go.microsoft.com/fwlink/?LinkId=207059) (<https://go.microsoft.com/fwlink/?LinkId=207059>). |
|    noerr     |                                    Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.                                     |

## <a name="remarks"></a>Comentários
- Se nenhum ponto de montagem ou letra de unidade for especificado, a próxima letra de unidade disponível será atribuída. Se o ponto de montagem ou letra de unidade já está em uso, um erro será gerado.
- Usando o comando assign, você pode alterar a letra da unidade associada a uma unidade removível.
- É possível atribuir letras de unidade para volumes do sistema, os volumes de inicialização ou volumes que contêm o arquivo de paginação. Além disso, você não pode atribuir uma letra de unidade para uma partição Original Equipment Manufacturer (OEM) ou qualquer partição de tabela de partição GUID (gpt) que não seja uma partição de dados básica.
- Um volume deve ser selecionado para essa operação seja bem-sucedida. Use o **selecionar volume** comando para selecionar um volume e mudar o foco a ele.
  ## <a name="BKMK_examples"></a>Exemplos
  Para atribuir a letra E para o volume de em foco, digite:
  ```
  assign letter=e
  ```

