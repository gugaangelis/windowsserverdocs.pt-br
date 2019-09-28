---
title: assign
description: O tópico de comandos do Windows para **atribuir atribui** uma letra de unidade ou ponto de montagem ao volume com foco.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3e07edcd4ac4ddf5eca1e57da17df441043d15f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382681"
---
# <a name="assign"></a>assign

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

atribui uma letra de unidade ou ponto de montagem ao volume com foco.

## <a name="syntax"></a>Sintaxe
```
assign [{letter=<d> | mount=<path>}] [noerr]
```
## <a name="parameters"></a>Parâmetros

|  Parâmetro   |                                                                                                                                 Descrição                                                                                                                                 |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  letra = <d>  |                                                                                                             A letra da unidade que você deseja atribuir ao volume.                                                                                                              |
| Mount = <path> | O caminho do ponto de montagem que você deseja atribuir ao volume.<br /><br />para obter instruções sobre como usar esse comando, consulte [atribuir um caminho de pasta de ponto de montagem a uma unidade](https://go.microsoft.com/fwlink/?LinkId=207059) (<https://go.microsoft.com/fwlink/?LinkId=207059>). |
|    NOERR     |                                    Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.                                     |

## <a name="remarks"></a>Comentários
- Se nenhuma letra de unidade ou ponto de montagem for especificado, a próxima letra de unidade disponível será atribuída. Se a letra da unidade ou o ponto de montagem já estiver em uso, um erro será gerado.
- Usando o comando atribuir, você pode alterar a letra da unidade associada a uma unidade removível.
- Não é possível atribuir letras de unidade a volumes do sistema, volumes de inicialização ou volumes que contêm o arquivo de paginação. Além disso, não é possível atribuir uma letra da unidade a uma partição OEM (fabricante original do equipamento) ou a qualquer partição GPT (tabela de partição GUID) que não seja uma partição de dados básica.
- Um volume deve ser selecionado para que essa operação seja realizada com sucesso. Use o comando **selecionar volume** para selecionar um volume e deslocar o foco para ele.
  ## <a name="BKMK_examples"></a>Disso
  Para atribuir a letra E ao volume em foco, digite:
  ```
  assign letter=e
  ```

